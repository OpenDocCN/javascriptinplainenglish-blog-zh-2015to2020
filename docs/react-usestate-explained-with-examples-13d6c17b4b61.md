# 反应使用状态举例说明

> 原文：<https://javascript.plainenglish.io/react-usestate-explained-with-examples-13d6c17b4b61?source=collection_archive---------12----------------------->

## 通过实例了解 React UseState

![](img/e38310e522b579a96e997d952925935e.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

React 提供了一系列挂钩，允许您向组件添加特性。这些钩子是 JavaScript 函数，可以从 React 包中导入。然而，钩子只能用于基于函数的组件，所以它们不能在类组件中使用。

在本文中，我们将通过实际例子了解 React UseState 钩子。让我们开始吧。

# 什么是使用状态，我们何时使用它？

正如我所说，React 为您提供了一系列可以在应用程序中使用的钩子。然而，`useState`和`useEffect`是你将会经常用到的两个重要的钩子。

钩子`useState`是一个带一个参数的函数，这个参数是初始状态，它返回两个值:当前状态和一个可以用来更新状态的函数。如果您试图在 React dev 工具(`console.log(useState)`)中打印函数`useState()`，您会注意到它返回一个数组，该数组包含您放入函数`useState`和`undefined`中的参数，您将在其中添加一个函数来更新状态。

当您想在点击按钮或创建计数器等之后更改文本时，可以使用挂钩`useState`。

# 简单的使用状态示例

为了使用钩子`useState`，你必须首先从 React 包中导入它。

这里有一个例子:

```
import React, **{ useState }** from 'react'
```

现在你可以毫无问题地在你的代码上使用钩子了。看看下面的例子:

```
import React, **{ useState }** from 'react'

*function* Component() {
  const [name, setName] = **useState('Mehdi')**
}
```

请注意，我们在组件内部使用了 ES6 数组析构。所以数组里面的变量`name`指的是函数`useState`的自变量(当前状态)。另一方面，变量`setName`指的是您将添加来更新状态的函数。这意味着我们有一个名为`name`的状态，我们可以通过调用`setName()`函数来更新它。

让我们在 return 语句中使用它:

```
import React, **{ useState }** from 'react'

*function* Component() {
  const [name, setName] = **useState('Brad')**

  return <h1> My name is **{name}** </h1>
}//Returns: My name is Brad
```

由于功能组件没有`setState()`功能，所以需要使用`setName()`功能来更新。以下是如何将名字从“布拉德”改为“约翰”:

```
import React, **{ useState }** from 'react'

*function* Component() {
  const [name, setName] = **useState('Brad')**

  if(name === "Brad"){
    **setName("John")**
  }

  return <h1> My name is **{name}** </h1>
}//Returns: My name is John
```

# 多重使用状态

当您有多个状态时，您可以根据需要多次调用`useState`钩子。

这里有一个例子:

```
import React, { useState } from 'react'

*function* Component() {
  const [name, setName] = useState('Alex')
  const [age, setAge] = useState(15)
  const [friends, setFriends] = useState(["Brad", "Mehdi"])

  return <h1> My name is **{name}** and I'm **{age}** </h1> 
  //My name is Alex and I'm 15
}
```

注意，钩子接收所有有效的 JavaScript 数据类型，比如字符串、数字、布尔值、数组和对象。

# 结论

钩子`useState`是你必须知道的重要和有用的反作用钩子之一。而且这个钩子基本上是让功能组件有自己的内部状态，给它们添加特性。

感谢您阅读本文，希望您觉得有用。

## 更多阅读

[](https://medium.com/javascript-in-plain-english/4-useful-html5-features-you-probably-dont-know-a4be822378d0) [## 你可能不知道的 4 个有用的 HTML5 特性

### 非常有用的 HTML 特性和例子

medium.com](https://medium.com/javascript-in-plain-english/4-useful-html5-features-you-probably-dont-know-a4be822378d0)