# 开发中的一些小的点

这是一些开发日程中的一些小的知识点，你应当有一个大概的了解。个人随时看到总结，可能会比较杂乱

## 1. 迭代和递归的区别

1. 迭代使用的是循环结构，递归使用的是选择结构。怎么理解呢？ 迭代和递归，虽然它们都是执行多次，但是他们的终结点是不同的：迭代是因为自身没有可需要迭代而结束，递归是有终止条件的，不能进入死循环。

2. 递归使得程序结构更清晰，更简洁，更容易被人理解，从而减少读懂代码的时间。但是大量的递归会建立函数副本，会占用内存进行维护，迭代并不需要反复调用函数和占用额外的内存。

> 递归的内存占用可以使用尾递归优化 [阮一峰老师关于尾递归文章](http://www.ruanyifeng.com/blog/2015/04/tail-call.html)

## 2. `vue3` 脱离 `vue`  

咋一看，好像觉得有问题， `vue3` 怎么脱离 `vue` 环境，那它还是 `vue` 吗， 还能正常运行吗？看下面代码：

```js
import { ref } from "vue";

export function useCount() {
  const count = ref(0);

  function increment() {
    count.value++;
  }

  function decrement() {
    count.value--;
  }

  return {
    count,
    increment,
    decrement,
  };
}
```

如果你了解 `vue3` , 不难看懂这段代码，但是我要说的是：如果你在另一个 `js` 中导入这个文件使用，也是可以运行的。这里的另一个 `js` 它可以就是一个单纯的js文件，或者基于其他框架的。 `vue3` 的响应式系统已经脱离了组件上下文。[看看作者尤大大的讲解👉](https://www.zhihu.com/question/492260571/answer/2169913043)

## `DOMContentLoaded` 与 `onload`

当纯 `HTML` 被完全加载以及解析时，`DOMContentLoaded` 事件会被触发，而不必等待样式表，图片或者子框架完成加载。而所有资源加载完成之后，`load` 事件才会被触发。这里说的使用资源，包括切不局限于 `css` `js` `img` `font` 等。
