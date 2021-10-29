<!--
 * @Author: your name
 * @Date: 2021-10-29 21:14:00
 * @LastEditTime: 2021-10-29 21:15:09
 * @LastEditors: your name
 * @Description: In User Settings Edit
 * @FilePath: /docs/Users/xiaocaiji/Desktop/study/docs/blog/httpCache.md
-->

## 1. http 缓存

首先，`http`缓存 分为 **强缓存** 和 **协商缓存**。浏览器加载一个页面的流程如下：

1. 浏览器先根据这个资源的 http 头信息来判断是否命中强缓存。如果命中则直接加在缓存中的资源，并不会将请求发送到服务器。返回 `http` 状态码 200。

2. 若没有命中强缓存，则与服务器进行协商缓存的对比，命中则返回 `http` 状态码 304。

3. 没有命中则服务器返回最新的资源。`http` 状态码 200

## 2. 强缓存

强缓存利用 header 中的 **Expires** 和 **cache-Contol** 两个字段来控制。

- **Expires** 指定缓存过期的时间。超过这个时间，缓存就会失效，则去服务端请求资源。

- **Cache-Contol** 可取值：max-age ，s-maxage ， public （客户端和代理服务器(CDN)都可缓存）， private （只有客户端缓存）， no-cache （缓存） ， no-store （内容不缓存）。 例如：Cache-Contol= max-age=3600.代表资源有效期为 3600 秒。

## 3. 协商缓存

- 方式一： **ETag 和 If-None-Match**

**Etag** 是在上一次请求资源时生成，在`response header`返回， 是对该资源的唯一标识（类似于`id`）。只要资源变化即重新生成。 下一次请求资源时，会携带在 `request header` 中的 **If-None-Match**。 服务器拿到该值与上一次生成的**Etag**进行对比。如果相同，命中协商缓存。不相同则返回最新资源。

- 方式二：**Last-Modified 和 If-Modified-Since**

这种方式一样，**Last-Modified** 是该资源文件最后一次更改时间,服务器会在 `response header`里返回。浏览器进行保存，下一次请求在 `request header`中的 **If-Modified-Since**进行携带。 服务器进行比较。

区别：

1. 精度上 ：**Etag**要优于 **Last-Modified** ，Last-Modified 的时间单位是秒，如果某个文件在 1 秒内改变了多次，那么他们的 **Last-Modified** 其实并没有体现出来修改，但是 **Etag** 每次都会改变确保了精度（即只要修改就会体现出来）。

2. 性能：同样，**Etag** 在生成唯一标识的时候是涉及到复杂计算来得到结果，存在着消耗。性能会比 **Last- Modified** 差。

优先级： **Etag** 比 **Last- Modified** 大

## 4. 浏览器行为效果

| 用户操作                   | Expires/Cache-Control（强缓存） | Last- Modified （协商缓存） |
| -------------------------- | ------------------------------- | --------------------------- |
| 地址栏回车                 | 有效                            | 有效                        |
| 新开窗口                   | 有效                            | 有效                        |
| 前进后退                   | 有效                            | 有效                        |
| F5 刷新                    | 无效                            | 有效                        |
| ctrl + F5 刷新（强制刷新） | 无效                            | 无效                        |
