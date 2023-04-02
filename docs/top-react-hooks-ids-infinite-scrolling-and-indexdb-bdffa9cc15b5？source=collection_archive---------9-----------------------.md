# Top React 挂钩— IDs、无限滚动和 IndexDB

> 原文：<https://javascript.plainenglish.io/top-react-hooks-ids-infinite-scrolling-and-indexdb-bdffa9cc15b5?source=collection_archive---------9----------------------->

![](img/a263d31213e84b04fb1653b040b37a16.png)

Photo by [Bruno Aguirre](https://unsplash.com/@elcuervo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应-使用-标识-挂钩

react-use-id-hook 库让我们可以在 react 组件中轻松地创建惟一的 id。

我们可以通过运行以下命令来安装它:

```
npm i react-use-id-hook
```

然后我们可以通过写来使用它:

`index.js`

```
import React from "react";
import ReactDOM from "react-dom";
import { IdProvider } from "react-use-id-hook";import App from "./App";const rootElement = document.getElementById("root");
ReactDOM.render(
  <IdProvider>
    <App />
  </IdProvider>,
  rootElement
);
```

`App.js`

```
import React from "react";
import { useId } from "react-use-id-hook";export default function App() {
  const id = useId();
  const [value, setValue] = React.useState(); return (
    <form>
      <input
        id={id}
        type="checkbox"
        checked={value}
        onChange={ev => setValue(ev.target.checked)}
      />
      <label htmlFor={id}>Click me</label>
    </form>
  );
}
```

我们使用`useId`钩子来生成我们的 ID。

然后我们把它传到道具里或者任何我们想用的地方。

我们必须记住用`IdProvider`包装我们的应用程序，这样我们就可以使用钩子。

# 反应-使用-idb

react-use-idb 允许我们使用 IndexDB 来存储我们的数据，而不使用本地库。

要安装它，我们运行:

```
npm i react-use-idb
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useIdb } from "react-use-idb";export default function App() {
  const [value, setValue] = useIdb("key", "foo"); return (
    <div>
      <div>Value: {value}</div>
      <button onClick={() => setValue("bar")}>bar</button>
      <button onClick={() => setValue("baz")}>baz</button>
    </div>
  );
}
```

`useIdb`钩子接受一个键和初始值。

它返回一个带有值的数组和一个按此顺序设置值的函数。

然后我们可以像在`onClick`处理程序中那样使用它。

设置数据需要一个参数。

# 反应-使用-无限加载器

react-use-infinite-loader 包允许我们在 react 应用程序中添加无限滚动。

要安装它，我们运行:

```
yarn add react-use-infinite-loader
```

然后我们可以通过写来使用它:

```
import React from "react";
import useInfiniteLoader from "react-use-infinite-loader";export default function App() {
  const [canLoadMore, setCanLoadMore] = React.useState(true);
  const [data, setData] = React.useState([]);
  const loadMore = React.useCallback(() => {
    setTimeout(() => {
      setCanLoadMore(true);
      setData(currentData => [
        ...currentData,
        ...Array(20)
          .fill()
          .map((_, i) => i)
      ]);
    }, 1000);
  });
  const { loaderRef } = useInfiniteLoader({ loadMore, canLoadMore }); return (
    <>
      {data.map(data => (
        <div>{data}</div>
      ))}
      <div ref={loaderRef} />
    </>
  );
}
```

我们添加了`loadMore`函数来加载我们的数据。

我们将数据加载代码放在回调中，以便可以缓存调用。

数据加载代码只是将更多的数据推入`data`数组。

然后我们有了`useInfiniteLoader`让我们为元素创建一个 ref 来监视要加载的条目。

钩子通过`loadMore`函数获取一个对象来加载更多的数据。

`canLoadMore`告诉库我们是否有更多的数据要加载。

我们将它传递到 div 中，以便在加载 div 时加载新数据。

![](img/6640a119c779dc6fc2496b26523a6023.png)

Photo by [Alvan Nee](https://unsplash.com/@alvannee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

react-use-id-hook 允许我们在 react 组件中创建一个 id。

react-use-idb 是一个库，让我们与浏览器的 indexedDB 数据库进行交互。

react-use-infinite-loader 库允许我们向 react 应用程序添加无限滚动。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**