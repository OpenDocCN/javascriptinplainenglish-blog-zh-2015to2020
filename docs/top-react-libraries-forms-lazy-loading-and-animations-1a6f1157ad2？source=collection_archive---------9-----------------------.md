# 顶级 React 库—表单、延迟加载和动画

> 原文：<https://javascript.plainenglish.io/top-react-libraries-forms-lazy-loading-and-animations-1a6f1157ad2?source=collection_archive---------9----------------------->

![](img/6f50d07f1794e238bf3cb31408ab788b.png)

Photo by [Pero Kalimero](https://unsplash.com/@pericakalimerica?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# rc 形式

我们可以使用 rc-form 包来帮助我们处理表单输入。

要安装它，我们运行:

```
npm i rc-form
```

然后我们可以通过写来使用它:

```
import React from "react";
import { createForm, formShape } from "rc-form";class App extends React.Component {
  static propTypes = {
    form: formShape
  }; submit = () => {
    this.props.form.validateFields((error, value) => {
      console.log(error, value);
    });
  }; render() {
    let errors;
    const { getFieldProps, getFieldError } = this.props.form;
    return (
      <div>
        <input {...getFieldProps("normal")} placeholder="first name" />
        <input
          placeholder="last name"
          {...getFieldProps("required", {
            onChange() {},
            rules: [{ required: true }]
          })}
        />
        {(errors = getFieldError("required")) ? errors.join(",") : null}
        <button onClick={this.submit}>submit</button>
      </div>
    );
  }
}export default createForm()(App);
```

我们有两个输入字段，其中传递了从 rc-form 包中生成的属性。

此外，我们为具有`FormShape`结构的`forms`道具指定了道具类型。

规则通过`getFieldProps`函数传递到输入中。

`required`属性添加了一个对象来指定实际的规则。

错误来自于`getFieldError`函数。

当我们点击提交按钮时，调用`submit`方法。

当我们将组件传递给高阶函数`createForm`时，我们得到了`form` prop。

# 反应-lazyload

react-lazyload 包允许我们向应用程序中添加延迟加载内容。

这意味着它们只在屏幕上显示。

要安装它，我们可以运行:

```
npm i react-lazyload
```

然后我们可以通过写来使用它:

```
import React from "react";
import LazyLoad from "react-lazyload";export default function App() {
  return (
    <div className="list">
      <LazyLoad height={200}>
        <img src="http://placekitten.com/200/200" alt="cat" />
      </LazyLoad>
      <LazyLoad height={200} once>
        <img src="http://placekitten.com/200/200" alt="cat" />
      </LazyLoad>
      <LazyLoad height={200} offset={100}>
        <img src="http://placekitten.com/200/200" alt="cat" />
      </LazyLoad>
      <LazyLoad>
        <img src="http://placekitten.com/200/200" alt="cat" />
      </LazyLoad>
    </div>
  );
}
```

我们只需使用`LazyLoad`组件，将我们想要的任何东西放入其中。

内容的高度用`height`道具设定。

`once`意味着一旦它被加载，就不会被`LazyLoad`组件管理。

`offset`表示当组件的上边缘距离视口 100 像素时，组件将被加载。

# 反应-平滑

react-smooth 是一个动画库，我们可以在 react 应用程序中使用。

要安装它，我们运行:

```
npm i react-smooth
```

然后我们可以通过编写以下代码来使用`Animate`组件:

```
import React from "react";
import Animate from "react-smooth";export default function App() {
  return (
    <div className="app">
      <Animate to="0" from="1" attributeName="opacity">
        <div>hello</div>
      </Animate>
    </div>
  );
}
```

我们指定`to`和`from`道具来指定动画。

`attributeName`具有随动画变化的 CSS 属性。

此外，我们可以单独指定每个步骤。

例如，我们可以写:

```
import React from "react";
import Animate from "react-smooth";const steps = [
  {
    style: {
      opacity: 0
    },
    duration: 400
  },
  {
    style: {
      opacity: 1,
      transform: "translate(0, 0)"
    },
    duration: 2000
  },
  {
    style: {
      transform: "translate(300px, 300px)"
    },
    duration: 1200
  }
];export default function App() {
  return (
    <div className="app">
      <Animate steps={steps}>
        <div>hello</div>
      </Animate>
    </div>
  );
}
```

在`steps`数组中指定动画的步骤。

我们在`style`属性中指定要更改的 CSS 属性和要更改的值。

`duration`是每一步的长度。

我们将数组传递到`steps`道具中，按照给定的步骤制作动画。

现在我们得到一些不透明度的变化和这些数组条目的转换。

此外，我们可以指定 CSS 属性来改变和缓和。

除此之外，标记之间的代码可以更改为具有我们可以使用的样式值的函数:

```
import React from "react";
import Animate from "react-smooth";export default function App() {
  return (
    <div className="app">
      <Animate from={{ opacity: 0 }} to={{ opacity: 1 }} easing="ease-in">
        {({ opacity }) => <div style={{ opacity }}>hello</div>}
      </Animate>
    </div>
  );
}
```

`from`和`to`有动画开头或结尾的样式。

`easing`具有与动画配合使用的缓动功能。

然后我们在函数的参数中获取样式，并在`style`属性中使用它。

![](img/7ab3db3feb1e40d899d5b3906ea1c181.png)

Photo by [Alex Machado](https://unsplash.com/@alexmachado?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

rc-form 让我们用更少的代码构建带有数据处理的表单。

react-lazyload 允许我们在应用程序中进行延迟加载。

react-smooth 允许我们在应用程序中创建动画。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**