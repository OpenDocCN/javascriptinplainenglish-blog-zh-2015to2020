# 顶部反应挂钩—热键、可见性、宽度和元标签

> 原文：<https://javascript.plainenglish.io/top-react-hooks-hotkeys-visibility-width-and-meta-tags-ce12dce0b1e5?source=collection_archive---------13----------------------->

![](img/91c6471865162549b28882b4dcbfea9d.png)

Photo by [Sergey Pesterev](https://unsplash.com/@sickle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

钩子在我们的反应应用程序中包含我们的逻辑代码。

我们可以创建自己的钩子，并使用其他人提供的钩子。

在本文中，我们将查看一些有用的 React 钩子。

# 反应-热键-钩子

反应-热键-钩子库有一个钩子，让我们听热键按下。

要安装它，我们运行:

```
npm install react-hotkeys-hook
```

或:

```
yarn add react-hotkeys-hook
```

然后我们可以通过书写来使用它:

```
import React from "react";
import { useHotkeys } from "react-hotkeys-hook";export default function App() {
  const [count, setCount] = React.useState(0);
  useHotkeys("ctrl+\\", () => setCount(prevCount => prevCount + 1)); return <p>Pressed {count} times.</p>;
}
```

`useHotkeys`挂钩让我们可以听到组合键的按压声。

第一个参数是组合键。

第二个是按下组合键时运行的回调。

# 反应-交叉-可见-钩子

“反应-相交-可见-钩子”库允许我们检测组件的可见性。

要使用它，我们可以通过运行以下命令来安装它:

```
npm i react-intersection-visible-hook
```

然后我们可以通过书写来使用它:

```
import React from "react";
import useVisibility from "react-intersection-visible-hook";export default function App() {
  const nodeRef = React.useRef(null);
  const visibility = useVisibility(nodeRef); return (
    <>
      <div className="App" ref={nodeRef}>
        <p style={{ position: "fixed" }}>
          {visibility.isIntersecting && visibility.isIntersecting.toString()}
        </p>
        <p>hello</p>
      </div>
    </>
  );
}
```

我们创建参考，然后将其传递给`useVisibility`钩子。

然后我们把裁判传给`ref`道具观看比赛。

然后我们可以使用它和`visible.isIntersecting`属性来检查 div 的可见性。

# 反应-介质-钩子

反应-媒体-钩子库让我们观察屏幕的尺寸。

要安装它，我们运行:

```
yarn add react-media-hook
```

或:

```
npm i react-media-hook --save
```

然后我们可以通过书写来使用它:

```
import React from "react";
import { useMediaPredicate } from "react-media-hook";export default function App() {
  const biggerThan300 = useMediaPredicate("(min-width: 300px)"); return <div>{biggerThan300 && <button>click me</button>}</div>;
}
```

我们把媒体查询传到`useMediaPredicate`钩子上来观察屏幕的宽度。

如果屏幕宽度为 300px 或更宽，它将返回`true`。

然后，给定宽度，我们可以展示一些东西。

# 对元标签挂钩做出反应

反应元标签钩子让我们可以为元标签添加各种属性。

我们可以通过运行以下命令安装它来使用它:

```
npm install react-metatags-hook
```

或:

```
yarn add react-metatags-hook
```

然后我们可以通过书写来使用它:

```
import React from "react";
import useMetaTags from "react-metatags-hook";const Page = ({ match }) => {
  const {
    params: { id },
    url
  } = match;
  useMetaTags(
    {
      title: `Page Title`,
      description: `A pahe with id: ${id}`,
      charset: "utf8",
      lang: "en",
      metas: [
        { name: "keywords", content: "foo, bar" },
        { name: "robots", content: "index, follow" },
        { name: "DC.Title", content: "Dublin Core Title" },
        { name: "url", content: `http://example.com${url}` },
        { property: "fb:app_id", content: "1234567890" },
        { "http-equiv": "Cache-Control", content: "no-cache" }
      ],
      links: [
        { rel: "canonical", href: "http://example.com" },
        { rel: "icon", type: "image/ico", href: "/favicon.ico" },
        {
          rel: "apple-touch-icon",
          sizes: "72x72",
          type: "image/png",
          href: "/apple-72.png"
        }
      ],
      openGraph: {
        title: "Page Title",
        image: "http://example.com/ogimage.jpg",
        site_name: "My Site"
      },
      twitter: {
        card: "summary",
        creator: "[@you](http://twitter.com/you)",
        title: "Page Title"
      }
    },
    [id, url]
  );
  return <>hello</>;
};export default function App() {
  return (
    <div>
      <Page match={{ params: { id: 1 }, url: "example" }} />
    </div>
  );
}
```

我们只是通过创建一个具有有效元标签属性的对象来使用它。

包括`title`、`description`、`charset`、`lang`、`of:*`和`twitter:*`属性。

我们可以把所有的东西放进去，它们会被添加到元标签中。

![](img/4fda46b6876306e5a1791be61f898ba8.png)

Photo by [ian dooley](https://unsplash.com/@sadswim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

反应-热键-钩子库让我们观察热键按压。

反应-交叉-可见-钩子让我们观察元素的可见性。

反应-媒体-挂钩让我们检查各种屏幕尺寸。

反应元标签钩子让我们可以动态添加元标签。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**