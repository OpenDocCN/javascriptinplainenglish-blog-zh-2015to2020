# 反应提示—组件和道具

> 原文：<https://javascript.plainenglish.io/react-tips-components-and-props-7becd640e095?source=collection_archive---------10----------------------->

![](img/7f62cdafe055a91e68e325f23a3a09bb.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是创建前端应用程序最流行的库之一。它还可以用来创建带有 React Native 的移动应用程序。

在本文中，我们将看看我们可能错过或忘记的 React 基础知识。

# 组件可以在我们应用的任何地方重用

创建 React 组件的全部意义在于，我们创建了一组可以在应用程序中的任何地方使用的元素和组件。

例如，我们可以定义组件并在任何地方使用它们，如下所示:

```
import React from "react";const Foo = () => <p>foo</p>;
const Bar = () => <p>bar</p>;export default function App() {
  return (
    <div className="App">
      <Foo />
      <Bar />
      <Bar />
    </div>
  );
}
```

在上面的代码中，我们创建了`Foo`和`Bar`组件，我们在`App`组件中使用了它们。

我们可以多次引用它们。尽管如果我们引用它们两次以上，我们应该使用`map`方法返回一个组件的多个实例，而不是到处重复它们。

例如，如果我们想在`App`中显示`Bar`组件 5 次，我们可以编写以下代码:

```
import React from "react";
const Bar = () => <p>bar</p>;export default function App() {
  return (
    <div className="App">
      {Array(5)
        .fill()
        .map((_, i) => (
          <Bar key={i} />
        ))}
    </div>
  );
}
```

在上面的代码中，我们创建了一个数组，用带有`fill`的`undefined`填充它们，然后将所有条目映射到`Bar`，这样我们就可以显示`Bar` 5 次。

此外，我们必须记住设置`key`属性，以便 React 可以正确地识别它们。每个条目的`key`属性的值应该是一个惟一的值，比如数组索引或另一个惟一的 ID。

# 数据可以通过 Props 动态传递给组件

没有道具，大多数组件都是无用的，因为几乎所有的组件都需要数据才能发挥作用。

例如，我们可以创建一个带道具的组件，并按如下方式使用它:

```
import React from "react";const names = ["jane", "joe", "alex"];const Name = ({ name }) => <p>{name}</p>;export default function App() {
  return (
    <div className="App">
      {names.map((n, i) => (
        <Name key={i} name={n} />
      ))}
    </div>
  );
}
```

在上面的代码中，我们创建了使用`name`道具的`Name`组件，并将其显示在组件内部。

然后我们通过调用`names`上的`map`来使用它，将数组中的名称字符串映射到`Name`组件来呈现它。

我们从`names`数组传入了带有值`n`的`name`属性，因此`name`属性将由数组中的名称字符串填充。

因此，我们会看到:

```
janejoealex
```

显示在屏幕上。

如果我们有多个道具，我们可以使用 spread 操作符将它们全部分散到道具中，这样我们就不必一次将它们全部写出来。

例如，我们可以编写以下代码，将我们的道具扩展到一个组件中，如下所示:

```
import React from "react";const names = [
  { firstName: "jane", lastName: "smith" },
  { firstName: "john", lastName: "smith" },
  { firstName: "alex", lastName: "jones" }
];const Name = ({ firstName, lastName }) => (
  <p>
    {firstName} {lastName}
  </p>
);export default function App() {
  return (
    <div className="App">
      {names.map((n, i) => (
        <Name key={i} {...n} />
      ))}
    </div>
  );
}
```

在上面的代码中，我们有一个`names`数组，它有`firstName`和`lastName`属性。

当我们将道具传递给`Name`组件时，为了使我们的代码更短，我们通过使用 spread 操作符将每个条目的属性扩展为道具，就像我们在:

```
<Name key={i} {...n} />
```

只要属性和道具名称完全匹配，并且在名称嵌套层中，那么它们就会作为道具正确地分布。

由于`firstName`和`lastName`都是顶级的，我们可以用 spread 运算符轻松地将它们展开，它们将是同名道具的值。

![](img/39b058f802acd460f2214b662c8b3c56.png)

Photo by [Curtis MacNewton](https://unsplash.com/@curtismacnewton?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

React 组件可以在我们应用程序的任何地方。只要它们被定义，就可以被引用。

我们可以通过 props 将数据传递给组件。这使得它们很有用。我们可以通过像 HTML 属性那样显式地传递属性来传递属性，也可以使用 spread 操作符一次传递多个同名的属性。

# **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎