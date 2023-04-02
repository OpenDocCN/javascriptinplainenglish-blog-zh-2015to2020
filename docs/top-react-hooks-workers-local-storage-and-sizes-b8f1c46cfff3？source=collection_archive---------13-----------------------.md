# 顶级反应挂钩—工人、本地存储和尺寸

> 原文：<https://javascript.plainenglish.io/top-react-hooks-workers-local-storage-and-sizes-b8f1c46cfff3?source=collection_archive---------13----------------------->

![](img/454e30d1c1097fd84c8496ae4d501bbc.png)

Photo by [sol](https://unsplash.com/@solimonster?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 使用工人

useWorker 挂钩让我们可以在 React 应用中轻松使用 web workers。

要安装它，我们运行:

```
npm install --save @koale/useworker
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useWorker } from "@koale/useworker";const numbers = [...Array(1000000)].map(e => Math.random() * 1000000);
const sortNumbers = nums => nums.sort();export default function App() {
  const [sortWorker] = useWorker(sortNumbers); const runSort = async () => {
    const result = await sortWorker(numbers);
    console.log("End.");
  }; return (
    <>
      <button type="button" onClick={runSort}>
        Run Sort
      </button>
    </>
  );
}
```

我们有一个大的`numbers`数组需要排序。

我们想在后台这样做，这样我们就可以使用我们的应用程序做其他事情。

一旦我们点击按钮，tzhe`runSort`函数运行，它使用`sortWorker`函数进行排序。

我们只是用`useWorker`传递我们想要运行的长时间运行的函数。

一旦完成，我们将记录`'End.'`来表明它已经完成。

# @rehooks/component-size

@rehooks/component-size 是一个钩子，它让我们获得组件的大小。

为了使用它，我们运行:

```
yarn add @rehooks/component-size
```

来安装它。

然后我们通过书写来使用它:

```
import React from "react";
import useComponentSize from "@rehooks/component-size";export default function App() {
  const ref = React.useRef(null);
  const size = useComponentSize(ref);
  const { width, height } = size;
  let imgUrl = `https://via.placeholder.com/${width}x${height}`; return (
    <div>
      <img ref={ref} src={imgUrl} style={{ width: "100vw", height: "100vh" }} />
    </div>
  );
}
```

我们设置图像大小来填充整个屏幕。

我们将 ref 传递给图像，这样我们就可以用`useComponentSize`钩子得到图像的大小。

那么`width`和`height`将具有图像的最新宽度和高度值。

# 重新书签/文档可见性

我们可以使用 rebooks/document-visibility 包来获得页面的可见性。

为了使用它，我们运行:

```
yarn add @rehooks/document-visibility
```

来安装它。

然后我们可以通过写来使用它:

```
import React from "react";
import useDocumentVisibility from "@rehooks/document-visibility";export default function App() {
  const documentVisibility = useDocumentVisibility(); return <div>{documentVisibility}</div>;
}
```

我们只需调用钩子，然后获取可见性值。

# `@rehooks/input-value`

@rehooks/input-value 包有一个钩子，可以让我们自动将输入值绑定到一个状态。

要安装它，我们运行:

```
yarn add @rehooks/input-value
```

然后我们可以写:

```
import React from "react";
import useInputValue from "@rehooks/input-value";export default function App() {
  let name = useInputValue("mary");
  return (
    <>
      <p>{name.value}</p>
      <input {...name} />
    </>
  );
}
```

去使用它。

我们得到用`value`属性输入的值。

我们传递所有的属性作为输入的道具。

我们将默认值传递给钩子。

# @ re books/local-storage

@rehooks/local-storage 允许我们在 React 应用程序中使用一些钩子来操作本地存储。

要安装它，我们运行:

```
yarn add @rehooks/local-storage
```

或者:

```
npm i @rehooks/local-storage --save
```

然后我们可以通过写来使用它:

```
import React from "react";
import { writeStorage, useLocalStorage } from "@rehooks/local-storage";export default function App() {
  const [counterValue] = useLocalStorage("i", 0);
  return (
    <>
      <p>{counterValue}</p>
      <button onClick={() => writeStorage("i", counterValue + 1)}>
        increment
      </button>
    </>
  );
}
```

我们用`useLocalStorage`钩子得到这个值。

第一个论点是关键。第二个是初始值。

`writeStorage`让我们分别用给定的键和值来写值。

![](img/5004d19713c9cd3e07588e74e0716b46.png)

Photo by [David Siglin](https://unsplash.com/@dsiglin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

这对于获取组件大小、操作本地存储和绑定输入值非常有用。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**