# 反应引导—覆盖

> 原文：<https://javascript.plainenglish.io/react-bootstrap-overlays-5cb54d402a1c?source=collection_archive---------0----------------------->

![](img/b64a0a9b31094b6e94bb475a59fe6efa.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 添加覆盖。

# 覆盖物

覆盖图是工具提示，当我们点击或悬停在一个元素上时可以显示。

为了添加一个简单的组件，我们使用了`Overlay`组件:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Button from "react-bootstrap/Button";
import Overlay from "react-bootstrap/Overlay";export default function App() {
  const [show, setShow] = React.useState(false);
  const target = React.useRef(null);return (
    <>
      <Button variant="primary" ref={target} onClick={() => setShow(!show)}>
        Click me
      </Button>
      <Overlay target={target.current} show={show} placement="right">
        {({
          placement,
          scheduleUpdate,
          arrowProps,
          outOfBoundaries,
          show: _show,
          ...props
        }) => (
          <div
            {...props}
            style={{
              backgroundColor: "orange",
              padding: "2px 10px",
              color: "white",
              borderRadius: 3,
              ...props.style
            }}
          >
            tooltip
          </div>
        )}
      </Overlay>
    </>
  );
}
```

我们用按钮上的`useRef`创建一个引用，这样它就可以在`Overlay`组件中被引用。

这样，当我们点击按钮时就会显示出来。

然后我们添加`Overlay`组件，它将带有各种属性的对象的函数作为参数作为子对象。

`placement`设置摆放位置。

`scheduleUpdate`让覆盖图自己重新定位。

`arrowProps`让我们改变工具提示箭头的位置。

该函数返回一个元素，其中包含一些样式和要显示的内容。

`target` prop 是触发元素显示的元素。

在我们的例子中，它是按钮。

`placement`设置为`right`在右侧显示。

# 重叠触发器

我们可以使用`OverlayRrigger`组件在一个地方创建一个带有触发器的覆盖图。

这样，我们不需要为触发器创建一个 ref

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Button from "react-bootstrap/Button";
import OverlayTrigger from "react-bootstrap/OverlayTrigger";
import Tooltip from "react-bootstrap/Tooltip";function renderTooltip(props) {
  return <Tooltip {...props}>tooltip</Tooltip>;
}export default function App() {
  return (
    <>
      <OverlayTrigger
        placement="right"
        delay={{ show: 250, hide: 400 }}
        overlay={renderTooltip}
      >
        <Button variant="success">Hover me</Button>
      </OverlayTrigger>
    </>
  );
}
```

简化我们的工具提示代码。

我们使用`OverlayTrigger`组件，将`placement`设置为`right`。

`delay`是一个对象，用来设置我们显示或隐藏工具提示时的延迟时间，以毫秒为单位。

`overlay`具有工具提示的组件。

它需要我们传递给`Tooltip`的所有道具。

我们可以用`placement`道具设置工具提示的位置:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Button from "react-bootstrap/Button";
import OverlayTrigger from "react-bootstrap/OverlayTrigger";
import Tooltip from "react-bootstrap/Tooltip";export default function App() {
  return (
    <>
      {["top", "right", "bottom", "left"].map(placement => (
        <OverlayTrigger
          key={placement}
          placement={placement}
          overlay={<Tooltip>Tooltip {placement}.</Tooltip>}
        >
          <Button variant="secondary">Tooltip {placement}</Button>
        </OverlayTrigger>
      ))}
    </>
  );
}
```

它可以是顶部、右侧、底部或左侧。

# 松饼

我们可以有弹出框，这是包含更多内容的工具提示。

它们看起来像在 iOS 中找到的打开。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Button from "react-bootstrap/Button";
import OverlayTrigger from "react-bootstrap/OverlayTrigger";
import Popover from "react-bootstrap/Popover";const popover = (
  <Popover>
    <Popover.Title as="h1">Popover right</Popover.Title>
    <Popover.Content>
      Lorem ipsum dolor sit amet, consectetur adipiscing <b>lit.</b>
    </Popover.Content>
  </Popover>
);export default function App() {
  return (
    <>
      <OverlayTrigger trigger="click" placement="right" overlay={popover}>
        <Button variant="success">Click me</Button>
      </OverlayTrigger>
    </>
  );
}
```

在按钮的右边添加一个弹出窗口。

当我们点击它时，它就会显示出来。

我们使用内置的`Popover`组件在我们触发它时显示它。

我们将它传递到`overlay` prop 中，这样当我们点击按钮时，它就会显示出来，如`trigger`所示。

像工具提示一样，弹出框也可以放在不同的位置:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Button from "react-bootstrap/Button";
import OverlayTrigger from "react-bootstrap/OverlayTrigger";
import Popover from "react-bootstrap/Popover";export default function App() {
  return (
    <>
      {["top", "right", "bottom", "left"].map(placement => (
        <OverlayTrigger
          trigger="click"
          key={placement}
          placement={placement}
          overlay={
            <Popover id={`popover-positioned-${placement}`}>
              <Popover.Title as="h1">{`Popover ${placement}`}</Popover.Title>
              <Popover.Content>
                Lorem ipsum dolor sit amet, consectetur adipiscing <b>lit.</b>
              </Popover.Content>
            </Popover>
          }
        >
          <Button variant="secondary">Popover {placement}</Button>
        </OverlayTrigger>
      ))}
    </>
  );
}
```

我们将`placement`道具设置为`top`、`right`、`bottom`或`left`，以在这些位置显示它们。

![](img/b0030a4c99d38505cf131381bbb44610.png)

Photo by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`Overlay`或`OverlayTrigger`组件添加工具提示和弹出窗口。

`OverlayTrigger`更短，所以我们可以写更少的代码。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**