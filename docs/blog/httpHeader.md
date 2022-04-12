# 前言

有没有一种可能，你在浏览器network中看到的数据，你并不能拿到。下面👇是你在 network中看到的一个简单示例：

```js
常规
请求 URL:https://api.juejin.cn/interact_api/v1/pin_tab_lead?aid=2608&uuid=7076816640441288192
请求方法:GET
状态代码:200
远程地址:111.1.167.3:443
引用站点策略:strict-origin-when-cross-origin

响应头
cache-control:no-cache
access-control-allow-credentials:true
access-control-allow-origin:https://juejin.cn
content-length:44
content-type:application/json; charset=utf-8
.....

请求头
accept:*/*
accept-encoding:gzip, deflate,
accept-language:zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
content-length:2257
content-type:text/plain;charset=UTF-8
origin:https://www.juejin.net
......
```
上面出现了很多我们比较熟悉的信息， 比如请求方式，请求状态码，还有 `content-type` 表示已何种方式去解析。当然了，还有返回的响应数据了，那有没有可能，看到了响应的数据，却拿不到了。

## 1.有没有可能获取不到响应数据

当你指定你的响应类型 `responseType` 时, **`responseType`** 是一个枚举字符串值，用于指定响应中包含的数据类型。它有下面这些值对应的介绍：

|value|说明|
|---|---|
|""|空的 `responseType` 字符串与默认类型 `"text"` 相同。|
|arrayBuffer|`response`包含二进制数据的 JavaScript|
|blob|[`response`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/response "response") 是一个包含二进制数据的 [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象。|
|documnet|[`response`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/response "response") 是一个 [HTML](https://developer.mozilla.org/zh-CN/docs/Glossary/HTML) [`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 或 [XML](https://developer.mozilla.org/zh-CN/docs/Glossary/XML) [`XMLDocument`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLDocument)，根据接收到的数据的 MIME 类型而定。|
|json|-   [`response`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/response "response") 是通过将接收到的数据内容解析为 [JSON](https://developer.mozilla.org/zh-CN/docs/Glossary/JSON) 而创建的 JavaScript 对象。|
|text|-   [`response`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/response "response") 是 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString) 对象中的文本。
|
|ms-stream|-   [`response`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/response "response") 是流式下载的一部分；此响应类型仅允许用于下载请求，并且仅受 Internet Explorer 支持。|


所以当 `responseType` 的值为 `json`， 但是实际得到的却是 `text`，则 `response` 的值会被设置为 `null`。 看看 MDN文档的介绍 [XMLHttpRequest.responseType - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseType)


## 2.有没有可能获取不到 headers 中的部分信息

业务中，我们常常会在 `header` 中增加一个 `token` 字段来进行身份校验，有没有可能我们拿不到呢？ 

上面的示例中响应头信息出现了一个 `Access-Control-Allow-Credentials` 属性，它表示是否可以将对请求的响应暴露给页面。返回true则可以，其他值均不可以。实际中还需工作中与[`XMLHttpRequest.withCredentials`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/withCredentials) 或Fetch API中的[`Request()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request/Request "Request()") 构造器中的`credentials` 选项结合使用。确保前后端都允许。

还有另一个属性 `Access-Control-Allow-Headers`，它的值是列出了将会在正式请求的 [`Access-Control-Request-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Headers) 字段中出现的首部信息。举一个例子：

```
Access-Control-Allow-Headers: Token
Content-length:2257
Content-type:text/plain;charset=UTF-8
Origin:https://www.juejin.net
Token：111
Name：jack
```
上面的这个响应头我们加入了自定义的 `Token` 和 `Name`，但由于 `Access-Control-Allow-Headers` 的值只有 `Token`, 那么你并不能获取到 `Name` 。[Access-Control-Allow-Headers - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)

最后说真的 这个 MDN 关于HTTP的文档里面知识点有点多，确实需要看看了。[HTTP | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)
