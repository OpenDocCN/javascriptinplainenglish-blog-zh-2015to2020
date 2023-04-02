# 反应最佳实践—小组件

> 原文：<https://javascript.plainenglish.io/react-best-practices-small-components-f998a8e8ab7e?source=collection_archive---------10----------------------->

![](img/2b0a4b8bb8c5e5af5c7dbece0674f1fb.png)

Photo by [Davies Designs Studio](https://unsplash.com/@davies_designs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时应该遵循的一些最佳实践。

# 保持组件小而具体

组件应该只做一件事。

这样，我们可以更容易地测试和维护它们。

每个小组件都可以跨项目重用。

社区可以使用这些组件。

我们可以更容易地在更小的组件上实现性能优化，

更大的组件必须做更多的工作，并且更难维护。

# 复用性

我们应该创建可重用的组件，这样我们就不必一直添加新的组件。

它们可以用于任何数量的项目。

如果一个组件变大了，我们可以把它们分解成更小的组件。

例如，我们可以写:

```
const Button = ({ iconClass, onClick }) => {
  return (
    <button onClick={onClick()}>
      <i class={iconClass} />
    </button>
  );
};
```

做一个可重复使用的按钮。

它使用了`iconClass`和`onClick`道具，这样我们就可以将点击处理程序和图标类传递给它们。

如果我们有重复的代码，我们应该把它们合并成一个。

这样，我们可以只用一个图标做任何事情。

# 将 CSS 放到 JavaScript 中

我们可以将 CSS 放入 JavaScript 代码中。

如果一个项目是用 Create React App 创建的，我们可以将 CSS 文件作为模块导入。

这是因为 Webpack 添加了 CSS 加载器来让我们这样做。

所以我们可以写:

```
import "./styles.css";
```

导入样式。

我们还可以在项目中添加像 EmotionJS 这样的库。

它可以通过导入库来生成用于生产的 CSS 文件。

我们可以通过运行以下命令来安装它:

```
npm install --save emotion
```

然后我们可以写:

```
import React from "react";
import { css } from "emotion";const color = "white";const Div = props => {
  return (
    <div
      className={css`
        padding: 32px;
        background-color: yellow;
        font-size: 24px;
        border-radius: 4px;
        &:hover {
          color: ${color};
        }
      `}
    >
      Hover to change color.
    </div>
  );
};
```

添加我们的 CSS 代码。

这比仅仅在 CSS 文件中写出代码更方便，因为我们可以动态地改变样式。

我们用`color`做到了这一点。

`css`标签将样式字符串转换成一个对象。

还有其他像 styled-jsx 和 styled-components 这样的库让我们做同样的事情。

# 仅在必要时进行注释

我们应该只在必要的地方进行评论。

我们不需要用重复代码中内容的注释来搅乱我们的代码。

此外，我们不想在代码和注释之间产生差异。

评论很容易过时。

# 以功能命名组件

以所具有的功能来命名组件使得以后更容易找到它们。

我们可以更容易地找到组件在哪里，并知道它做什么。

我们可以将一个组件命名为`AuthorAvatar`，这样我们就知道头像是为作者准备的。

为功能命名一个函数也使它更容易被发现。

![](img/6667bed4618774970060add73ef7f9ad.png)

DPhoto by [Brian Holdsworth](https://unsplash.com/@holdsworthdesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该使功能变得小而可重用。我们应该根据它们的功能来命名，这样我们就能找到它们。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**