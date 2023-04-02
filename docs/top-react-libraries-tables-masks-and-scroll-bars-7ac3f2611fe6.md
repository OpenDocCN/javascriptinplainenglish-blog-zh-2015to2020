# Top React 库—表格、遮罩和滚动条

> 原文：<https://javascript.plainenglish.io/top-react-libraries-tables-masks-and-scroll-bars-7ac3f2611fe6?source=collection_archive---------8----------------------->

![](img/61fb240558374d5a77c7a58a90815afa.png)

Photo by [Hermes Rivera](https://unsplash.com/@hermez777?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# rc 表

rc-table 是一个用于在 React 应用程序中添加简单表格的库。

要使用它，我们可以运行:

```
npm i rc-table
```

来安装它。

然后我们可以通过写来使用它:

```
import React from "react";
import Table from "rc-table";const columns = [
  {
    title: "Name",
    dataIndex: "name",
    key: "name",
    width: 100
  },
  {
    title: "Age",
    dataIndex: "age",
    key: "age",
    width: 100
  },
  {
    title: "Address",
    dataIndex: "address",
    key: "address",
    width: 200
  },
  {
    title: "",
    dataIndex: "",
    key: "operations",
    render: () => <a href="#">Delete</a>
  }
];const data = [
  { name: "james", age: 18, address: "address", key: "1" },
  { name: "mary", age: 26, address: "address", key: "2" }
];export default function App() {
  return (
    <div className="App">
      <Table columns={columns} data={data} />
    </div>
  );
}
```

我们在`columns`数组中指定列。

`title`是显示的文本。

`dataIndex`是要在列中显示的属性名。

`key`是列的唯一 ID。

`width`有宽度。

`render`是返回部分分量的函数。

我们还有一个包含我们想要显示的对象的`data`数组。

在`App`中，我们将它们全部传递给`Table`组件，作为呈现表格的道具。

它为我们提供了许多其他选项，如更改表格布局、样式、使表格可扩展等等。

# 简单条-反应

让我们在应用程序中添加一个样式化的滚动条。

要安装它，我们运行:

```
npm i simplebar-react
```

然后我们可以通过写来使用它:

```
import React from "react";
import SimpleBar from "simplebar-react";
import "simplebar/dist/simplebar.min.css";export default function App() {
  return (
    <div className="App">
      <SimpleBar style={{ maxHeight: 300 }}>
        {Array(1000)
          .fill()
          .map((_, i) => (
            <p>{i + 1}</p>
          ))}
      </SimpleBar>
    </div>
  );
}
```

我们只是将`SimpleBar`组件与捆绑的 CSS 一起使用。

用`maxHeight`将`style`道具设置为一个对象。

然后我们在`SimpleBar`组件中显示一个 p 元素的数组。

然后，我们将看到显示的包提供的滚动条。

还有其他选项，如强制滚动条可见和自动隐藏滚动条。

我们还可以用 refs 访问 SimpleBar 实例。

# 反应输入掩码

React Input Mask 包是一个组件，可以让我们将输入掩码添加到应用程序中。

要安装它，我们运行:

```
npm i react-text-mask
```

然后我们可以通过写来使用它:

```
import React from "react";
import MaskedInput from "react-text-mask";export default function App() {
  return (
    <div className="App">
      <MaskedInput
        mask={[
          "(",
          /[1-9]/,
          /\d/,
          /\d/,
          ")",
          " ",
          /\d/,
          /\d/,
          /\d/,
          "-",
          /\d/,
          /\d/,
          /\d/,
          /\d/
        ]}
      />
    </div>
  );
}
```

我们将`MaskedInput`组件与`mask`支柱一起使用。

它有一个数组，包含我们想要限制输入值的模式的各个部分。

可以用定制组件对其进行定制。

此外，我们可以添加占位符、类名以及模糊和更改处理程序:

```
import React from "react";
import MaskedInput from "react-text-mask";export default function App() {
  return (
    <div className="App">
      <MaskedInput
        mask={[
          "(",
          /[1-9]/,
          /\d/,
          /\d/,
          ")",
          " ",
          /\d/,
          /\d/,
          /\d/,
          "-",
          /\d/,
          /\d/,
          /\d/,
          /\d/
        ]}
        className="form-control"
        placeholder="Enter a phone number"
        guide={false}
        id="phone-input"
        onBlur={() => {}}
        onChange={() => {}}
      />
    </div>
  );
}
```

我们添加类名和占位符来为它们添加样式和占位符。

此外，我们有`onBlur`和`onChange`道具来将这些处理程序添加到我们的应用程序中。

`guide`是一个布尔值，表示我们是否想要显示带有提示的字符。

![](img/11a085545440447c34d472c15d1ef622.png)

Photo by [Matthew Foulds](https://unsplash.com/@fouldsmatt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

rc-table 让我们可以轻松地添加一个简单的表格。

react 是 React 应用程序的滚动条组件。

React 输入掩码允许我们在应用程序中添加输入掩码。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**