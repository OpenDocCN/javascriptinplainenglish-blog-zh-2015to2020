# React 中事件处理函数和属性的方便命名约定

> 原文：<https://javascript.plainenglish.io/handy-naming-conventions-for-event-handler-functions-props-in-react-fc1cbb791364?source=collection_archive---------2----------------------->

![](img/fbaab745336f99cd64efe33c6018c750.png)

Photo by [Ross Findon](https://unsplash.com/@rossf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

事件处理函数表面上很简单:某个事件发生了，某个函数处理这个事件。然而，为了保持 React 组件之间的一致性并保持代码的整洁，使用某种命名约定会有所帮助。

幸运的是，HTML 事件属性已经为我们完成了一半的工作！HTML 事件属性已经提供了一个非常基本的命名约定，它遵循了`onEvent`的格式(为了便于阅读，采用了驼峰式大小写)，其中`Event`是事件。例如，`onchange`、`onclick`和`onsubmit`都是遵循这种命名约定的例子。为什么在使用 React 时要改变这个约定？让我们跟着它走，让我们的生活更轻松！

# 命名您的函数

命名处理函数遵循与事件处理函数属性相似的命名约定。简单地记住，当谈到原因和结果时，这里有一个流程。鉴于事件属性坚持使用`on`前缀，我们将调整我们的处理程序命名约定，以便它*通过采用`handle`前缀来处理*事件。因此，命名约定变成了`handleSubjectEvent`，其中`Subject`是处理程序关注的事情，`Event`是正在发生的事件。

为了更加清楚，让我们看一些例子:

**约定** `handleEvent
handleSubjectEvent`

**例题** `handleNameChange`
`handleChange`
`handleFormReset`
`handleReset`

# 命名你的道具

当创建一个 React 组件，该组件有一个处理某些事情的属性时，只需遵循`onEvent`约定(如果适用，添加一个`Subject`)。

**约定** `onEvent`
`onSubjectEvent`

**例题** `onNameChange`
`onChange`
`onFormReset`
`onReset`

# 把这一切联系在一起

```
<SomeComponent
  onNameChange={handleNameChange}
  onFormReset={handleFormReset}
/>
```

遵循这些简单的命名约定可以为您的代码提供可预测性和一致性。通过实现这些命名约定，不再有猜谜游戏，不再需要提供合适的名称。

我希望这些命名约定能在您编写 React 组件时对您有所帮助。