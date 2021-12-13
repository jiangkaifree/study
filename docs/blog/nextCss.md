# 前言

前端发展的这几年，`css` 也由最初的转变为 `less`, `sass/scss` 等预处理器。它们给 `css` 带来了和 `js` 一样的变量，函数，运算，使得更加的灵活。那么下一代的 `css` 解决方案是什么呢？一起来探寻一下吧

## 解决痛点

事物的出现，肯定伴随着问题。新的解决方案一定是为了解决一定程度上的问题。那么原本的 `css` 方案已经足够的完善，为什么还有着下一代的探索呢？ 主要是为了两个问题：

1. 更大程度的自定义化

2. 避免重复 `css` 书写

解释一下上面两点: 结合一个使用场景来理解， 项目中通常我们会使用一个 `ui` 库来快速开发，预先设定好的 `ui` 给我们带来快速搭建的同时，也有着样式上的局限性。假如一个简单的 `button`，虽然通过预留的 `api` 我们能够改变它的颜色, 比如：

```css
// 主题颜色
<Button type="primary">Primary Button</Button>

// 错误颜色
<Button type="error">Primary Button</Button>

// 警告颜色
<Button type="danger">Primary Button</Button>
```

如果我们需要其他的颜色，就应该写自己的样式进行覆盖

```css
<Button type="primary" class='btnColor'>Primary Button</Button>

// style.css
.btnColor {
    background-color: #ccc;
}
```

内置的样式可能并不能满足我们的需求。同时，这个你可能并没有使用到的 `css` 还会打包进入到你的代码当中，尽管最终生效的并不是它。还有，如果此时另一个地方也需要着相同的样式，我们又需要重新写一份。当然了， `less`等语言他喵通过 `extend`, `minx` 也为我们带来了解决方式。但是实际上我们经常使用的 `css` 样式也就那么几个。我们能不能直接预定义写好所有的常用样式，后续直接进行引用即可。这也就带来了新的 `css` 方案。

## 新的`css` 方案(原子化css)

新的 `css` 方案在社区已经有很久了，而且已被诸多的实践,运用。 比较大的框架有： [tailWindCSS](https://www.tailwindcss.cn/docs) , [windcss](https://windicss.org/guide/)。因为有着很多的运用，相信很多人都有着了解，使用上：

```html
<div class="p-6 text-xl  bg-white rounded-xl  flex items-center space-x-4">
  
</div>
```

上面的案例中,使用了简单的几个预设样式：`p-6` 指的是 `padding: 6px`, `text-xl` z 


