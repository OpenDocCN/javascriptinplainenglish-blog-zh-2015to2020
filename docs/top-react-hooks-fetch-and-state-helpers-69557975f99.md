# Top React 挂钩—获取和状态助手

> 原文：<https://javascript.plainenglish.io/top-react-hooks-fetch-and-state-helpers-69557975f99?source=collection_archive---------11----------------------->

![](img/9a5f834701b45f83ffcaaa7063501ea4.png)

Photo by [Dave Francis](https://unsplash.com/@davefrancis101?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应取钩

react-fetch-hook 允许我们用钩子从 API 中获取数据。

它的大小很小，并且包含了流和类型脚本类型。

要安装它，我们可以运行:

```
yarn add react-fetch-hook
```

或者:

```
npm i react-fetch-hook --save
```

然后我们可以通过写来使用它:

```
import React from "react";
import useFetch from "react-fetch-hook";export default function App() {
  const { isLoading, data } = useFetch("https://api.agify.io/?name=michael"); return isLoading ? <div>Loading...</div> : <>{JSON.stringify(data)}</>;
}
```

我们将`useFetch`挂钩用于我们想要从中获取数据的 API 端点的 URL。

`isLoading`表示加载状态，`data`表示数据。

它支持错误处理和多请求。

我们也可以用自定义的格式来格式化我们的数据。

# 反应悬挂器

react-hanger 库附带了各种钩子，我们可以用它们来做各种事情。

要安装它，我们运行:

```
yarn add react-hanger
```

我们可以使用`useInput`钩子来自动绑定表单值。

例如，我们可以写:

```
import React from "react";
import { useInput } from "react-hanger";export default function App() {
  const newTodo = useInput(""); return (
    <>
      <input type="text" value={newTodo.value} onChange={newTodo.onChange} />
      <p>{newTodo.value}</p>
    </>
  );
}
```

`useInput`钩子返回一个具有`value`和`onChange`属性的对象，我们可以将它们传递给各自的道具。

`useBoolean`让我们可以轻松地切换布尔值。

例如，我们可以写:

```
import React from "react";
import { useBoolean } from "react-hanger";export default function App() {
  const showCounter = useBoolean(true); return (
    <>
      <button onClick={showCounter.toggle}>toggle</button>
      <p>{showCounter.value.toString()}</p>
    </>
  );
}
```

`useNumber`钩子让我们设置一个数字，只要它在一个范围内。

例如，我们可以写:

```
import React from "react";
import { useNumber } from "react-hanger";export default function App() {
  const limitedNumber = useNumber(3, { lowerLimit: 0, upperLimit: 5 }); return (
    <>
      <button onClick={() => limitedNumber.increase()}> increase </button>
      <button onClick={() => limitedNumber.decrease()}> decrease </button>
      <p>{limitedNumber.value}</p>
    </>
  );
}
```

我们有一个带有属性为`lowerLimit`和`upperLimit`的对象的`useNumber`钩子。

然后我们可以使用`increase`和`decrease`方法来改变`limitedNumber`的`value`属性。

我们不能用它设定一个超出给定范围的数字。

我们还可以添加`loop`选项，并将其设置为`true`，这样当数字达到极限时就会循环。

如果我们移除钩子的第二个参数中的对象，那么数字可以设置为任意值:

```
import React from "react";
import { useNumber } from "react-hanger";export default function App() {
  const limitedNumber = useNumber(3); return (
    <>
      <button onClick={() => limitedNumber.increase()}> increase </button>
      <button onClick={() => limitedNumber.decrease()}> decrease </button>
      <p>{limitedNumber.value}</p>
    </>
  );
}
```

![](img/4c4c6d9b7671a1fd6f1bdbacf72713a3.png)

Photo by [James Hartono](https://unsplash.com/@james300402?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

react-fetch-hook 让我们可以轻松地从端点获取数据。

此外，我们可以使用 react-hanger 钩子，它包含各种状态助手。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**