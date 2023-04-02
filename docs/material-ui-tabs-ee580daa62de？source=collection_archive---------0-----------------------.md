# 材料用户界面—选项卡

> 原文：<https://javascript.plainenglish.io/material-ui-tabs-ee580daa62de?source=collection_archive---------0----------------------->

![](img/8e87c86dfa8c152e646a639b46f3959c.png)

Photo by [Zuriela Benitez](https://unsplash.com/@zury14benitez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加标签与材料用户界面。

# 制表符

我们可以用`Tabs`组件添加选项卡。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return (
    <div {...other}>
      {value === index && <Box p={3}>{children}</Box>}
    </div>
  );
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs value={value} onChange={handleChange}>
          <Tab label="Item One" />
          <Tab label="Item Two" />
          <Tab label="Item Three" />
        </Tabs>
      </AppBar>
      <TabPanel value={value} index={0}>
        Item One
      </TabPanel>
      <TabPanel value={value} index={1}>
        Item Two
      </TabPanel>
      <TabPanel value={value} index={2}>
        Item Three
      </TabPanel>
    </>
  );
}
```

我们创建自己的`TabPanel`来显示属于给定选项卡的内容。

我们将该值传递给`value`属性，并将其与`index`属性进行比较，以检查要显示的内容。

在`TabPanel`组件中，我们比较`value`和`index`看它们是否相同。

如果是，我们就显示内容。

否则，我们把它藏起来。

`label`有标签的文字。

我们将用标签的索引`newValue`来更新`value`。

# 包装标签

如果我们有很长的标签，那么它可能会因为太长而被剪掉。

为了让它缠绕，我们可以添加`wrapped`道具。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs value={value} onChange={handleChange}>
          <Tab
            label="Lorem ipsum dolor sit amet, consectetur adipiscing elit."
            wrapped
          />
          <Tab label="Item Two" />
          <Tab label="Item Three" />
        </Tabs>
      </AppBar>
      <TabPanel value={value} index={0}>
        Item One
      </TabPanel>
      <TabPanel value={value} index={1}>
        Item Two
      </TabPanel>
      <TabPanel value={value} index={2}>
        Item Three
      </TabPanel>
    </>
  );
}
```

第一个`Tab`具有长`label`值。

由于我们添加了`wrapped`道具，它将被包裹。

# 禁用选项卡

要禁用选项卡，我们可以添加`disabled`属性。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs value={value} onChange={handleChange}>
          <Tab label="Item One" />
          <Tab label="Item Two" />
          <Tab label="Item Three" disabled />
        </Tabs>
      </AppBar>
      <TabPanel value={value} index={0}>
        Item One
      </TabPanel>
      <TabPanel value={value} index={1}>
        Item Two
      </TabPanel>
      <TabPanel value={value} index={2}>
        Item Three
      </TabPanel>
    </>
  );
}
```

然后我们可以禁用第三个选项卡。

# 全角标签

我们可以在`variant`道具设置为`fullWidth`的情况下制作全宽标签。

为此，我们写道:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs value={value} onChange={handleChange} variant="fullWidth">
          <Tab label="Item One" />
          <Tab label="Item Two" />
        </Tabs>
      </AppBar>
      <TabPanel value={value} index={0}>
        Item One
      </TabPanel>
      <TabPanel value={value} index={1}>
        Item Two
      </TabPanel>
    </>
  );
}
```

使所有选项卡均匀填充空间。

它们一起占据了屏幕的整个宽度。

![](img/f93148846c0bd81e4add5abaf7af46b3.png)

Photo by [Mateus Campos Felipe](https://unsplash.com/@matfelipe?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`Tabs`组件添加选项卡。

内容可以添加到任何元素中。

我们可以根据单击选项卡时设置的值来检查内容的索引，从而确定要显示的内容。

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[T21【plain English . io找到一切的链接！](https://plainenglish.io/)