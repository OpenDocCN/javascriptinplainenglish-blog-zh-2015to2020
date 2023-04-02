# 顶部反应挂钩—悬停、聚焦和阵列

> 原文：<https://javascript.plainenglish.io/top-react-hooks-hover-focus-and-array-663b0bad5d73?source=collection_archive---------5----------------------->

![](img/3d2de8dac76fcf042d01991a6360e28f.png)

Photo by [Paul Skorupskas](https://unsplash.com/@pawelskor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 逃避反应

React hookedUp 是一个有很多钩子的库，让我们的生活更轻松。

要安装它，我们运行:

```
npm install react-hookedup --save
```

或者:

```
yarn add react-hookedup
```

`useInput`钩子让我们用一个钩子来处理值的变化。

例如，我们可以写:

```
import React from "react";
import { useInput } from "react-hookedup";export default function App() {
  const newTodo = useInput(""); return (
    <div>
      <input {...newTodo.bindToInput} />
      <input {...newTodo.bind} />
      <p>{newTodo.value}</p>
    </div>
  );
}
```

我们有两个输入。

它们都使用由`useInput`返回的`newTodo`对象的属性来让我们处理输入值的变化。

`bindToInput`将`value`和`onChange`绑定到具有`e.target.value`的输入。

`bind`将`value`和`onChange`属性绑定到使用`onChange`中`e`的输入。

还有清除输入值的`clear`方法。

`useFocus`钩子让我们检测一个元素是否在焦点上。

要使用它，我们可以写:

```
import React from "react";
import { useFocus } from "react-hookedup";export default function App() {
  const { focused, bind } = useFocus(); return (
    <div>
      <input {...bind} />
      <p>{focused ? "focused" : "not focused"}</p>
    </div>
  );
}
```

`useFocus`钩子返回一个具有`focused`和`bind`属性的对象。

`bind`对象传递到输入中，这样我们可以观察输入的焦点状态。

然后我们可以用`focused`属性获得焦点状态。

`useHover`钩子让我们观察一个元素的悬停状态。

我们可以通过书写来使用它:

```
import React from "react";
import { useHover } from "react-hookedup";export default function App() {
  const { hovered, bind } = useHover(); return (
    <div>
      <p>{hovered ? "hovered" : "not hovered"}</p>
      <input {...bind} />
    </div>
  );
}
```

像使用`useFocus`一样，我们将`bind`属性传递给输入。

通过这种方式，我们可以观察输入是否处于悬停状态。

然后我们可以用`hovered`属性得到悬停状态。

挂钩让我们可以轻松地管理数组状态。

例如，我们可以写:

```
import React from "react";
import { useArray } from "react-hookedup";export default function App() {
  const { add, clear, removeIndex, value } = useArray(["foo", "bar"]); return (
    <div>
      <button onClick={() => add("qux")}>add</button>
      <button onClick={() => clear()}>clear</button>
      <button onClick={() => removeIndex(value.length - 1)}>
        remove index
      </button>
      <p>{value.join(", ")}</p>
    </div>
  );
}
```

我们使用`useArray`钩子返回一个数组作为它的初始值。

然后，它返回一个具有我们可以使用的各种属性的对象。

让我们将一个项目添加到数组中。

让我们清空数组。

`removeIndex`让我们根据索引删除一个项目。

`value`有数组的值。

它还返回`removeById`属性，让我们通过 ID 删除一个项目。

例如，我们可以写:

```
import React from "react";
import { useArray } from "react-hookedup";export default function App() {
  const { add, clear, removeById, value } = useArray([
    { id: 1, name: "foo" },
    { id: 2, name: "baz" }
  ]); return (
    <div>
      <button onClick={() => add({ id: value.length + 1, name: "qux" })}>
        add
      </button>
      <button onClick={() => clear()}>clear</button>
      <button onClick={() => removeById(value.length)}>remove by id</button>
      <p>{value.map(v => v.name).join(", ")}</p>
    </div>
  );
}
```

我们有一个数组，其中的条目具有`id`属性。

然后我们可以用 ID 调用`removeById`来根据 ID 移除项目。

![](img/1fd8b958d968289e2f9e3c7e0b61c423.png)

Photo by [Anders Wideskott](https://unsplash.com/@wideshot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

React hookedUp 有用于跟踪悬停、焦点和数组状态的有用钩子。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**