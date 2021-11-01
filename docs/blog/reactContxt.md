# 前言

普遍的认为是 `Redux` 在使用上比较繁琐。下面是简单使用 `hooks` 来实现状态共享。

## `createContext`

第一步我们使用 `createContext` 创建一个全局的状态(store)。

```js
import { createContext } from "react";

// 为当前的 theme 创建一个 context（“light”为默认值）
const GlobalContext = createContext('light');

const GlobalStore = ({ children }) => {
  return <GlobalContext.Provider>{children}</GlobalContext.Provider>;
};

export default GlobalStore;
```
