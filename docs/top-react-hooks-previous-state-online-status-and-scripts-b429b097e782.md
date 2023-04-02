# Top React 挂钩—以前的状态、在线状态和脚本

> 原文：<https://javascript.plainenglish.io/top-react-hooks-previous-state-online-status-and-scripts-b429b097e782?source=collection_archive---------14----------------------->

![](img/9fd7a4b4a613e9430605587692de6d87.png)

Photo by [Anika Huizinga](https://unsplash.com/@iam_anih?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应配方

React Recipes 附带了许多钩子，我们可以用来做各种事情。

`useMedia`钩子允许我们在 JavaScript 中添加媒体查询。

例如，我们可以写:

```
import React from "react";
import { useMedia } from "react-recipes";export default function App() {
  const columnCount = useMedia(
    ["(min-width: 1500px)", "(min-width: 1000px)", "(min-width: 600px)"],
    [5, 4, 3],
    2
  ); return (
    <div style={{ columns: columnCount }}>
      {Array(1000)
        .fill()
        .map((_, i) => (
          <p>{i}</p>
        ))}
    </div>
  );
}
```

我们有一个`useMedia`钩子来设置媒体查询。

第一个参数是一组媒体查询。

第二个是每个媒体查询的值数组。

第三个是要返回的默认值。

然后我们可以随心所欲地使用返回值。

在上面的例子中，我们使用媒体查询来设置列数。

`useOnClickOutside`钩子让我们监听元素外部的点击。

例如，我们可以写:

```
import React from "react";
import { useOnClickOutside } from "react-recipes";export default function App() {
  const ref = React.useRef();
  const [isModalOpen, setModalOpen] = React.useState(false); useOnClickOutside(ref, () => setModalOpen(false)); return (
    <div>
      {isModalOpen ? (
        <div ref={ref}>modal</div>
      ) : (
        <button onClick={() => setModalOpen(true)}>Open Modal</button>
      )}
    </div>
  );
}
```

我们在裁判面前使用了`useOnClickOutside`钩子。

ref 被传递给我们想要监视外部点击的元素。

当我们在一个元素外点击时，钩子接受一个回调来运行一些代码。

`useOnlineStatus`钩子让我们监听在线或离线事件，以查看当前状态。

例如，我们可以这样使用它:

```
import React from "react";
import { useOnlineStatus } from "react-recipes";export default function App() {
  const onlineStatus = useOnlineStatus(); return (
    <div>
      <h1>{onlineStatus ? "Online" : "Offline"}</h1>
    </div>
  );
}
```

我们调用了`useOnlineStatus`钩子来监听在线状态。

`usePrevious`钩子让我们获得先前设置的状态值。

例如，我们可以写:

```
import React from "react";
import { usePrevious } from "react-recipes";export default function App() {
  const [count, setCount] = React.useState(0); const prevCount = usePrevious(count); return (
    <div>
      <p>
        Now: {count}, before: {prevCount}
      </p>
      <button onClick={() => setCount(count => count + 1)}>Increment</button>
    </div>
  );
}
```

我们使用带有`count`状态的`usePrevious`钩子来返回`count`状态的先前值。

`prevCount`具有`count`的先前值。

钩子让我们用代码创建一个脚本标签。

例如，我们可以写:

```
import React from "react";
import { useScript } from "react-recipes";export default function App() {
  const [loaded, error] = useScript(
    "https://code.jquery.com/jquery-2.2.4.min.js"
  ); return (
    <div>
      <div>
        Script loaded: <b>{loaded.toString()}</b>
      </div>
      {loaded && !error && (
        <div>
          Script function call response: <b>{$().jquery}</b>
        </div>
      )}
    </div>
  );
}
```

我们用脚本的 URL 调用了`useScript`钩子。

我们加载了 jQuery 并使用`$().jquery`获得了 jQuery 版本。

`loaded`如果是`true`表示已经加载，

`error`有错误的话。

![](img/216e08f423ac2b26775f20ec910df181.png)

Photo by [Maarten van den Heuvel](https://unsplash.com/@mvdheuvel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

React Recipes 让我们可以使用媒体查询、加载脚本、观察在线状态以及获取状态的前一个值。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**