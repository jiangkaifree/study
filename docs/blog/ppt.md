# 前言

日常中时不时需要写点ppt，说到ppt，你肯定用的是 office 和 wps，还是用起来有点生涩（我真的不太会），那能不能以程序员的方式(**markdown**)来写ppt呢，接下来介绍一款**专为开发者打造的文档演示工具** —— `Slidev` 。[Sildev文档](https://cn.sli.dev/) , 下面简单了解一下，可能我说的不是很好，但你一点要看文档。

## 具备的功能

1. Markdown支持 ——  这点挺重要
2. 代码高亮
3. 可定制主题
4. 可以交互
5. `mermaid` 图标支持
6. 演讲录制功能

## 开始上手

安装这里就不介绍了，说点其他的：

### 动画

点击动画让 `ppt` 更具有交互性，你不希望你的只是在翻书吧？

`v-click` 为点击添加动画，下面例子就是未 `hello world` `Hey` 追加动画效果：
```
# Hello

<!-- 组件用法：在你按下 “下一步” 之前，这是不可见的 -->
<v-click>

Hello World

</v-click>

<!-- 指令用法：在你第二次按下 “下一步” 之前，这是不可见的 -->
<div v-click class="text-xl p-2">

Hey!

</div>
```

当你在元素中应用 `v-click` 指令时，它会给该元素添加名为 `slidev-vclick-target` 的类。当元素隐藏时，还加上了 `slidev-vclick-hidden` 类。例如：

```
<div class="slidev-vclick-target slidev-vclick-hidden">Text</div>
```

点击后，会变成：
```
<div class="slidev-vclick-target">Text</div>
```

`v-clicks` 仅作为组件提供。它可以快速将其子元素全部添加 `v-click` 指令。它在列表中尤为实用。每次你点击 “下一步” 按钮时，元素会逐条依次出现。
```
<v-clicks>

- Item 1
- Item 2
- Item 3
- Item 4

</v-clicks>
```

更炫的还有动画库， 你要你够6 ，能玩出花：

Slidev 内置了 [@vueuse/motion](https://motion.vueuse.org/)。你可以对任何元素应用 `v-motion` 指令，以对它们施加运动效果。例如 （从其初始化位置 `-80px` 移至其原始位置）：

```
<div
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }">
  Slidev
</div>
```

### 导出或部署

安装 `playwright-chromium`：

```
$ npm i -D playwright-chromium
```

接着，使用如下命令即可将你的幻灯片导出为 PDF：
```
$ slidev export
```

如果你要部署，执行：
```
$ slidev build
```

生成的应用程序会保存在 `dist/` 目录下，然后你可以将该目录部署在 [GitHub Pages](https://pages.github.com/)，[Netlify](https://netlify.app/)，[Vercel](https://vercel.com/)，等你想部署的任何地方。接着，就可以将你幻灯片的链接分享给任何人。

### 编辑器

因为 `Slidev` 使用 `Markdown` 作为源文件，所以你可以使用任何你喜欢的编辑器。

VS Code 插件提供一些特性，这些特性可以帮你更好的组织幻灯片并且可以快速浏览。： [Slidev for VS Code](https://github.com/slidevjs/slidev)


## 主题使用

主题也是我们风格多变的一个必要选项，在 `Slidev` 中更换主题非常简单。在 `frontmatter` 中添加 `theme:` 配置即可。

在服务启动后，它会自动提示你是否安装该主题：

```
? The theme "@slidev/theme-seriph" was not found in your project, do you want to install it now? › (Y/n)
```

想配置主题？

如果你想对当前的主题拥有完全的掌控，你可以将它 **弹出** （eject）到本地的文件系统，并且随心所欲地修改它。可以使用以下命令：`slidev theme eject`

它会在你的目录下生成 `./theme` 目录， 这是请将你的 frontmatter 修改为 (指向你的配置哦)：
```
theme: ./theme
```


## 配置

`Slidev` 内置了很多工具 由于是大部分 `vue` 团队的开发的，`vue` `vite` 的支持这里就不说了，当然还有其他的:  比如 `windicss` 来更好的支持样式，`mermaid` 来支持图表，`Latex` 对数学公示的支持。这些都是按需配置的。

## 组件

当今的前端，如果你还不知道啥事组件的话，那么你需要了解了解。 `Slidev` 中也可以自定义组件，当你在项目根目录下创建 `components/` 文件夹，然后直接把你的自定义 Vue 组件放进去；然后你就可以在你的 markdown 文件里使用该组件啦！

示例：
```
your-slidev/
  ├── ...
  └── components/
      ├── MyComponent.vue
      └── HelloWorld.ts
```

在 `slides.md` 中使用：
```
<!-- slides.md -->

# My Slide

<MyComponent :count="4"/>

<!-- both namings work -->

<hello-world foo="bar">
  Slot
</hello-world>
```