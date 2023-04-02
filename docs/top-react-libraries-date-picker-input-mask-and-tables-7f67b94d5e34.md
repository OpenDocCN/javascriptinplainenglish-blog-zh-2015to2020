# Top React 库—日期选择器、输入掩码和表格

> 原文：<https://javascript.plainenglish.io/top-react-libraries-date-picker-input-mask-and-tables-7f67b94d5e34?source=collection_archive---------12----------------------->

![](img/1f4fd1124cafbd44a2d430c72ecd247a.png)

Photo by [Ani Kolleshi](https://unsplash.com/@anikolleshi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反应日期时间

react-datetime 包允许我们在应用程序中添加日期或时间选择器。

要安装它，我们运行:

```
npm i react-datetime moment
```

然后我们可以通过写来使用它:

```
import React from "react";
const Datetime = require("react-datetime");export default function App() {
  return (
    <div>
      <Datetime />
    </div>
  );
}
```

这将显示一个日期选择器，我们可以从中选择日期。

我们还可以通过编写以下内容来自定义日期选择器的外观:

```
import React from "react";
const Datetime = require("react-datetime");const renderDay = (props, currentDate, selectedDate) => {
  const date = currentDate.date();
  return <td {...props}>{date < 10 ? "0" + date : date}</td>;
};const renderMonth = (props, month, year, selectedDate) => {
  return <td {...props}>{month}</td>;
};const renderYear = (props, year, selectedDate) => {
  return <td {...props}>{year % 100}</td>;
};export default function App() {
  return (
    <div>
      <Datetime
        renderDay={renderDay}
        renderMonth={renderMonth}
        renderYear={renderYear}
      />
    </div>
  );
}
```

我们添加了 3 个函数来以不同于默认的方式呈现日、月和年。

我们可以更改日期格式、时间格式、地区、值等等。

此外，我们可以通过编写以下内容使其成为受控组件:

```
import React from "react";
const Datetime = require("react-datetime");export default function App() {
  const [value, setValue] = React.useState(new Date()); return (
    <div>
      <Datetime value={value} onChange={val => setValue(val.toDate())} />
    </div>
  );
}
```

我们传入`value`来获取值，传入`onChange`来设置它。

`val`是一个 moment 对象，所以我们必须用`toDate`转换成一个`Date`实例。

# rc 表

rc-table 是一个包，可以让我们在 React 应用程序中轻松添加表格。

要安装它，我们运行:

```
npm i rc-table
```

然后我们写道:

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
    title: "Operations",
    dataIndex: "",
    key: "operations",
    render: () => <a href="#">Delete</a>
  }
];const data = [
  { name: "james", age: 22, address: "some where", key: "1" },
  { name: "mary", age: 33, address: "some where", key: "2" }
];export default function App() {
  return (
    <div>
      <Table columns={columns} data={data} />
    </div>
  );
}
```

去使用它。

我们在`columns`数组中定义了`columns` 。

`title`有标题。

`dataIndex`具有要为该列显示的条目的属性键。

`key`是该列的唯一键。

`width`有以像素为单位的列宽。

`render`有要渲染的组件。

`data`有数据，是一个对象数组。

在`App`中，我们将提供的`Table`组件与`columns`支柱一起使用。`data`有数据要显示。

# 反应输入屏蔽

react-input-mask 为我们提供了一个易于使用的输入掩码。

要安装它，我们运行:

```
npm i react-input-mask
```

然后我们可以通过写来使用它:

```
import React from "react";
import InputMask from "react-input-mask";export default function App() {
  return (
    <div>
      <InputMask mask="+4\9 999 999 9999" maskChar=" " />
    </div>
  );
}
```

我们使用`InputMask`组件来添加输入。

`mask`有限制输入的模式。

`maskChar`是遮盖面具未填充部分的字符。

我们还可以更改掩码中的格式化字符。

还有，我们可以用 Material UI 来使用。

我们也可以用`value`和`onChange`道具把它变成一个受控组件:

```
import React from "react";
import InputMask from "react-input-mask";export default function App() {
  const [value, setValue] = React.useState(""); return (
    <div>
      <InputMask
        value={value}
        onChange={e => setValue(e.target.value)}
        mask="+4\9 999 999 9999"
        maskChar=" "
      />
    </div>
  );
}
```

我们使用`onChange`回调将输入的值设置为`value`状态的值。

我们将`value`状态作为`value` 属性的值传递。

![](img/351bd020996e22ab6bd700a33715771d.png)

Photo by [zibik](https://unsplash.com/@zibik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

react-datetime 是一个日期和时间选择器，我们可以在 react 应用程序中使用。

rc-table 是一个表格组件。

react-input-mask 是输入掩码组件。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**