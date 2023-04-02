# 反应模式—动画、图形和样式

> 原文：<https://javascript.plainenglish.io/react-patterns-animations-graphics-and-styles-aa704206d04?source=collection_archive---------10----------------------->

![](img/100d136da8e3fd0458d7ffd887d6b431.png)

Photo by [Erik-Jan Leusink](https://unsplash.com/@ejleusink?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解如何使用 React 创建简单的动画、添加矢量图形和样式。

# 反作用运动

我们可以用`react-motion`制作基本的动画。

它给了我们一个容易使用的 API 来创建任何动画。

我们通过运行以下命令来安装它:

```
npm install --save react-motion
```

然后我们可以通过写来使用它:

```
import React from "react";
import { Motion, spring } from "react-motion";export default function App() {
  return (
    <div className="App">
      <Motion defaultStyle={{ opacity: 0.01 }} style={{ opacity: spring(1) }}>
        {interpolatingStyle => <p style={interpolatingStyle}>hello world</p>}
      </Motion>
    </div>
  );
}
```

我们只是传递了一些道具来创建我们的动画。

当我们开始动画时,`opacity`是不透明的。

然后我们有`style`来改变不透明度从 0.01 到 1。

我们有一个函数，用从`interpolatingStyle`参数中计算出的样式来呈现我们想要的子元素。

# 可缩放矢量图形

可缩放矢量图形(SVG)是可以添加到 web 应用程序中的图形。

它是通过添加创建图形的标签来创建的。

我们可以将 SVG 代码直接放入 HTML 代码中。

这也意味着我们可以将相同的代码放入 React 组件中。

这可以使他们充满活力。

例如，我们可以创建一个`Circle`组件并使用它:

```
import React from "react";const Circle = ({ x, y, radius, fill }) => (
  <svg>
    <circle cx={x} cy={y} r={radius} fill={fill} />
  </svg>
);export default function App() {
  return (
    <div className="App">
      <Circle x={100} y={100} radius={50} fill="green" />
    </div>
  );
}
```

`svg`标签表明它是一个 SVG。

然后在里面添加我们想要的形状。

`cx`有圆心的 x 坐标。

`cy`有圆心的 y 坐标。

`r`有圆的半径。

`fill`是圆的填充颜色。

这使得创建动态形状变得容易。

# JS 中的 CSS

为了让我们的组件看起来更好，我们必须设计它们的样式。

要设计它们的样式，我们必须使用 CSS。

在 React 应用程序中，我们可以将 CSS 直接放入 JavaScript 文件中，也可以从外部样式表导入它们。

如果它们是静态的，那么它们应该是不变的。否则，我们可以将它们设置为内联。

CS 的痛苦在于选择器的修改可能不准确。

在样式和客户端之间共享常量可能是一个挑战，因为我们不能显式地导入变量。

很难控制样式、规则和路径的各种组合。

这意味着我们必须修改浏览器中的 CSS 来使它们正确。

文件之间也没有隔离，这意味着风格可能会冲突。

# 内嵌样式

有了 React，我们可以将样式内联。这意味着我们可以有选择地应用于我们想要的确切元素。

例如，我们可以写:

```
import React from "react";
const style = {
  color: "green",
  backgroundColor: "orange"
};
const Button = () => <button style={style}>Click me!</button>;export default function App() {
  return (
    <div className="App">
      <Button />
    </div>
  );
}
```

我们有了`style`对象，它应用于`Button`。

样式通过`style`道具应用。

当我们以这种方式应用样式时，我们可以使我们的样式动态化:

```
import React, { useState } from "react";export default function App() {
  const [fontSize, setFontSize] = useState(40);
  return (
    <div className="App">
      <button onClick={() => setFontSize(size => (size === 40 ? 20 : 40))}>
        toggle
      </button>
      <p style={{ fontSize }}>foo</p>
    </div>
  );
}
```

我们有一个按钮，可以用来切换“foo”文本的大小。

这是因为我们设置了`fontSize`状态，并将其作为样式应用于 p 元素内容的大小。

这种方法有局限性。

不能用`:hover`这样的伪选择器。

另外，我们不能使用媒体查询。

在 JavaScript 中我们也不能同时拥有两个属性，但是我们可以用 CSS 做到这一点。

我们可以这样写:

```
display: -webkit-flex; 
display: flex;
```

内联样式也很难调试。如果它们是动态应用的，就很难用 Chrome 开发工具来检查它们。

![](img/6122863579b766b94bfe038fa1e08c3a.png)

Photo by [Ramiz Dedaković](https://unsplash.com/@ramche?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一些库创建动画。

此外，我们可以用`svg`元素添加矢量图形。

内联样式可以应用于 React 组件，因此我们可以获得动态样式。

## **简明英语 JavaScript**

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**