# 在 React 中制作可拖动组件

> 原文：<https://javascript.plainenglish.io/making-draggable-components-in-react-4c6d4d1df95f?source=collection_archive---------7----------------------->

随着用户交互在现代应用程序中变得越来越普遍，在应用程序中实现可拖动的组件有时会很好。想想 Trello 或者 Wix 这样的网站建设者。他们有拖放元素，这使得它对用户来说简单而直观。

大家好！在这篇文章中，我将使用 [react-draggable package](https://www.npmjs.com/package/react-draggable) 演示一些在 React 中制作可拖动组件的简单方法。

![](img/394cdff182e54d255b68eab45ecc5d55.png)

# 步骤 1:安装 npm 软件包

使用`npx create-react-app my-app`创建 React 应用程序后，运行:

```
npm install react-draggable
```

# 步骤 2:添加一个可拖动的组件

在`App.js`中，只需导入可拖动的组件:

```
import Draggable from 'react-draggable';
```

然后将您希望可拖动的元素包装在其中，如下所示:

```
<Draggable>
    <div className="box">
        <div>Move me around!</div>
    </div>
</Draggable>
```

我们可以给`box`添加一些 CSS:

```
.box {
  position: absolute;
  cursor: move;
  color: black;
  max-width: 215px;
  border-radius: 5px;
  padding: 1em;
  margin: auto;
  user-select: none;
}
```

# 就是这样！

这很简单，不是吗？现在让我们讨论一下 Draggable 组件中一些很酷的属性，您可以添加这些属性来对拖动进行更多的定制。

# 1.在一个轴上拖动

默认情况下,`axis`属性被设置为`both`,所以从上面的例子可以看出，它允许组件被拖动到任何方向。可以设置的其他值有:

*   **axis="x"** :组件只能水平拖动。
*   **axis="y"** :组件只能垂直拖动。
*   **axis="none"** :组件不能拖动。

# 2.跟踪可拖动的位置

每当组件被拖动时，就会触发`onDrag`事件处理程序。我们可以用它来跟踪组件的位置。

首先，导入`useState`挂钩:

```
import React, { useState } from "react";
```

然后，初始化`position`状态来存储我们的位置。

```
const [position, setPosition] = useState({ x: 0, y: 0 });
```

接下来，我们的“trackPos”函数将在触发`onDrag`时更新我们的`position`。

```
const trackPos = (data) => {
    setPosition({ x: data.x, y: data.y });
 };
```

而我们会在`onDrag`被触发的时候调用这个函数。

```
<Draggable onDrag={(e, data) => trackPos(data)}>
   <div className="box">
       <div>Here's my position...</div>
       <div>
            x: {position.x.toFixed(0)}, y: {position.y.toFixed(0)}
       </div>
   </div>
</Draggable>
```

# 3.仅在手柄上可拖动

我们可以包含`handle`属性，只允许在该手柄上拖动。例如:

```
<Draggable handle="#handle">
   <div className="box">
      <span id="handle">Drag here</span>
         <div style={{ padding: "1em" }}>Cannot drag here</div>
   </div>
</Draggable>
```

# 结论

在本文中，我们将介绍 react-draggable 以及如何使用它来制作可拖动的组件。我们还讨论了它的一些属性，这些属性可以进一步定制可拖动组件。拥有可拖动的元素有助于构建交互式应用。看看我做的这个迷你项目，它是一个利用可拖动特性的[公告板应用](https://victoria-lo.github.io/bulletin-board/)。

有关 react-draggable 的更多信息，请查看其文档[这里](https://www.npmjs.com/package/react-draggable)。希望这篇文章有所帮助。如果是请点赞分享。欢迎在下面的评论中提出任何问题或分享你用这个包做的任何项目。干杯！

*最初发表于*[T5【https://lo-victoria.com】](https://lo-victoria.com/making-draggable-components-in-react)*。*

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**