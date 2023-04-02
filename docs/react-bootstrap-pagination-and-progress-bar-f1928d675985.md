# React 引导—分页和进度条

> 原文：<https://javascript.plainenglish.io/react-bootstrap-pagination-and-progress-bar-f1928d675985?source=collection_archive---------0----------------------->

![](img/066197266c93c2875b3647dfbe5cd428.png)

Photo by [Adam Jaime](https://unsplash.com/@arobj?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 添加分页按钮和进度条。

# 页码

我们可以用`Pagination`组件添加分页按钮。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Pagination from "react-bootstrap/Pagination";let active = 2;
let items = [];
for (let num = 1; num <= 10; num++) {
  items.push(
    <Pagination.Item key={num} active={num === active}>
      {num}
    </Pagination.Item>
  );
}export default function App() {
  return (
    <>
      <Pagination>{items}</Pagination>
    </>
  );
}
```

我们有`Pagination`组件，其中有`Pagination.Item`组件来显示分页链接。

同样，在每个项目上，我们设置`active`道具来突出显示活动的项目。

# 更多选项

我们可以添加按钮去第一页，上一页，下一页，最后一页，并显示省略号。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Pagination from "react-bootstrap/Pagination";export default function App() {
  return (
    <>
      <Pagination>
        <Pagination.First />
        <Pagination.Prev />
        <Pagination.Item>{1}</Pagination.Item>
        <Pagination.Ellipsis /> <Pagination.Item disabled>{10}</Pagination.Item>
        <Pagination.Item>{11}</Pagination.Item>
        <Pagination.Item active>{12}</Pagination.Item>
        <Pagination.Item>{13}</Pagination.Item> <Pagination.Ellipsis />
        <Pagination.Item>{20}</Pagination.Item>
        <Pagination.Next />
        <Pagination.Last />
      </Pagination>
    </>
  );
}
```

`Pagination.First`组件让我们添加一个按钮来进入第一页。

`Pagination.Prev`让我们添加一个按钮，转到上一页。

`Pagination.Ellipsis`让我们在`Pagination`组件中显示省略号。

`Pagination.Next`让我们添加一个按钮进入下一页。

并且`Pagination.Last`让我们添加一个按钮去最后一页。

# 进度条

进度条让我们向用户显示进度反馈。

我们可以用`ProgressBar`组件添加一个。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ProgressBar from "react-bootstrap/ProgressBar";export default function App() {
  return (
    <>
      <ProgressBar now={80} />
    </>
  );
}
```

我们只是在`ProgressBar`后面加上`now`来表示进度。

我们可以用`label`道具添加一个标签:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ProgressBar from "react-bootstrap/ProgressBar";const now = 80;
export default function App() {
  return (
    <>
      <ProgressBar now={now} label={`${now}%`} />
    </>
  );
}
```

`srOnly`道具让我们使标签只对屏幕阅读器可用，所以它不会显示:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ProgressBar from "react-bootstrap/ProgressBar";const now = 80;
export default function App() {
  return (
    <>
      <ProgressBar now={now} label={`${now}%`} srOnly />
    </>
  );
}
```

# 风格选择

我们可以添加`variant`道具来改变样式:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ProgressBar from "react-bootstrap/ProgressBar";export default function App() {
  return (
    <>
      <ProgressBar variant="success" now={60} />
      <ProgressBar variant="info" now={23} />
      <ProgressBar variant="warning" now={30} />
      <ProgressBar variant="danger" now={86} />
    </>
  );
}
```

我们添加了带有不同名称的`variant`来给它们添加不同的样式。

现在，条形应该有不同的颜色。

# 有斑纹的

我们可以用`striped`道具在横条上添加条纹:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ProgressBar from "react-bootstrap/ProgressBar";export default function App() {
  return (
    <>
      <ProgressBar striped variant="success" now={60} />
      <ProgressBar striped variant="info" now={23} />
      <ProgressBar striped variant="warning" now={30} />
      <ProgressBar striped variant="danger" now={86} />
    </>
  );
}
```

# 动画条纹

为了让它们变成条纹动画，我们可以添加`animated`道具:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ProgressBar from "react-bootstrap/ProgressBar";export default function App() {
  return (
    <>
      <ProgressBar animated variant="success" now={60} />
    </>
  );
}
```

# 叠放在一起的

可以堆叠多个进度条。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ProgressBar from "react-bootstrap/ProgressBar";export default function App() {
  return (
    <>
      <ProgressBar>
        <ProgressBar striped variant="success" now={25} key={1} />
        <ProgressBar variant="warning" now={20} key={2} />
        <ProgressBar striped variant="danger" now={10} key={3} />
      </ProgressBar>
    </>
  );
}
```

我们添加了`key`以便 React 可以识别它们。

我们添加了`variant`来改变它们的样式。

`now`有进步。

它们一个接一个地展示。

![](img/723a5eddcea2e453cadbeace65b5a45c.png)

Photo by [Whitney Wright](https://unsplash.com/@whitney_wright?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`Pagination`组件添加分页。

它有许多分页按钮和跳转到各种页面的按钮。

React Bootstrap 还附带了一个进度条组件，可以有各种样式和标签。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**