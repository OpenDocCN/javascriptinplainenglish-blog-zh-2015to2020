# Top React 挂钩—浏览器 API

> 原文：<https://javascript.plainenglish.io/top-react-hooks-browser-apis-1c76931ef245?source=collection_archive---------13----------------------->

![](img/63ee29a2400e1f4641d9bcc78949c281.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# @ 21kb/react-device-orientation-hook

[@ 21kb/react-device-orientation-](http://twitter.com/21kb/react-device-orientation-)钩子让我们获得屏幕的方向和角度。

要使用它，我们通过运行以下命令来安装它:

```
npm install --save @21kb/react-device-orientation-hook
```

或者:

```
yarn add @21kb/react-device-orientation-hook
```

然后我们可以通过写来使用它:

```
import React from "react";
import useOrientation from "@21kb/react-device-orientation-hook";export default function App() {
  const state = useOrientation({
    angle: 0,
    type: "landscape-primary"
  });
  const { angle, type } = state; return (
    <div className="App">
      {angle} {type}
    </div>
  );
}
```

我们用`type`属性设置默认的角度和方向。

然后我们可以用`useOrientation`钩子返回它。

# @21kb/react-notification-hook

我们可以使用通知挂钩在浏览器中显示本地通知。

要安装它，我们运行:

```
npm install --save @21kb/react-notification-hook
```

或者:

```
yarn add @21kb/react-notification-hook
```

然后我们可以通过写来使用它:

```
import React from "react";
import useNotification from "@21kb/react-notification-hook";export default function App() {
  const notify = useNotification({
    title: "hello world",
    options: {
      dir: "rtl"
    }
  }); return <button onClick={notify()}>Notify me!</button>;
}
```

`useNotificatiin`钩子返回一个函数让我们显示一个通知。

`title`具有通知的标题。

`options`包括方向的`dir`。

# @21kb/react-online-status-hook

@21kb/react-online-status-hook 包让我们获得应用程序的在线状态。

我们可以使用`useOnlineStatus`或`useOfflineStatus`钩子来获取在线状态。

我们安装了:

```
npm install --save @21kb/react-online-status-hook
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useOnlineStatus } from "[@21kb/react-online-status-hook](http://twitter.com/21kb/react-online-status-hook)";
export default function App() {
  const status = useOnlineStatus(); return <div>You are currently {status ? "online" : "offline"}.</div>;
}
```

`useOfflineStatus`做同样的事情。

例如，我们可以写:

```
import React from "react";
import { useOfflineStatus } from "@21kb/react-online-status-hook";
export default function App() {
  const status = useOfflineStatus(); return <div>You are currently {status ? "online" : "offline"}.</div>;
}
```

# @ 21kb/react-page-visible-钩子

@21kb/react-page-visible-hook 包为我们提供了获取应用程序页面可见性状态的钩子。

要安装它，我们运行:

```
npm install --save @21kb/react-page-visible-hook
```

或者:

```
yarn add @21kb/react-page-visible-hook
```

来安装它。

然后我们可以通过写来使用它:

```
import React from "react";
import useVisible from "@21kb/react-page-visible-hook";export default function App() {
  const state = useVisible();
  const { visibilityState } = state; return <>{visibilityState}</>;
}
```

挂钩返回我们应用程序的可见性状态。

该值可以是`'visible'`或`'prerender'`。

# @21kb/react-dom-status-hook

@21kb/react-dom-status-hook 包为我们提供了一个钩子来获取我们的应用程序的就绪状态。

我们可以通过运行以下命令来安装它:

```
npm install --save @21kb/react-dom-status-hook
```

或者:

```
yarn add @21kb/react-dom-status-hook
```

然后我们可以通过编写来使用`useDomStatus`钩子:

```
import React from "react";
import useDomStatus from "@21kb/react-dom-status-hook";export default function App() {
  const { readyState } = useDomStatus(); return <div>{readyState}</div>;
}
```

它返回一个带有`readyState`属性的对象来获取 DOM 加载状态。

![](img/05a03357ffc89b4e595911d87d6698ed.png)

Photo by [Lily Banse](https://unsplash.com/@lvnatikk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用各种钩子来使用各种浏览器 API。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**