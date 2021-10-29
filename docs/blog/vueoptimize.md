## 一，代码层面的优化

1. 区分 `v-if` 和 `v-show`的使用场景

    `v-if` 是 真正 的条件渲染，它会形成真实的DOM 进行切换 ，不断的销毁和生成。也就是会照成 回流。

    `v-show` 就简单得多， 不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 `CSS` 的 `display` 属性进行切换。

2. `computed` 和 `watch` 区分使用场景

    computed： 是计算属性，依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值. 实际应用比如： 购物车价格计算。

    watch： 更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作。

3. v-for 遍历必须为 item 添加 key，且避免同时使用 v-if

    在列表数据进行遍历渲染时，采用就地复用策略来减少DOM 渲染，需要为每一项 `item` 设置唯一 `key` 值，避免使用 `index` 。 方便 `Vue.js` 内部机制精准找到该条列表数据。当 `state` 更新时，新的状态值和旧的状态值对比，较快地定位到 `diff` 。

    `v-for` 比 `v-if` 优先级高，如果每一次都需要遍历整个数组，将会影响速度，尤其是当之需要渲染很小一部分的时候，必要情况下应该替换成 `computed` 属性。

4. 长列表性能优化

    在我们列表展示的时候，往往数据是很少有变化的 ，使用我们不需要 `Vue` 来劫持列表变量。在大量数据展示的情况下，这能够很明显的减少组件初始化的时间.
    ```
    export default {
      data: () => ({
        users: {}
          }),
      async created() {
        const users = await axios.get("/api/users");
        this.users = Object.freeze(users);     // 重点代码
      }
    };
    ```

5. 事件的销毁和计算器的销毁

    当我们在 `Vue` 中使用 `addEventListener` 和 `$on`来监听事件的时候，我们需要在组件销毁时手动移除这些事件的监听，以免造成内存泄露。

    ```
    created() {
      addEventListener('click', this.click, false)
      $on("click",function (msg) {
          console.log(msg)
        })
    },
    beforeDestroy() {
      removeEventListener('click', this.click, false)
      $off("click")
    }
    ```
当然 我们也可以使用 $once 监听一个事件，但是只触发一次。一旦触发之后，监听器就会被移除。

6. 路由懒加载

    Vue 是单页面应用，可能会有很多的路由引入 ，这样使用 webpcak 打包后的文件很大，当进入首页时，加载的资源过多，页面会出现白屏的情况，不利于用户体验。使用我们只在用户访问时才去加载，这样就更加高效了。这样会大大提高首屏显示的速度。

    ```
    const Foo = () => import('./Foo.vue')
    const router = new VueRouter({
      routes: [
        { path: '/foo', component: Foo }
      ]
    })
    ```
7. 组件懒加载

    在 `Vue`中, 页面是由 一个个的组件构成的 , 当页面组件过多的时候，我们就需要用户只展示需要的组件，比如 当用户点击了才去加载一个弹窗组件来展示。

    ```
    export default {
        ...,
        components: {
            'TabBar': ()=>import("./TabBar");
        }
    }
    ```

 8. `keep-alive` 标签的使用

     当我们 在两个组件之间会频繁来回切换的情况下，这时候我们就要使用 `keep-alive` 来包裹 ，进行缓存。例如： 菜单栏等。

    ```
    <keep-alive>
      <comp-a v-if="a > 1"></comp-a>
      <comp-b v-else></comp-b>
    </keep-alive>
    ```

9. 取消打包生成 .map 文件

    在做vue项目打包 打包之后js中 会自动生成一些map文件。如果运行时报错，输出的错误信息无法准确得知是哪里的代码报错。有了map就可以像未加密的代码一样，准确的输出是哪一行哪一列有错。但在真实生产环境我们并不需要。有说法 map文件会造成项目源码泄漏。 所以我们在 `vue.config.js` 配置取消 生成。

    ```
   module.exports = {
      build: {
        productionSourceMap: false,
      }
    }
    ```

## 二, 借助第三方插件

10. 优化无限列表性能

    如果你的应用存在非常长或者无限滚动的列表，那么需要采用 窗口化 的技术来优化性能，只需要渲染少部分区域的内容，减少重新渲染组件和创建 dom 节点的时间。你可以参考以下开源项目 vue-virtual-scroll-list 和 vue-virtual-scroller 来优化这种无限列表的场景的。

11. 组件库的按需加载

    我们可以借助 babel-plugin-component ，然后可以只引入需要的组件，以达到减小项目体积的目的。以下为项目中引入 element-ui 组件库为例：

    1.1安装 `babel-plugin-component `
    ```
    npm install babel-plugin-component -D
    ```
    1.2 更改配置文件
    然后，将 .babelrc 修改为：

    ```
    {
      "presets": [["es2015", { "modules": false }]],
      "plugins": [
        [
          "component",
          {
            "libraryName": "element-ui",
            "styleLibraryName": "theme-chalk"
          }
        ]
      ]
    }
    ```
12. 使用服务端渲染

    服务端渲染是指 `Vue` 在客户端将标签渲染成的整个 `html` 片段的工作在服务端完成，服务端形成的 `html` 片段直接返回给客户端这个过程就叫做服务端渲染。在这过程中客户端中渲染， 数据已经在服务端拿到并且到了 `html`中了。

    * 服务端渲染的优点：

     1. 更好的 SEO：因为 SPA 页面的内容是通过 Ajax 获取，而搜索引擎爬取工具并不会等待 Ajax 异步完成后再抓取页面内容，所以在 SPA 中是抓取不到页面通过 Ajax 获取到的内容；而 SSR 是直接由服务端返回已经渲染好的页面（数据已经包含在页面中），所以搜索引擎爬取工具可以抓取渲染好的页面；

    2. 更快的内容到达时间（首屏加载更快）：SPA 会等待所有 Vue 编译后的 js 文件都下载完成后，才开始进行页面的渲染，文件下载等需要一定的时间等，所以首屏渲染需要一定的时间；SSR 直接由服务端渲染好页面直接返回显示，无需等待下载 js 文件及再去渲染等，所以 SSR 有更快的内容到达时间；

    * 服务端渲染的缺点

    1. 更多的开发条件限制：例如服务端渲染只支持 beforCreate 和 created 两个钩子函数，这会导致一些外部扩展库需要特殊处理，才能在服务端渲染应用程序中运行；并且与可以部署在任何静态文件服务器上的完全静态单页面应用程序 SPA 不同，服务端渲染应用程序，需要处于 Node.js server 运行环境；

    2. 更多的服务器负载：在 Node.js 中渲染完整的应用程序，显然会比仅仅提供静态文件的 server 更加大量占用CPU 资源，因此如果你预料在高流量环境下使用，请准备相应的服务器负载，并明智地采用缓存策略。

## 三, 基于webpack

这里具体的使用不细说了。插件的版本不同可能会有细微差别，大体上不变，先安装，然后 引入插件，接下来是插件的相关配置。

13. 使用 `image-webpack-loader` 来压缩图片。

14. 使用 `uglify.js` 来压缩代码

15. 使用 `vue-lazyload` 来实现图片懒加载。

16. 使用 `vue-skeleton-webpack-plugin` 插件实现骨架屏，减少用户白屏等待焦虑。

## 四， web技术的优化

下面的这些优化并不只针对vue，其他项目也是一样做的。

17. 使用 `compression` 开启 `gzip` 压缩。 开启成功以后我们 `response header` 会有一个 `Content-Encoding: gzip` 字段。当然这个需要我们服务器配置了 `gzip`。

18. 浏览器缓存，为了提高用户加载页面的速度，对静态资源进行缓存是非常必要的，根据是否需要重新向服务器发起请求来分类，将 HTTP 缓存规则分为两大类（强制缓存，对比缓存）。

18. CDN 的使用。
