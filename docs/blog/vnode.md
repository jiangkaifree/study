## 1.什么是虚拟 `Dom`

虚拟 Dom (`Virtual DOM`) 实际上它只是一层对真实`DOM`的抽象，以 JavaScript 对象 (`VNode` 节点) 作为基础的树，用对象的属性来描述节点，最终可以通过一系列操作使这棵树映射到真实环境上。在`Javascrip`t 对象中，虚拟 DOM 表现为一个 `Object`对象。并且最少包含标签名 (`tag`)、属性 (`attrs`) 和子元素对象 (`children`) 三个属性，不同框架对这三个属性的名命可能会有差别。但是大体上是一致的。下面就是一个例子 表示 `p`标签里面还有一个`img`标签

```javascript
{
   tag: "p",        // 表示一个p标签
   attrs: {
    "id": "app",
    "class": ["header-wrap"]      //   等等属性
    ···
  },
  children: [
  {
 tag: "img",
  attrs: {
   "src": 'https://······',    // 图片地址
 },
}
]
}
```

创建虚拟 DOM 就是为了更好将虚拟的节点渲染到页面视图中，所以虚拟 DOM 对象的节点与真实 DOM 的属性一一照应

真实 Dom

```js
<div id="app">
  <p class="p">节点内容</p>
  <h3>{{ foo }}</h3>
</div>
```

实例化`Vue`

```javascript
const app = new Vue({
  el: "#app",
  data: {
    foo: "foo",
  },
});
```

通过 VNode，vue 可以对这颗抽象树进行创建节点,删除节点以及修改节点的操作， 经过 diff 算法得出一些需要修改的最小单位,再更新视图，减少了 dom 操作，提高了性能

## 2.为什么需要虚拟 `DOM`

`DOM`是很慢的，其元素非常庞大，页面的性能问题，大部分都是由`DOM`操作引起的
真实的`DOM`节点，哪怕一个最简单的`div`也包含着很多属性。 我们可以在控制台输入如下代码

```javascript
var div = document.createElement("dic"); // 创建一个div标签
var str;
for (var key in div) {
  // 遍历div属性
  str = str + key + "";
}
console.log(str); // 打印结果
```

我们会发现有很多的 属性。由此可见我们操作`dom`来渲染数据的代价是非常昂贵的。
举个例子： 在传统的`mvc`框架中（`jquery`）中我们去更新 10 次 `dom`。 浏览器就会硬生生的更新 10 次。而当我们有了`vnode`以后，同样的 10 次更新，我们会将 10 次更新之后的内容 保存到`js`对象中，最终一次性更新到`dom`中去。
