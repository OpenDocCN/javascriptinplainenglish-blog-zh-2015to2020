# Top React 挂钩— Github 和 States

> 原文：<https://javascript.plainenglish.io/top-react-hooks-github-and-states-7461c025f11c?source=collection_archive---------6----------------------->

![](img/83a698976598d4a450c1b800b3730dcd.png)

Photo by [Miguel Ángel Sanz](https://unsplash.com/@maswdl95?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# @d2k/react-github

@d2k/react-github 是一系列让我们从 github 获取数据的 react 钩子。

要安装它，我们运行:

```
npm i @d2k/react-github --save
```

然后我们可以通过编写以下代码来使用`useLatestRelease`钩子:

```
import React from "react";
import { useLatestRelease } from "@d2k/react-github";export default function App() {
  const { release, loading, error } = useLatestRelease("facebook", "react"); return (
    <>
      <div>{JSON.stringify(release)}</div>
    </>
  );
}
```

我们通过使用带有用户和回购名称的`useLatestRelease`钩子来获取最新的错误。

然后`release`有一个带有发布数据的对象。

`loading`和`error`有加载和出错状态。

我们可以使用`useTaggedRelease`钩子从我们的应用程序中获得一个带标签的版本。

为了使用它，我们写:

```
import React from "react";
import { useTaggedRelease } from "@d2k/react-github";export default function App() {
  const { release, loading, error } = useTaggedRelease(
    "facebook",
    "react",
    "v16.8.4"
  );
  return (
    <>
      <div>{JSON.stringify(release)}</div>
    </>
  );
}
```

它还有一个`useForks`钩子来获取回购的叉。

第一个参数是用户。

第二个理由是回购。

第三个是标签版本。

例如，我们可以写:

```
import React from "react";
import { useForks } from "@d2k/react-github";export default function App() {
  const { forks, loading, error } = useForks("facebook", "react"); return (
    <>
      <div>{JSON.stringify(forks)}</div>
    </>
  );
}
```

得到反应的叉子。

# @d2k/react-localstorage

@d2k/react-localstorage 是一个带有操作本地存储钩子的包。

为了使用它，我们运行:

```
npm i @d2k/react-localstorage --save
```

来安装它。

然后我们可以通过写来使用它:

```
import React from "react";
import useLocalStorage from "@d2k/react-localstorage";export default function App() {
  const [firstName, setFirstName, removeFirstName] =   useLocalStorage(
    "firstName",
    "mary"
  );
  const [lastName, setLastName, removeLastName] = useLocalStorage(
    "lastName",
    "smith"
  ); return (
    <>
      <div>
        {firstName && lastName && (
          <p>
            Hello {firstName} {lastName}
          </p>
        )}
      </div>
    </>
  );
}
```

我们使用了返回当前值的`useLocalStorage`钩子，这是一个用给定的键更新项目的函数，并按此顺序删除带有给定键的项目。

关键是钩子的第一个参数。

默认值是第二个参数。

然后我们可以用它们来更新我们的本地存储，而不是直接操作它。

# 钩子状态

Hookstate 包允许我们在应用程序中全局管理状态。

它可以作为 Redux 或 Mobx 等状态管理解决方案的替代方案。

要安装它，我们运行:

```
npm install --save @hookstate/core
```

或者:

```
yarn add @hookstate/core
```

来安装它。

然后我们可以使用它附带的钩子来写:

```
import React from "react";
import { createState, useState } from "@hookstate/core";
const globalState = createState(0);export default function App() {
  const state = useState(globalState);
  return (
    <>
      <p>Counter value: {state.get()}</p>
      <button onClick={() => state.set(p => p + 1)}>Increment</button>
    </>
  );
}
```

我们使用`createState`函数来创建一个全局状态。

然后我们可以将它传递给 Hookstate 的`useState`钩子，这样我们就可以对它做一些事情。

我们调用`get`来获取当前状态。

我们调用`set`来设置新的状态。

![](img/b8e8339e1810d8206176a8db50a4f3b6.png)

Photo by [Miguel Ángel Sanz](https://unsplash.com/@maswdl95?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

从 Github 获取数据以及管理本地存储和应用程序状态有一些有用的挂钩。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**