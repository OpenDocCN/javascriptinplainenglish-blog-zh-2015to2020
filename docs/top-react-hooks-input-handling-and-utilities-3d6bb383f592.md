# Top React 挂钩—输入处理和实用程序

> 原文：<https://javascript.plainenglish.io/top-react-hooks-input-handling-and-utilities-3d6bb383f592?source=collection_archive---------12----------------------->

![](img/cbb3bf6adbe4556638953dadea89cf42.png)

Photo by [Clark Street Mercantile](https://unsplash.com/@mercantile?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 坩埚

熔炉有很多钩子，我们可以在 React 应用程序中使用。

这是一个有许多挂钩的实用程序库。

要安装它，我们运行:

```
npm install @withvoid/melting-pot --save
```

或者:

```
yarn add @withvoid/melting-pot
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useActive } from "@withvoid/melting-pot";export default function App() {
  const { active, bind } = useActive(); const styles = {
    emphasis: {
      backgroundColor: active ? "yellow" : "red",
      color: active ? "black" : "white",
      padding: 5,
      width: 55,
      textAlign: "center"
    }
  }; return (
    <div {...bind}>
      <span style={styles.emphasis}>{active ? "red" : "white"}</span>
      <p>{active.toString()}</p>
    </div>
  );
}
```

我们将`bind`对象扩展到外部 div，让我们观察内部的项目是否是活动的。

现在，当我们单击它时，就会显示出`emphasis`属性中的样式。

当我们点击外部 div 时,`active`状态会改变。

当我们单击外部 div 时，它是活动的。

它还提供了`useDidMount`钩子来代替 React 中的`componentDidMount`方法。

我们可以通过书写来使用它:

```
import React from "react";
import { useDidMount } from "@withvoid/melting-pot";export default function App() {
  useDidMount(() => {
    console.log("hello world");
  }); return <div />;
}
```

我们只是把我们想运行的东西放到`useDidMount`回调函数中来运行它。

`useFormField`钩子让我们可以轻松处理表单值。

例如，我们可以写:

```
import React from "react";
import { useFormField } from "@withvoid/melting-pot";export default function App() {
  const form = {
    name: useFormField(),
    age: useFormField()
  }; const onSubmit = event => {
    event.preventDefault();
    if (!onValidate()) {
      return;
    }
    alert("Success");
    onReset();
  }; const onReset = () => Object.values(form).map(({ reset }) => reset()); const onValidate = () => {
    let isValid = true;
    Object.values(form).map(({ isEmpty, validate }) => {
      validate();
      if (isEmpty) {
        isValid = false;
      }
    });
    return isValid;
  }; return (
    <div>
      <form onSubmit={onSubmit}>
        <div>
          <label htmlFor="name">Name</label>
          <input id="name" {...form.name.bind} placeholder="name" />
          {form.name.isValid && <p>Name is required*</p>}
        </div>
        <div>
          <label htmlFor="age">Age</label>
          <input id="age" {...form.age.bind} type="number" placeholder="age" />
          {form.age.isValid && <p>Age is required*</p>}
        </div>
        <div>
          <button type="submit">Submit</button>
          <button type="button" onClick={onReset}>
            Reset
          </button>
        </div>
      </form>
    </div>
  );
}
```

创建带有`name`和`age`字段的表单。

我们将`form.name.bind`和`form.age.bind`属性中的所有内容传递给输入，让我们处理表单值。

然后我们可以使用每个字段的`isValid`属性来检查有效性。

`onReset`函数获取所有的表单字段，并对它们调用`reset`。

每个表单字段中还有一个`validate`方法来验证它们。

`isEmpty`检查是否为空。

这些都是由`useFormField`挂钩提供的。

![](img/d506feff2a00f5f095cfb75a1324f5ae.png)

Photo by [Anders Wideskott](https://unsplash.com/@wideshot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

熔炉库让我们可以做很多本来要自己写的事情。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**