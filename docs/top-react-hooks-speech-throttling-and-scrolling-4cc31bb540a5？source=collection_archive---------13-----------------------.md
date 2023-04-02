# 最重要的反应——语音、节流和滚动

> 原文：<https://javascript.plainenglish.io/top-react-hooks-speech-throttling-and-scrolling-4cc31bb540a5?source=collection_archive---------13----------------------->

![](img/b7e35d7bb4f82c58ce6ed8992f40c9ea.png)

Photo by [Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应配方

React Recipes 附带了许多钩子，我们可以用来做各种事情。

我们可以使用`useSpeechSynthesis`钩子让我们将语音合成添加到 React 应用程序中。

例如，我们可以写:

```
import React from "react";
import { useSpeechSynthesis } from "react-recipes";export default function App() {
  const [value] = React.useState("hello world");
  const [ended, setEnded] = React.useState(false);
  const onEnd = () => setEnded(true);
  const { cancel, speak, speaking, supported, voices } = useSpeechSynthesis({
    onEnd
  }); if (!supported) {
    return "Speech is not supported. Upgrade your browser";
  } return (
    <div>
      <button onClick={() => speak({ text: value, voice: voices[0] })}>
        Speak
      </button>
      <button onClick={cancel}>Cancel</button>
      <p>{speaking && "Voice is speaking"}</p>
      <p>{ended && "Voice has ended"}</p>
    </div>
  );
}
```

我们用`useSpeechSynthesis`钩子返回一个具有不同属性的对象。

`cancel`取消语音合成。

`speak`让电脑开始说话。

`speaking`表明它在说话。

`supported`表示是否支持语音合成。

有我们可以选择的声音。

我们在`speak`函数中使用它。

`useThrottle`钩子让我们只在给定的毫秒数后改变节流值。

例如，我们可以写:

```
import React from "react";
import { useThrottle, useInterval } from "react-recipes";const Count = ({ count }) => {
  const throttledCount = useThrottle(count, 950); return (
    <>
      <div>Value: {count}</div>
      <div>Throttled value: {throttledCount}</div>
    </>
  );
};export default function App() {
  const [count, setCount] = React.useState(0);
  useInterval(() => {
    setCount(count => count + 1);
  }, 1000); return (
    <>
      <Count count={count} />
    </>
  );
}
```

我们有`Count`组件，我们有`useThrottle`钩子来观看`count`道具。

第二个参数是要观察的毫秒数。

`useWhyDidYouUpdate`让我们观察组件更新的原因。

例如，我们可以写:

```
import React from "react";
import { useWhyDidYouUpdate } from "react-recipes";const Counter = React.memo(props => {
  useWhyDidYouUpdate("Counter", props);
  return <div>{props.count}</div>;
});export default function App() {
  const [count, setCount] = React.useState(0); return (
    <>
      <button onClick={() => setCount(count => count + 1)}>increment</button>
      <p>
        <Counter count={count} />
      </p>
    </>
  );
}
```

观看`count`道具更新。

我们使用`useWhyDidYouUpdate`来观察属性值的变化。

参数是我们添加到日志中的名称。

第二是价值。

`useWindowScroll`钩子让我们在窗口滚动时重新渲染。

例如，我们可以写:

```
import React from "react";
import { useWindowScroll } from "react-recipes";export default function App() {
  const { x, y } = useWindowScroll(); return (
    <>
      <div style={{ position: "fixed" }}>
        <div>x: {x}</div>
        <div>y: {y}</div>
      </div>
      {Array(1000)
        .fill()
        .map((_, i) => (
          <p>{i}</p>
        ))}
    </>
  );
}
```

我们有`useWindowScroll`来观察 React 组件中的滚动位置。

返回滚动位置的`x`和`y`坐标。

![](img/370002fbde239478d3a8ff0d9687fed8.png)

Photo by [John Baker](https://unsplash.com/@jlondonbaker?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

React Recipes 允许我们在应用程序中添加语音合成功能。

我们还可以用它来调节和观察适当变化，以及观察滚动位置。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**