# 顶部反应挂钩—网络、尺寸和滚动

> 原文：<https://javascript.plainenglish.io/top-react-hooks-network-dimensions-and-scroll-61981a013cd0?source=collection_archive---------8----------------------->

![](img/141566a452baa5dcaaf4b1b05e8e3ffe.png)

Photo by [Anders Wideskott](https://unsplash.com/@wideshot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# @rehooks/network-status

@rehooks/network-status 挂钩让我们可以在 React 应用程序中获得网络状态。

为了使用它，我们运行:

```
yarn add @rehooks/network-status
```

然后我们可以使用`useNetworkStatus`钩子来获取网络信息:

```
import React from "react";
import useNetworkStatus from "@rehooks/network-status";export default function App() {
  const connection = useNetworkStatus();
  return (
    <div>
      <div>downlink: {connection.downlink}</div>
      <div>effectiveType: {connection.effectiveType}</div>
      <div>rtt: {connection.rtt}</div>
      <div>saveData: {connection.saveData ? "yes" : "no"}</div>
    </div>
  );
}
```

`downlink`有上传速度。

`effectiveType` hs 连接类型。

`rtt`是往返延迟，即发送一个信号所需的时间加上信号被确认所需的时间。

# @rehooks/online-status

@rehooks/online-status 是一个获取一个 app 在线状态的包。

它监听在线或离线事件来获取状态。

要安装它，我们可以运行:

```
yarn add @rehooks/online-status
```

然后我们可以通过写来使用它:

```
import React from "react";
import useOnlineStatus from "@rehooks/online-status";export default function App() {
  const onlineStatus = useOnlineStatus();
  return (
    <div>
      <h1>{onlineStatus ? "Online" : "Offline"}</h1>
    </div>
  );
}
```

我们可以使用`useOnlineStatus`钩子来获取我们的应用程序的在线状态。

# @ re hooks/窗口-滚动-位置

我们可以使用@ re hooks/window-scroll-position 钩子来观察我们的应用程序中的滚动位置。

为了安装这个包，我们运行:

```
yarn add @rehooks/window-scroll-position
```

然后我们可以用包买写作:

```
import React from "react";
import useWindowScrollPosition from "@rehooks/window-scroll-position";export default function App() {
  const options = {
    throttle: 100
  };
  let position = useWindowScrollPosition(options);
  return (
    <>
      <div style={{ position: "fixed" }}>{JSON.stringify(position)}</div>
      {Array(1000)
        .fill()
        .map((_, i) => (
          <p>{i}</p>
        ))}
    </>
  );
}
```

我们使用`useWindowScrollPosition`挂钩来观察滚动位置。

我们还传递了一个选项来限制位置监视。

然后当我们滚动时，我们会看到`position`的`x`和`y`属性发生了变化。

# @rehooks/window-size

我们可以使用@ re books/window-size 包来监视窗口大小的变化。

要安装它，我们运行:

```
yarn add @rehooks/window-size
```

然后我们可以通过写来使用它:

```
import React from "react";
import useWindowSize from "@rehooks/window-size";export default function App() {
  let windowSize = useWindowSize(); return (
    <>
      <div>{JSON.stringify(windowSize)}</div>
    </>
  );
}
```

然后我们使用`useWindowSize`钩子来获得窗口尺寸。

`innerHeight`具有水平滚动条高度的内部高度，以像素为单位。

`innerWidth`具有以像素为单位的带有垂直滚动条的窗口的内部宽度。

`outerHeight`以像素为单位显示整个浏览器窗口的高度。

`outerHeight`以像素为单位显示整个浏览器窗口的宽度。

它们都包括边栏和其他边框。

![](img/b90468b73a2b13b4c9f2e59bb3df5d74.png)

Photo by [Lynda Hinton](https://unsplash.com/@lyndaann1975?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用各种钩子获得网络状态、滚动位置和窗口大小。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**