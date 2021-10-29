## 1. 目的

代码规范是指程序员在编码时要遵守的规则，规范的目的就是为了让程序员编写易于阅读、可维护的代码。它的一些好处：

 * 可以促进团队合作
 * 可以降低维护成本
 * 有助于 `code review`（代码审查）
 * 养成代码规范的习惯，有助于程序员自身的成长

 ## 2.命名规范

 命名规范一般指命名是使用驼峰式、匈牙利式还是帕斯卡式；用名词、名词组或动宾结构来命名。

 ```js
     const userInfo = {} // 驼峰式，首字母小写
     const UserInfo = {} // 帕斯卡式，首字母大写
     cosnt strUser = '' // 匈牙利式，前缀表示了变量是什么。这个前缀 str 表示了是一个字符串
 ```

变量命名和函数命名的侧重点不同：
* 变量命名的重点是表明这个变量“是什么”，倾向于用名词命名。
* 而函数命名的重点是表明这个函数“做什么”，倾向于用动宾结构来命名（动宾结构就是 doSomething）

```js
    // 变量命名示例
    const userName = '我只是一只小菜鸡'
    const userAge = 10

    // 函数命名示例
    function getUserInfo () {}
    const chooseAge = () => {}

```

## 3. JS注释

`JS`代码注释比较简单，例如单行注释使用 `//`，多行注释使用 `/**/`。

```js
/**
 * @TODO 相加两数
 * @param {Number} a
 * @param {Number} b
 * @return {Number} 返回一个相加结果
 * @func {getUserInfo} 重新用户信息
 */
function add(a, b) {
    return a + b
}

// 单行注释
const active = true

```

如果这是一个导出的模块，那么我们应当有一个文件头部，来表示这个文件是做什么的，如：

```js

/*
 * @TODO 格式化时间
 * @Author: mikey.zhaopeng
 * @Date:   2016-07-29 15:57:29
 * @Last Modified by: mikey.zhaopeng
 * @Last Modified time: 2016-08-09 13:29:41
 */

```

tips： 这里推荐一个 `vscode` 插件， `vscode-fileheader` 使用快捷键 `ctrl + alt + i`可以生成文件头部注释 ，他自己会监听文件的修改和生成时间，只要你修改 修改时间就会改变。 这里的 `TODO` 内容我我自己加上去的。

## 4.html注释

单纯的`html`文件我们就使用 `<!--这是一段注释。注释不会在浏览器中显示。-->`

```html
<body>

    <!--header模块 -->
    <div></div>
    <!--main模块 -->
    <div></div>
    <!--footer模块 -->
    <div></div>

</body>

```

## 5. css注释

`css` 部分，我们注释一下 主要部分即可，不需要很细节。每一个小块都进行一个注释。如

```css
/**头部**/
.header{}

/**主体部分**/
.main{}

/**footer**/
.footer{}

```

同时我们应当尽量语义化`class`名。如：

```css
// 表示最外层 header容器样式
.header-wrap{}

// 表示 商品列表的每一项样式
.goods-item{}

//表示 年龄板块样式
.age{}

```

## 6. vue组件

在 `vue` 项目中 我们组件化时候应当注释组件的 `props` 以及方法。 以及表示哪些是必传的。如：

```vue

       <!--
        @TODO 表格组件
        @param{Array} tableData 表格数据
        @param{Array} header 表头
        @param{Number} total 数据总条数
        @param{Object} ?pageInfo 数据分页信息  该项可选
       -->
       <DataTable :data="tableData" :total="total" :pageInfo="pageInfo" />
```

对应的 `vue` 项目代码规范，可以查看我的其他文章哦。 [传送门](https://juejin.cn/post/6962172023556538375)

**后续继续记录**