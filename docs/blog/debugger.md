# 前端简单debugger知识gif图指引

直接进入前端 `debugger` 主题。这些都适合新手。你一定能学会

##  先简单介绍几个按钮

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a2691c31d8e94a7d8f1eff7a965cc72d~tplv-k3u1fbpfcp-watermark.image?)

红色框框内 ，从左往右依次是（我的通俗理解）： 暂停/继续，   下一步，  进入函数， 退出函数。

## 下面我们开始打断点， 开始在你想要的地方调试， 这很简单,取消也只需再次点击：


![GIF.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cfcfaa0c54db410c96ab40d89e21f9d9~tplv-k3u1fbpfcp-watermark.image?)

那么当我们代码执行到此次就会立即停止变成这个样子（看到了吗前面说的按钮亮起了）：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c38fb3db6e1b4f579a29ca4974827c0c~tplv-k3u1fbpfcp-watermark.image?)

## 那么我们接下来开始 `debugger` 吧：

还记得第二个是 **下一步** 吧？ 我们就这样一步一步走，边走边看输出，看和自己预想的逻辑有什么不同


![GIF.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/02d09fa0e589493daa0fbd3739d8dbc5~tplv-k3u1fbpfcp-watermark.image?)

你可以看到鼠标放上去，能看到当前变量的结果 ，点击 **下一步** 执行下面的一行代码。

## 接下来我们进行进行 第三个按钮的操作：

![GIF.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/817ecf9cf6e844e59179b6ba9c27b67b~tplv-k3u1fbpfcp-watermark.image?)

执行到下一行 我们遇到了 另一个函数 `checkWx` , 我们想看到这个函数执行的细节。那就需要 **进入下一个函数调用** 了, 也就是第三个按钮。如果我们并不关心，也可直接 **下一步** 无需进入。

下面我们一步一步执行完出来，回调调用的函数， 同事注意小箭头位置，这里很好的记录了当前一些变量的值：

![GIF.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/df833c7fbf4d4766990184a573f37068~tplv-k3u1fbpfcp-watermark.image?)

## 继续下一步：


![GIF.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ccfe4af9f81742f3afc81ec0b8152d3a~tplv-k3u1fbpfcp-watermark.image?)
很明显， 由于 `result` 为 `false` 我们登录失败，并弹出了消息。

到这里简单的 `debugger` 就结束了。
