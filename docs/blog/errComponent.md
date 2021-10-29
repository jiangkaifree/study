## 1. 出发点

因为自己在公司原有项目上的维护，有时候因为与后端获取数据过程当中，因为种种原因获取失败，这里有可能是后端的问题，也可能是前端的问题。一旦获取失败，前端页面就会直接性的崩溃（白屏不显示，页面报错等等）。这样子给用户带来不好的体验。


所以，想封装一个异常显示的组件来解决，在意料之外的错误，以至于页面不会白屏，只是提醒用户页面的一些部分处于一个异常状态。**完整代码**

## 2. 思考需要哪些部分

首先，一个组件，最基本的两个部分 `prop` 和 `function`。 我们希望通过传入哪些 `props` 来为组件提供哪些属性，以及 方法。下面列出来一些基本的，需要支持的。后面可以进行扩展。

* 必须支持 `GET` 和 `POST` 请求。
* 正常状态下，显示页面的正常组件。异常状态显示异常组件
* 异常状态提供一个回调，让用户来重试
* 请求过程 需要loading， 减少用户焦虑
* 提供一个无论成功展示还是异常展示都会触发的回调，以满足一些需求

## 3. 开始上手 (React)

这里使用 `react` + `antd` 别问我为什么,因为公司项目使用的是 `react`. 手动狗头.

3.1 默认展示 `loading` 状态, 但可以通过传入不同的 `loading` 来达到 `loading` 不同大小场景. 可能是页面级别的 `loading` 效果,也可能只是一个小小的 `div`

```js
    import { Spin, Result, Button } from 'antd';
    import { useState } from 'react'
    const Get = ({loading}) => {
    const [view, setView] = useState(loading || defaultLoading);
    // 默认Loading组件
    const defaultLoading = (
      <Spin
        style={{ margin: 'auto', textAlign: 'center', width: '100%' }}
        indicator={<LoadingOutlined style={{ fontSize: 24 }} spin />}
        tip="努力加载中..."
      ></Spin>);

        // 无论如何都是返回一个 component
        return view;
    }

```

3.2 通过传入不同的 `url` 来请求接口获取数据,以及 `error` 来展示不同的异常状态以满足不同的异常场景.

```js
import { useEffect, usecallback } from 'react'
import { Result, Button } from 'antd';
const Get = ({url,error,children}) => {

/**
 * TODO 错误展示Component
 * @param {() => {}} errorCd 按钮重试方法
 * @returns {JSX.Element}
 */
const defaultError = (errorCd) => {
  return (
    <Result
      status="500"
      title="错误"
      subTitle="发生预料之外的错误，请点击下方按钮重试！"
      extra={
        <Button type="primary" onClick={() => errorCd()}>
          重试
        </Button>
      }
    />
  );
};

 // 发起请求
const getData = useCallback(() => {
    request
      .get('/sslvpn-backstage/common/module/info')
      .then((res) => {
        const {errCode,data } = res
        if(errCode === 0) {
           // 数据获取成功 ,调用 父组件成功方法展示不同的 success 组件
            setView(children(res.data));
        } else {
            // 获取失败还是展示 error
            setView(error || defaultError(getData));
        }
      })
      .catch((e) => {
        // 触发错误效果显示 并为异常状态提供一个回调方法这里是 getData
        setView(error || defaultError(getData));
      });

  }, [url]);

useEffect(() => {
    try {
      // 发送请求获取数据
      getData();
    } catch (e) {
       // 失败 显示error
      setView(error || defaultError(getData));
      throw e;
    }
  }, []);
return view
}

```

3.3 最后我们增加 `completeHandle` 到 `setVIew` 方法后.

## 4. `POST` 请求

前面我们已经完成 `GET` 请求, 那么 `POST` 我们需要增加什么来达到 我们的目的呢? `POST` 只是比 `GET` 多了一个请求体数据 . 只需要在 增加 一个 `data` 来传入我们的 `POST` 请求数据,然后在发起一个对应的请求就可以了. 最后进行导出,就要可以使用了. 使用方法 :

```js
// Get 使用 这里展示的是一个 antd 的table
const get = {
    url: 'https://localhost:8000/api/users'
}
<Http.Get {...get} >
    {(data) => <Tables columns={columns} dataSource={data} />}
</Http.Get>

// Post使用
const post = {
    url: 'https://localhost:8000/api/users',
    data: {
        id: 1,
    }
}
<Http.Post {...post}>
    {(data) => <Tables columns={columns} dataSource={data} />}
</Http.Post>

```
看看最终效果：

`loading` 状态

![1629794331(1).jpg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2712cda6e4a249b997a5ac18d5a039ff~tplv-k3u1fbpfcp-watermark.image)

成功状态：

![图片.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ee212e4c8f8c48a582f94b8c209f93bc~tplv-k3u1fbpfcp-watermark.image)

失败状态：

![图片.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9d49f8866c7e44e7b6bd5e5c36a42d2c~tplv-k3u1fbpfcp-watermark.image)


完整代码：

```js
/*
 * TODO Http Get获取数据展示对应component
 * @Author: 小菜鸡
 * @Date: 2021-08-21 11:12:30
 * @Last Modified by: 小菜鸡
 * @Last Modified time: 2021-08-24 15:27:30
 */
import { useCallback, useEffect, useState } from 'react';
import { Spin, Result, Button } from 'antd';
import request from '@/utils/request';
import { LoadingOutlined } from '@ant-design/icons';

// 默认Loading
const defaultLoading = (
  <Spin
    style={{ margin: 'auto', textAlign: 'center', width: '100%' }}
    indicator={<LoadingOutlined style={{ fontSize: 24 }} spin />}
    tip="努力加载中..."
  ></Spin>
);

/**
 * TODO 页面发送错误展示内容
 * @param {() => {}} errorCd 按钮重试方法
 * @returns {JSX.Element}
 */
const defaultError = (errorCd: () => void) => {
  return (
    <Result
      status="500"
      title="错误"
      subTitle="发生预料之外的错误，请点击下方按钮重试！"
      extra={
        <Button type="primary" onClick={() => errorCd()}>
          重试
        </Button>
      }
    />
  );
};

/**
 * TODO Get获取数据展示页面
 * @param {JSX.Element} error 页面异常显示的效果
 * @param {JSX.Element} loading 加载中显示的loading效果
 * @param {string} 请求url
 * @param {() => {}} completeHandle 成功失败都会执行的回调
 * @returns {JSX.Element} 返回一个正常显示的component
 */
const Get = ({
  error,
  loading,
  url,
  completeHandle = () => {},
  children,
}) => {
  const [view, setView] = useState(loading || defaultLoading);
  const getData: () => void = useCallback(() => {
    request
      .get('/sslvpn-backstage/common/module/info')
      .then((res) => {
          // 模拟table数据
        setView(
          children([
            {
              key: '3',
              name: 'Joe Black',
              id: 11111,
              age: 32,
              mark: '120.212.12.1/20',
              createTime: '2020-09-12 08:20:09',
              address: '120.212.12.1/10',
            },
            {
              key: '2',
              name: 'Joe Black',
              age: 32,
              id: 111123,
              mark: '120.212.12.1/20',
              createTime: '2020-09-12 08:20:09',
              address: '120.212.12.1/10',
            },
            {
              key: '2212',
              name: 'Joe Black',
              id: 11434111,
              age: 32,
              mark: '120.212.12.1/20',
              createTime: '2020-09-12 08:20:09',
              address: '120.212.12.1/10',
            },
          ]),
        );
        completeHandle();
      })
      .catch((e) => {
        // 触发错误效果显示
        setView(error || defaultError(getData));
        completeHandle();
      });
  }, [url]);

  useEffect(() => {
    try {
      // 发送请求获取数据
      getData();
    } catch (e) {
      setView(error || defaultError(getData));
      completeHandle();
      throw e;
    }
  }, []);
  return view;
};

/**
 * TODO POST获取数据展示视图
 * @param {JSX.Element} error 页面异常显示的效果
 * @param {JSX.Element} loading 加载中显示的loading效果
 * @param {string} 请求url
 * @param {object} data POST请求中的 data
 * @param {() => {}} completeHandle 成功失败都会执行的回调
 * @returns {JSX.Element} 返回一个正常显示的component
 */
const Post = ({
  error,
  loading,
  url,
  completeHandle = () => {},
  children,
  data,
}) => {
  const [view, setView] = useState(loading || defaultLoading);
  useEffect(() => {
      // 发送请求获取数据
      const data = ['aa', 'bb', 'ss', 'fd'];
      const successView = children(data);
      setView(successView);
    } catch (e) {
      setView(error || defaultError(getData));
      errorHandle();
      throw e;
    }
  }, []);
  return view;
};
const Http = {
  Get,
  Post,
};
export default Http;

```