## 1、根本理念上的不同

Vue 和 React 之间最根本的区别在于 它们对待自己的定位是不一样的。

从它们官网直观来看，React 把自己描述为 “一个用于构建用户界面的 JavaScript 库” ，而 Vue.js 则把自己描述为 “渐进式 JavaScript 框架”。一个是库 一个是框架。从历史上看，库和框架都专注于让它们的工作表现得更出色，但 框架的要求和提供的能力更全面详尽，而库则更少更轻量。

## 2、单文件组件

Vue 和 React 都有用来创建 UI 的组件。

组件通常由 3 部分组成：

1. UI(HTML)

2. 行为 (JavaScript)

3. 外观 (CSS)

Vue 是单文件 开箱即可使用 一个 .vue 文件已经帮你区分好了基本的三大模块

```javascript
<template>
 <p>{{ greeting }} World!</p>
</template>

// 行为
<script>

</script>


// 样式
<style scoped>

</style>
```

React 组件提供了开箱即用的 UI 和 行为部分 ，但是样式在很大程度上不受限制.

```javascript
import React, { useState } from "react";

function Button() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Current count: {count}
      <br />
      Click me
    </button>
  );
}
```

当然了，React 有一个非常活跃的社区，所以如果你想包含样式，可以轻松使用第三方的库 比如 ==Emotion== 或 ==Styled Components== 等 来填补样式的空缺，但是：

1. 它们是非内置的。

2. 你必须知道这些库才能使用他们。

> tips: 官方的 推荐`css`方案是 `css in js` 像 `Styled Components` 这种就是 `css in js` 方案

## 3. 开发当中的不同

### 3.1 v-model （最重要的一个部分）

通俗来说 Vue 和 React 他们 很大的一个使用上的区别是 `Vue` 是自动挡 而 React 是手动挡。Vue 当中 你使用`v-model` 就会自动为你进行 视图与数据的统一， 而`React` 则需要你 自己手动去触发 `this.setState`。

### 3.2 插槽

在 Vue 中 会有插槽这个概念 而 React 当中是没有的

```javascript
<!-- A Vue.js component template named "base-layout" -->
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>

<!-- When "base-layout" is used -->
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

### 3.3 指令

Vue.js 确实关注“代码重用和抽象的主要形式是组件”，有官方的一些指令 比如 `v-show` `v-if` `v-bind` `v-model` 等 。当然还可以 自定义指令 这里有一个使用自定义指令不错的例子，通过 v-focus 在 mount 时自动聚焦到元素上：

```js
const app = Vue.createApp({})
// Register a global custom directive called `v-focus`
app.directive('focus', {
  // When the bound element is mounted into the DOM...
  mounted(el) {
    // Focus the element
    el.focus()
  }
})

// 使用
<input v-focus />
```

React 当中没指令这个概念
