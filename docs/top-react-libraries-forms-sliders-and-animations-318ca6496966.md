# 顶级 React 库—表单、滑块和动画

> 原文：<https://javascript.plainenglish.io/top-react-libraries-forms-sliders-and-animations-318ca6496966?source=collection_archive---------8----------------------->

![](img/cb716abdcd313b0dee72b82ed688437a.png)

Photo by [Alessandro Alimonti](https://unsplash.com/@alessandro_alimonti?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 福米克

为了使表单处理更加容易，我们可以使用表单库来处理表单。

Formik 是我们可以使用的一个库。

要安装它，我们运行:

```
npm i formik
```

然后我们可以通过写来使用它:

```
import React from "react";
import { Formik } from "formik";export default function App() {
  return (
    <div className="App">
      <Formik
        initialValues={{ email: "", name: "" }}
        validate={values => {
          const errors = {};
          if (!values.email) {
            errors.email = "Required";
          } else if (
            !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
          ) {
            errors.email = "Invalid email address";
          }
          return errors;
        }}
        onSubmit={(values, { setSubmitting }) => {
          setTimeout(() => {
            alert(JSON.stringify(values, null, 2));
            setSubmitting(false);
          }, 900);
        }}
      >
        {({
          values,
          errors,
          touched,
          handleChange,
          handleBlur,
          handleSubmit,
          isSubmitting
        }) => (
          <form onSubmit={handleSubmit}>
            <input
              type="email"
              name="email"
              placeholder="email"
              onChange={handleChange}
              onBlur={handleBlur}
              value={values.email}
            />
            {errors.email && touched.email && errors.email}
            <input
              type="name"
              name="name"
              placeholder="name"
              onChange={handleChange}
              onBlur={handleBlur}
              value={values.name}
            />
            {errors.name && touched.name && errors.name}
            <button type="submit" disabled={isSubmitting}>
              Submit
            </button>
          </form>
        )}
      </Formik>
    </div>
  );
}
```

我们使用包中附带的`Formik`组件。

`initialValues`属性有我们字段的初始值。

`validate`道具有我们的验证功能。

我们可以获取所有的`values`，检查它们，并返回对象中的任何错误(如果有的话)。

`onSubmit` prop 接受一个具有输入值的提交处理程序。

然后我们可以对他们做些什么来提交它们。

它只在所有值都有效时运行。

在标签之间，我们有一个函数，它的参数是一个具有许多属性的对象。

`values`拥有价值观。

`errors`有表单验证错误。

`touched`表示哪个区域被触摸。

`handleChange`具有输入字段更改处理程序功能。

`handleBlur`有处理焦点之外的表单字段的处理器。

`isSubmitting`有布尔值表示是否提交。

我们将它们传递到表单的各个部分。

`handleChange`、`handleBlur`和`values`被传递到表单域。

`errors`都是外地的。

`isSubmitting`被传入按钮，如果正在提交则禁用。

# rc 滑块

我们可以使用 rc-slider 包向 React 应用程序添加一个滑块组件。

要安装它，我们可以运行:

```
npm i rc-slider
```

然后我们可以使用`Slider`和`Range`组件将滑块输入添加到我们的应用程序中:

```
import React from "react";
import Slider, { Range } from "rc-slider";
import "rc-slider/assets/index.css";export default function App() {
  return (
    <div className="App">
      <Slider />
      <Range />
    </div>
  );
}
```

我们可以通过添加`value`、`onChange`和`onAfterChange`道具使其成为受控组件:

```
import React from "react";
import Slider from "rc-slider";
import "rc-slider/assets/index.css";export default function App() {
  const [value, setValue] = React.useState(0); const onSliderChange = value => {
    setValue(value);
  }; const onAfterChange = value => {
    console.log(value);
  }; return (
    <div className="App">
      <Slider
        value={value}
        onChange={onSliderChange}
        onAfterChange={onAfterChange}
      />
    </div>
  );
}
```

`onChange`将更新所选值作为`value`状态的值。

`value`具有被选择的值。

我们可以定制步骤、标记、手柄样式等等。

# RC-动画

我们可以使用 rc-animate 来制作 React 组件的动画。

要安装它，我们运行:

```
npm i rc-animate
```

然后我们可以写:

```
import React from "react";
import Animate from "rc-animate";const Div = props => {
  const { style, show, ...restProps } = props;
  const newStyle = { ...style, display: show ? "" : "none" };
  return <div {...restProps} style={newStyle} />;
};export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      enter: true
    };
  } toggle(field) {
    this.setState({
      [field]: !this.state[field]
    });
  } render() {
    const style = {
      width: "200px",
      height: "200px",
      backgroundColor: "red"
    };
    return (
      <div>
        <label>
          <input
            type="checkbox"
            onChange={this.toggle.bind(this, "enter")}
            checked={this.state.enter}
          />
          show
        </label>
        <br />
        <br />
        <Animate component="" showProp="show" transitionName="fade">
          <Div show={this.state.enter} style={style} />
        </Animate>
      </div>
    );
  }
}
```

去使用它。

我们创建`Div`组件来获得其他道具的样式。

然后我们使用`Animate`组件添加`transitionName`道具来添加渐变动画。

我们还有一个复选框来切换 div。

![](img/0782ac95588b9eccef3929f5eb9667a5.png)

Photo by [Shreyak Singh](https://unsplash.com/@shreyaksingh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Formik 是一个有用的表单处理组件。

rc-slider 帮助我们轻松添加滑块。

rc-animate 允许我们向 React 应用程序添加动画。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)