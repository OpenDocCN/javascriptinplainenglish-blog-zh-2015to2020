# 材料用户界面—日期和时间选择器和单选按钮

> 原文：<https://javascript.plainenglish.io/material-ui-date-and-time-pickers-and-radio-buttons-e383f524193a?source=collection_archive---------4----------------------->

![](img/42cfd91a4f201deef3efb974f6ec3ce0.png)

Photo by [Hermes Rivera](https://unsplash.com/@hermez777?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何用材质 UI 添加日期和时间选择器以及单选按钮。

# 日期/时间选择器

我们可以添加一个本机日期选择器，将`type`设置为`date`。

例如，我们可以写:

```
import React from "react";
import TextField from "@material-ui/core/TextField";export default function App() {
  return (
    <div>
      <form noValidate>
        <TextField
          id="date"
          label="Birthday"
          type="date"
          defaultValue="2020-05-26"
          InputLabelProps={{
            shrink: true
          }}
        />
      </form>
    </div>
  );
}
```

添加日期选取器。

我们使用了`TextField`组件来添加文本字段。

`defaultValue`用于设置拾取器的默认值。

# 日期和时间选择器

如果我们想为日期和时间添加一个选择器，我们可以设置`type`

例如，我们可以写:

```
import React from "react";
import TextField from "@material-ui/core/TextField";export default function App() {
  return (
    <div>
      <form noValidate>
        <TextField
          id="date"
          label="Birthday"
          type="datetime-local"
          defaultValue="2021-05-26T10:30"
          InputLabelProps={{
            shrink: true
          }}
        />
      </form>
    </div>
  );
}
```

我们将`type`设置为`datetime-local`来显示本地日期和时间选择器。

# 时间挑选者

要仅添加时间选择器，我们可以将`TextField`的`type`设置为`time`。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  return (
    <div>
      <form noValidate>
        <TextField
          id="time"
          label="Clock"
          type="time"
          defaultValue="07:30"
          InputLabelProps={{
            shrink: true
          }}
        />
      </form>
    </div>
  );
}
```

显示本机时间选取器。

# 单选按钮

单选按钮允许用户从一组选项中选择一个。

我们可以在一个`RadioGroup`中多放一个单选按钮。

例如，我们可以写:

```
import React from "react";
import RadioGroup from "@material-ui/core/RadioGroup";
import FormControlLabel from "@material-ui/core/FormControlLabel";
import Radio from "@material-ui/core/Radio";export default function App() {
  const [value, setValue] = React.useState("apple"); const handleChange = event => {
    setValue(event.target.value);
  }; return (
    <div>
      <form noValidate>
        <RadioGroup name="fruit" value={value} onChange={handleChange}>
          <FormControlLabel value="apple" control={<Radio />} label="apple" />
          <FormControlLabel value="orange" control={<Radio />} label="orange" />
          <FormControlLabel value="banana" control={<Radio />} label="banana" />
          <FormControlLabel
            value="grape"
            disabled
            control={<Radio />}
            label="grape"
          />
        </RadioGroup>
      </form>
    </div>
  );
}
```

我们添加了`RadioGroup`组件来添加一组单选按钮。

然后我们添加`FormControlLabel`来为单选按钮添加标签。

`value`是单选按钮的值。

我们将单选按钮放在每个`FormControlLabel`中。

`label`设置每个单选按钮的标签。

我们可以用`disabled`属性添加一个禁用单选按钮。

为了将用户选择的值设置为状态值，我们将`handleChange`函数传递给`onChange`属性。

然后当我们选择一个选项时，我们可以将`value`属性的值设置为`value`状态的值。

# 独立单选按钮

`Radio`组件可以作为独立组件使用。

例如，我们可以写:

```
import React from "react";
import Radio from "[@material](http://twitter.com/material)-ui/core/Radio";export default function App() {
  const [value, setValue] = React.useState("apple"); const handleChange = event => {
    setValue(event.target.value);
  }; return (
    <div>
      <form noValidate>
        <Radio
          checked={value === "apple"}
          onChange={handleChange}
          value="apple"
          name="fruit"
        />
        <Radio
          checked={value === "orange"}
          onChange={handleChange}
          value="orange"
          name="fruit"
        />
      </form>
    </div>
  );
}
```

我们有一个`Radio`组件来显示没有标签的单选按钮。

通过比较选择的`value`和单选按钮的值来设置`checked`状态。

`onChange`与上例相同。

# 标签放置

标签的位置可以用`labelPlacement`支柱改变。

例如，我们可以写:

```
import React from "react";
import Radio from "@material-ui/core/Radio";
import FormControlLabel from "@material-ui/core/FormControlLabel";export default function App() {
  return (
    <div>
      <form noValidate>
        <FormControlLabel
          value="bottom"
          control={<Radio color="primary" />}
          label="Bottom"
          labelPlacement="bottom"
        />
      </form>
    </div>
  );
}
```

我们将`labelPlacement`属性设置为`bottom`,将标签放在单选按钮的下面。

其他可能的值包括`top`、`start`和`end`。

![](img/9bb18f4bc6b832af1a0cd261fa1f9b04.png)

Photo by [SwapnIl Dwivedi](https://unsplash.com/@momentance?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`TextField`组件添加日期和时间选择器。

要添加单选按钮，我们可以使用`FormControlLabel`和`Radio`组件。

可以添加不带标签的单选按钮。