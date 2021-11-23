## 前言

前面有说到公司的一个项目是如何模块化的，当时模块化一方面为了整理代码，另一个方面是为了兼容 `ie` 。因为我们的业务场景大多数是面向政府，医院 等企业 。下面是关于兼容中的一些处理应对，希望你用 **不** 上（`ie` 早点拜拜吧，算我求求你了OVO）。

## 语法转换

如果你知道 `babel` 那这一块你应该难不倒你，主要是相关的一些配置。使用 `babel` 进行对应的语法转换。`babel` 本身就是一个语法转换器，让你写最新的语法代码，然后通过他的转换，支持旧的浏览器。

举个例子：

```js
// 这是es2021的可选链操作符
cosnt age = userInfo?.age

// 箭头函数
const getName = () => {}
```
上面的代码在低版本的浏览器环境下跑肯定是会报错的，如果通过 `babel` 的语法转换，这个问题就解决了。我们依旧写优质的代码。转换出来的，你不需要去关心。而且这里也使用了 `es6` 的 `const`, 也会转换成 `var` 。同样的还有箭头函数也会转换成 `es5` 中的函数声明。

下面是相关的配置：

```js
// 这是ewebpack5，webpack-cli4.8，babel7的配置
{
    mode：'development',
    // ...其他的一些配置
    module: {
      rules: [
        {
          test: /\.m?js$/,
          exclude: /(node_modules|bower_components)/,
          use: {
            loader: "babel-loader",
            options: {
              presets: ["@babel/preset-env"],
            },
          },
        },
      ],
    },
}

```

> 注意：不同版本的 `webpack` 或 `gulp` 等打包工具 和 `babel` 配置可能不相同。同时，也会存在兼容性问题， 比如：高版本的 `babel` 可能在低版本的 `webpack` 上会报错。这个时候我们应当降低版本或升级到适配的版本。

## `api` 支持

有了上面的语法转换并不能保证你万无一失的在 `ie` 里面跑，当你使用到一些 `api` 时，还是会出现问题的，举一个简单的例子：

```js
const codeMap = [400, 403, 404, 500]

if (codeMap.includes(code)) return 'network error'
```

上面的例子使用了 `Array.propotype.includes` , 这对于只进行单纯的语法转换是不行的，这压根没法转。所以需要进行 `api` 层面的支持。这就需要提到 `@babel/polyfill` 了，他可以让你支持新的`api`，像 `promise` , `Set`, `Map` 这种就没什么大问题了。但是他同样带来的是代码体积的增加，这是无法避免的，当然了，我们也可以进行对应的按需加载，用到了就支持。这在 `babel` 的配置里面也是可配的。

还可以提到的一点是：`babel` 需要结合工程化来使用，如果你的项目并没有基于打包工具（`webpack`，`rollup`, `gulp`, `vite`等）进行模块化，工程化，应该是不适用的（个人觉得，如果能使用请联系修改）， 这时候 `cdn` 或许能够解决你的一些烦恼，`es6-shim` 也能支持一些 `api` 的兼容，[es6-shim传送门](https://www.npmjs.com/package/es6-shim)

## 其他

做好上面的这些点，应该能帮你解决绝大多数场景的支持了，下面还有一些需要关注了解的：

### `browserslist`

这也是一个比较重要的配置项，表示你要支持的浏览器版本。如果你不知道， 可以进行相关的了解。在 `package.json` 中这样配置：

```
{
    "scripts": {
        "dev": "webpack-dev-server --mode=development",
        "build": "webpack --mode=production",
    },
    ...
    "browserslist": [
        "ie >= 8"
    ]
}
```

### 插件

无论是 `webpack` 还是 `babel` 它们的生态都有着许多的插件，这也是你结合的一个方面，比如我就在 使用 `esm` 的 `import/export` 语法后，导致打包处理的代码会报 `Object.Object.defineProperty` 的问题， 这应该是 `ie` 中遇到的最多的问题了。查了一下，`vue` 利用的就是这个 `api` 来实现的双向数据绑定，这也是它不支持 `ie8` 的原因。在 `ie8` 中 `Object.defineProperty` 只支持 `DOM` 对象。所以就会带来问题。解决办法 应该有两种：1. 不使用 `es6` 的模块化规范，即 `import/export` 语法，转而使用 `amd` 的 `module.export/require`。但是如果你就是想写 `import/export` , 那么可能你需要借助插件的转换 [@babel/plugin-transform-modules-commonjs](https://babel.docschina.org/docs/en/babel-plugin-transform-modules-commonjs/)。这个插件能够帮助你。



