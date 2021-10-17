# 2021-09-23

1. 看了 b 站的 `golang` 的速成学习视频。 准备先看完书籍，再跟着视频做一下简单案例。

> 最近的主要阅读基础性的书籍。

# 2021-09-25

1. 阅读红宝书 `IndexedDB` 章节内容。

# 2021-09-26

看完了 `IndexedDB` 章节，大概理解一下：

- `IndexedDB` 是类似于 `sql` 的数据存储，但是它们的区别点在于 `sql` 存储的是 **数据表** ，而 `IndexedDB` 存储的是对象。可以在，`IndexedDB` 创建游标来读取特定数据，可以倒序读，也可以顺序。只要传入不同的参数即可。还可以创建索，在索引上创建游标来读数据。

- 不同浏览器会有限制大小。

- 不能跨域共享，`www.aaa.com` 和 `bb.aaa.com` 它们并不共享同一个。

准备开启下一章节。这本书可以多读几遍才能吃的很透。

# 2021-09-28

今天开始阅读 **模块** 章节：

> 在 `es6` 之前的模块加载器 `CommonJS` , `AMD`, `UMD`。`CommonJS` 依赖几个全局属性如 `require` 和 `module.exports` ，`CommonJS` 以服务端为目标环境进行构建，而 `AMD` 是在浏览器环境按需加载依赖。所以最终诞生了 `UMD`. 它为了统一 `CommonJS`, `AMD`的生态系统。本质上，`UMD` 定义的模块会在启动时 检测要使用哪个模块系统，然后进行适当配置，并把所有逻辑包装在一个立即调用的函数表达式 `(IIFE)` 中。虽然这种不是很完美，但可以实现两个生态的共存。`ES6` 的模块方案也就诞生了 (解决`CommonJS` 与 `AMD` 之间的 冲突)。

# 2021-09-29

剩下的 `ES6` 模块， 大概简述：

1. `ES6` 借鉴了 前面所说的模块方案的优点，并在它们上，新增了新的行为：

> - 模块只加载一次，
> - 模块代码只在加载后执行
>   ......
>
> 新的：
>
> - `ES6` 模块不共享全局命名空间
> - 模块顶级 ` this`` 的值是 undefined ` (常规脚本中是 `window`)。
> - ES6 模块是异步加载和执行的。
> - 模块顶级 `this` 的值是 `undefined`(常规脚本中是 `window`)。
>   .......

2. 除了常规的导出操作，还有 一种比较特殊的

```js
// 1.命名导出
export const foo = 'foo';

// 2.命名导出
const foo = 'foo';
export { foo };

// 3.命名导出。 允许，但应该避免
 export { foo };
const foo = 'foo';

// 4.默认导出
const foo = 'foo';
export default foo;

// 5. 因为命名导出和默认导出不会冲突，所以 ES6 支持在一个模块中同时定义这两种导出:
const foo = 'foo';
const bar = 'bar';
export { bar };
export default foo;

```

> 注意：每个模块只能有一个默认导出

3. 导入

```js
// 1.
import ...

// 2.
import { foo } from './fooModule.js';

// 3. 允许，但应该避免
console.log(foo); // 'foo'
import { foo } from './fooModule.js';

// 4.
import { foo, bar, baz as myBaz } from './foo.js';

// 5. 等效
import { default as foo } from './foo.js';
import foo from './foo.js';

// 6.
import foo, { bar, baz } from './foo.js';


```

# 2021-10-03

已经休息了两天了，今天要看了点书。打算这个月看完这本红宝书。打算粗略的看看。大概了解部分即可，
今天看了 **工作者线程** ，感觉一下子, 有了新的认识：

简单的理解，浏览器开辟一个新的窗口就是一个新的线程。它们都是一个独立的沙盒。
即（实际的线程）。下面是工作者线程的特点：

1. 工作者线程是基于实际线程实现的。
2. 工作者线程并行执行。
3. 工作者线程可以共享某些内存。
4. 工作者线程不共享全部内存
5. 工作者线程不一定在同一个进程里。
6. 创建工作者线程的开销更大

工作者线程类型：

1. 专用工作者线程。
2. 共享工作者线程
3. 服务工作者线程

工作者线程中的全局对象是 `WorkerGlobalScope` 的实例. 通过 `self` 关键字暴露出来。 `self` 身上有属性和方法， 它们都是 `Window` 对象的子集。每种类型的 工作者线程都有自己特定的全局对象，都继承自 `WorkerGlobalScope`

- 专用工作者线程使用 `DedicatedWorkerGlobalScope`。
- 共享工作者线程使用 `SharedWorkerGlobalScope` 。
- 服务工作者线程使用 `ServiceWorkerGlobalScope`。

创建专用工作者线程:

```js
emptyWorker.js;
// 空的 JS 工作者线程文件 main.js
console.log(location.href); // "https://example.com/"

// 可修改为使用相对路径 ./emptyWorker.js
const worker = new Worker(location.href + "emptyWorker.js");
console.log(worker); // Worker {}
```

> 注意： 工作者线程的脚本文件只能从与父页面相同的源加载。从其他源加载工作者线程的脚本文件会导致错误。在工作者线程内部，使用 `importScripts()` 可以加载其他源的脚本。

> 注意： 要管理好使用 Worker()创建的每个 Worker 对象。在终止工作者线程之前，它不 会被垃圾回收。

`work` 对象有一些属性 `onerror` , `onmessage`, `onmessageerror`。

一般来说，专用工作者线程可以非正式区分为处于下列三个状态:初始化(initializing)、活动(active) 和终止(terminated)。

`Worker` 配置项：

- name
- type
- credentials

动态加载脚本 `importScripts()`

`importScripts()` 方法可以接收任意数量的脚本作为参数， 浏览器下载它们的顺序没有限制，但 执行则会严格按照它们在参数列表的顺序进行：

```js
console.log("importing scripts");
importScripts("./scriptA.js", "./scriptB.js");
console.log("scripts imported");
```

它们共享作用域：

```js
main.js;
const worker = new Worker("./worker.js", { name: "foo" });
// importing scripts in foo with bar
// scriptA executes in foo with bar
// scriptB executes in foo with bar
// scripts imported

// scriptA.js
console.log(`scriptA executes in ${self.name} with ${globalToken}`);

// scriptB.js
console.log(`scriptB executes in ${self.name} with ${globalToken}`);

// worker.js
const globalToken = "bar";
console.log(`importing scripts in ${self.name} with ${globalToken}`);
importScripts("./scriptA.js", "./scriptB.js");
console.log("scripts imported");
```

有时候可能需要在工作者线程中再创建子工作者线程

```js
main.js;
const worker = new Worker("./js/worker.js");
// worker
// subworker

// js/worker.js
console.log("worker");
const worker = new Worker("./subworker.js");

// js/subworker.js
console.log("subworker");
```

# 2021-10-05

接下来是 **共享工作者线程** 相关的点：

正如前面说的：共享工作者线程或共享线程与专用工作者线程类似，但可以被多个可信任的执行上下文访问。例如，同源的两个标签页可以访问同一个共享工作者线程。与专用工作者线程类似：

1. 创建共享工作者线程：

```js
new SharedWorker("./sharedWorker.js");
```

> 注意：多个相同的工作者线程也只会实例化一个

```js
// 实例化一个共享工作者线程
// - 全部基于同源调用构造函数
// - 所有脚本解析为相同的URL
// - 一个线程名称为'foo'，一个线程名称为'bar'
new SharedWorker("./sharedWorker.js", { name: "foo" });
new SharedWorker("./sharedWorker.js", { name: "foo" });
new SharedWorker("./sharedWorker.js", { name: "bar" });
```

当每个浏览器标签页使用 `new Worker('./worker.js');` 初始化一个专用工作者线程时, 线程数 +1 ，使用一次就会加一。而 `new SharedWorker('./sharedWorker.js');` 实例化：线程数 只有一个，因为它们共享。只有当所有页面销毁且没有 连接时，浏览器才会终止共享线程。本质上：每次调用 `SharedWorker()` 构造函数，无论是否创建了工作者线程，都会在共享线程内部触发 `connect` 事件。

## 服务工作者线程

服务工作者线程(service worker)是一种类似浏览器中代理服务器的线程，可以拦截外出请求和缓 存响应。与共享工作者线程类似，来自一个域的多个页面共享一个服务工作者线程。服务工作者线程在两个主要任务上最有用:充当网络请求的缓存层和启用推送通知。

服务工作者线程与专用工作者线程或共享工作者线程的一个区别是没有全局构造函数。服务工作者
线程是通过 `ServiceWorkerContainer` 来管理的，它的实例保存在 `navigator.serviceWorker` 属性中。

1. 创建服务工作者线程

```js
emptyServiceWorker.js;
// 空服务脚本 main.js

// 注册成功，成功回调(解决)
navigator.serviceWorker
  .register("./emptyServiceWorker.js")
  .then((registrationA) => {
    console.log(registrationA);
  });

// 使用不存在的文件注册，失败回调(拒绝)
navigator.serviceWorker
  .register("./doesNotExist.js")
  .then(console.log, console.error);
```

> 关于注册成功后的 `ServiceWorkerRegistration` 的 `api` 暂时没有记录，比较多，需要翻看原书。

2. 关于安全限制

由于服务工作者线程几乎可以任意修改和重定向网络请求，以及加载静态资源，服务工作者线程
API 只能在安全上下文(HTTPS)下使用。在非安全上下文(HTTP)中，`navigator.serviceWorker`
是 `undefined` 。为方便开发，浏览器豁免了通过 `localhost` 或 `127.0.0.1` 在本地加载的页面的安全上下文规则。

**相似的**：在服务工作者线程内部，全局上下文是 `ServiceWorkerGlobalScope` 的实例。`ServiceWorker- GlobalScope` 继承自 `WorkerGlobalScope`，因此拥有它的所有属性和方法。服务工作者线程可以通 过 self 关键字访问该全局上下文。

服务工作者线程状态：

- `install`: 在服务工作者线程进入安装状态时触发
- `activate`:在服务工作者线程进入激活或已激活状态时触发

---

服务工作者线程缓存

- 服务工作者线程缓存不自动缓存任何请求。所有缓存都必须明确指定。

- 服务工作者线程缓存没有到期失效的概念。除非明确删除，否则缓存内容一直有效。

- 服务工作者线程缓存必须手动更新和删除。

- 缓存版本必须手动管理。每次服务工作者线程更新，新服务工作者线程负责提供新的缓存键以保存新缓存。

- 唯一的浏览器强制逐出策略基于服务工作者线程缓存占用的空间。服务工作者线程负责管理自
  己缓存占用的空间。缓存超过浏览器限制时，浏览器会基于最近最少使用(LRU，Least Recently Used)原则为新缓存腾出空间。

`CacheStorage` 对象

```js
// create
caches.open('v1').then(console.log);

// has
caches.open('v1').then(() => caches.has('v1))

// delete
caches.open('v1').then(() => caches.has('v1))
.then(() => caches.delete('v1'))
.then(() => caches.has('v1'))
.then(console.log); // false

// keys
caches.open('v1')
.then(() => caches.open('v3'))
.then(() => caches.open('v2'))
.then(() => caches.keys())
.then(console.log);
// ["v1", "v3", "v2"]
```

关于 `caches`

```js
const request1 = new Request("https://www.foo.com");

const response1 = new Response("fooResponse");

caches.open("v1").then((cache) => {
  // 在键(Request 对象或 URL 字符串)和值(Response 对象)同时存在时用于添加缓存项。该方法返回期约，在添加成功后会解决。
  cache
    .put(request1, response1)
    .then(() => cache.keys())
    .then(console.log) // [Request]
    .then(() => cache.delete(request1))
    .then(() => cache.keys())
    .then(console.log); // []
});
```

# 2021-10-06

服务工作者线程的生命周期

6 种服务工作者线程可能存在的状态:已解析(parsed)、安装中 (installing)、已安装(installed)、激活中(activating)、已激活(activated)和已失效(redundant)。完整 的服务工作者线程生命周期会以该顺序进入相应状态

我们可以 如此进行状态的判断：

```js
navigator.serviceWorker.register("./serviceWorker.js").then((registration) => {
  registration.installing.onstatechange = ({ target: { state } }) => {
    console.log("state changed to", state);
  };
});
```

> 注意: 虽然已解析(`parsed`)是 `ServiceWorker` 规范正式定义的一个状态，但 `Service- Worker.prototype.state` 永远不会返回 `"parsed"`

安装中状态

```js
navigator.serviceWorker.register("./serviceWorker.js").then((registration) => {
  // 判断installing是否是否被设置为ServiceWorker 实例:
  if (registration.installing) {
    console.log("Service worker is in the installing state");
  }
});
```

已安装状态

```js
navigator.serviceWorker.register("./serviceWorker.js").then((registration) => {
  if (registration.waiting) {
    console.log("Service worker is in the installing/waiting state");
  }
});
```

激活中状态

```js
navigator.serviceWorker.register("./serviceWorker.js").then((registration) => {
  if (registration.active) {
    console.log("Service worker is in the activating/activated state");
  }
});
```

已激活状态

```js
navigator.serviceWorker.register("./serviceWorker.js").then((registration) => {
  if (registration.active) {
    console.log("Service worker is in the activating/activated state");
  }
});
```

上面 👆 这种表示服务工作者线程可能在激活中状态，也可能在已激活状态。不够严谨。下面的方式会更好：

```js
navigator.serviceWorker.register("./serviceWorker.js").then((registration) => {
  if (registration.controller) {
    console.log("Service worker is in the activating/activated state");
  }
});
```

已失效状态

已失效状态表示服务工作者线程已被宣布死亡。浏览器随时可能回收资源。

## 管理服务文件的缓存

为了尽可能传播更新后的服务脚本，常见的解决方案是在响应服务脚本时设置 Cache-Control: max-age=0 头部。这种方式只能服务端控制，为了让客户端控制:

```js
navigator.serviceWorker.register("/serviceWorker.js", {
  updateViaCache: "none",
});
```

## 小结

1. 工作者线程可以运行异步 JavaScript 而不阻塞用户界面。这非常适合复杂计算和数据处理，特别是 需要花较长时间因而会影响用户使用网页的处理任务。
2. 工作者线程可以是专用线程、共享线程。专用线程只能由一个页面使用，而共享线程则可以由同源 的任意页面共享。
3. 服务工作者线程用于让网页模拟原生应用程序。服务工作者线程也是一种工作者线程，但它们更像 是网络代理，而非独立的浏览器线程。可以把它们看成是高度定制化的网络缓存，它们也可以在 PWA 中支持推送通知。

这一个章节看完了。只是大概看了一下，细节的 `api` 相关的每太仔细，知道大概有这么个东西。

# 2021-10-07

假期结束了喔 😯 。最后一个章节了

## 1. 可维护代码具有如下特点：

- 容易理解
- 符合常识
- 容易适配
- 容易扩展
- 容易调试

变量及函数的命名相关规范指引： [变量函数命名规范文章](https://juejin.cn/post/7008071619109191693)

总结额外的几点：

1. 类名应该首字母大写，如 `Person` 、`RequestFactory` 。常量值应该全部大写并以 下划线相接，比如 `REQUEST_TIMEOUT`。
2. 因为 `JavaScript` 是松散类型的语言, 定义变量时，应该立即将其初始化为一个将来要使用的类型值。

```js
// 通过初始化标明变量类型
let found = false; // 布尔值
let count = -1; // 数值
let name = ""; // 字符串
let person = null; // 对象
```

## 2. 解耦

下面是有一个例子：
```js
function handleKeyPress(event) {
  if (event.keyCode == 13) {
    let target = event.target;
    let value = 5 * parseInt(target.value);
    if (value > 10) {
      document.getElementById("error-msg").style.display = "block";
    }
  }
}
```
上面例子中的这个事件处理程序除了处理事件，还包含了应用程序逻辑。更好的做法是将应用程序逻辑与事件处理程序分开，各自负责处理各自的事情。如下：

```js
// 应用程序逻辑
function validateValue(value) {
  value = 5 * parseInt(value);
  if (value > 10) {
    document.getElementById("error-msg").style.display = "block";
  }
}

//事件处理
function handleKeyPress(event) {
  if (event.keyCode == 13) {
    let target = event.target;
    validateValue(target.value);
  }
}
```
> 注意:
> * 不要把 event 对象传给其他方法，而是只传递 event 对象中必要的数据。
> * 应用程序中每个可能的操作都应该无须事件处理程序就可以执行。
> * 事件处理程序应该处理事件，而把后续处理交给应用程序逻辑。

## 3. 避免污染全局

```js
// 两个全局变量:不要!
var name = "Nicholas";
function sayName() {
  console.log(name)
}

以上代码声明了两个全局变量: `name` 和 `sayName()` 。可以像下面这样把它们包含在一个对象中:
// 一个全局变量:推荐
var MyApplication = {
  name: "Nicholas",
  sayName: function() {
    console.log(this.name);
  }
};
```
这样做还有以下的好处：
1. 调用 MyApplication.sayName()从逻辑上会暗示，出现任何问题都可以在 `MyApplication` 的代码中找原因。
2. 这样一个全局对象可以扩展为命名空间的概念。可以通过这个对象来 暴露能力。也方便后续的扩展。

## 常量的提取
满足以下标准则说明数据需要提取：
* 重复出现的值:任何使用超过一次的值都应该提取到常量中，这样可以消除一个值改了而另一个值没改造成的错误。这里也包括 CSS 的类名。
* 用户界面字符串:任何会显示给用户的字符串都应该提取出来，以方便实现国际化。
* `URL`: `Web` 应用程序中资源的地址经常会发生变化，因此建议把所有 `URL` 集中放在一个地方管理。
* 任何可能变化的值:任何时候，只要在代码中使用字面值，就问问自己这个值将来是否可能会变。如果答案是“是”，那么就应该把它提取到常量中。看下面例子：
```js
// 在管理后台中，会存在很多不同类型的用户他们的权限是不同的，下面是一个用户列表
const users = [
  {
    type: 'admin',
    name: '管理员',
  },
  {
    type: 'user',
    name: '普通用户',
  },
]

// 现在我们可能需要追加一个新的用户类型，这是我们应当提取
const users = [
  {
    type: 'admin',
    name: '管理员',
  },
  {
    type: 'system',
    name: '系统管理员',
  },
  {
    type: 'user',
    name: '普通用户',
  },
]
```

## 3. 代码优化

1. 避免查找
```js
let query = window.location.href.substring(window.location.href.indexOf("?"));

// 优化后
let url = window.location.href;
let query = url.substring(url.indexOf("?"));
```
通过数代码中出现的点号数量，就可以知道有几次属性查找。最上面的代码发生了 6 次属性查找，3 次是为查找 `window.location.href.substring()`，3 次是为查找 `window.location.href.indexOf()`。而下面的只有 4 次

2. 优化循环

* 简化终止条件。因为每次循环都会计算终止条件，所以它应该尽可能地快。
*  简化循环体。循环体是最花时间的部分，因此要尽可能优化。要确保其中不包含可以轻松转移到循环外部的密集计算。（简单说就是循环体内部不要设计可以在外部进行的计算）
```js
for (let i = 0; i < values.length; i++) {
  const minAge = 12
  const maxAge = minAge + i
  if (maxAge > value[i]) {
    process(values[i]);
  }
}

// 优化后
const minAge = 12
const counts = values.length
for (let i = 0; i < counts; i++) {
  const maxAge = minAge + i
  if (maxAge > value[i]) {
    process(values[i]);
  }
}
```
上面的例子中： 第一避免了多次计算终止条件 `values.length` 而直接读取 `counts`, 第二 避免了循环体当中多次声明 `minAge`, 它完全可以在循环外部进行声明，当然这里只是声明 ，也可能是计算。如果是计算则更应当提取到循环体外部。

## 4. `DOM` 操作的优化

1. 最小化的实时更新 `document.createDocumentFragment`
```js
let list = document.getElementById("myList"),
  item;
for (let i = 0; i < 10; i++) {
  item = document.createElement("li");
  list.appendChild(item); item.appendChild(document.createTextNode('Item ${i}');
}

// 优化后
let list = document.getElementById("myList"),
  fragment = document.createDocumentFragment(),
  item;
for (let i = 0; i < 10; i++) {
  item = document.createElement("li");
  fragment.appendChild(item);
  item.appendChild(document.createTextNode('Item ${i}');
}
list.appendChild(fragment);
```
这样修改之后，完成同样的操作只会触发一次实时更新。这是因为更新是在添加完所有列表项之后 一次性完成的。

2. 使用 `innerHTML`

上面的例子如果使用 `innerHTML` 重写就是这样子：
```js
let list = document.getElementById("myList"),
  html = '';
for (let i = 0; i < 10; i++) {
   html += '<li>Item ${i}</li>';
}
list.innerHTML = html;
```

这两种技术区别不大，但对于大量 DOM 更新，使用 innerHTML 要比使用标准 DOM 方法创建同样的结构快很多。事实上，调用 innerHTML 也应该看 成是一次实时更新。构建好字符串然后调用一次 `innerHTML` 比多次调用 `innerHTML` 快得多。
```js
let list = document.getElementById("myList");
for (let i = 0; i < 10; i++) {
  list.innerHTML += '<li>Item ${i}</li>'; // 不要
}
```

3. 事件委托
事件委托利用了事件的冒泡。任何冒泡的事件都可以不在事件目标上，而在目标的任何祖先元素上处理。

## yes 看完书了😄

# 2021-10-10

接下来准备看一下 《css世界》这本书，然后写下来系统的搭建一个 笔记文档 仓库。打算使用 [docsify](https://docsify.js.org/#/zh-cn/quickstart) 这个静态文档工具进行写文档，并发布。

# 2021-10-11

最近一直想提升一下自己的学历，想考研，先看一点书吧，打算试一次。

# 2021-10-16

1. 学习考研数学2内容。
2. 最近在整理代码规范相关的内容，在网上查找资料，总结等等。