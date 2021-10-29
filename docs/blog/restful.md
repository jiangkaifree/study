简单了解一下 `REST` 接口规范，总结了以下几点。

## 1. 域名

应当尽量尽量部署在专属域名下。

```http
https:api.yourside.com
```

## 2.追加版本方便后期扩展

```http
https:api.yourside.com/v1			// v1版本
```

## 3. 路径（具体的接口地址）

### 3.1 使用名词，不使用动词。

### 3.2 请求方式。

  `GET` 获取数据

  `POST` 提交数据

  `DELETE` 删除数据

  `PATCH` 修改数据，只提交修改部分的数据

  `PUT` 修改数据，提交修改后的完整数据

还有两个不常用的`OPTIONS`,`HEAD`。

下面举例，

```js
GET		https:api.yourside.com/v1/goods				//获取所有商品列表
GET		https:api.yourside.com/v1/goods/id			//获取这个id的商品
POST		https:api.yourside.com/v1/goods				// 提交商品
PUT		https:api.yourside.com/v1/goods/id			// 修改这个id商品信息，提交所有数据
PATCH		https:api.yourside.com/v1/goods/id			// 修改这个id商品信息，提交修改项数据
DELETE		https:api.yourside.com/v1/goods/id			// 删除这个id的商品
GET		https:api.yourside.com/v1/shops/id/goods/id		// 获取这个id店铺的某个id的商品
POST		https:api.yourside.com/v1/shops/id/goods				// 提交某个id商店的商品
以此类推
```

## 4. 过滤数据

过滤信息放在url当中，如下：

```js
?pageSize=10&pageIndex=0			// 指定第0页10条数据
?limt=10							// 限制获取10条数据
```

## 5. 返回数据

返回状态码。

```js
200 成功返回数据
201 修改或提交数据成功
204 删除成功
401 用户没有权限
403 用户有权限，但是被禁止访问
404 未找到资源数据
410 资源被删除，不会再得到
500 服务器发生错误
```

上面只是一些常用的状态码，还有其他的一些状态码。

返回结果

```js
GET		/goods		返回商品列表（数组）
GET		/goods/123456	返回id是123456的商品信息（对象）
POST		/goods		返回提交的商品数据
PUT		/goods/123456	放回id是123456商品修改后的完整数据
PATCH		/goods/123456	返回id是123456商品修改后的完整数据
DELETE		/goods/123456	返回空内容
```

## 6.错误处理

如果状态码是4XX，我们应当返回对应的错误内容，举例如下：

下面是一个401.的接口返回内容

```js
{
	code：401，
	error: '该用户没有权限'
}
```

欢迎访问个人全栈不一样的BLOG[预览](https://blog.happynewball.com/) [项目地址传送门有完整搭建](https://gitee.com/JK-2462870727/personal-blog)。