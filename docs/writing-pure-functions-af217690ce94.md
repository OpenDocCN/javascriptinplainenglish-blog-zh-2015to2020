# 编写纯函数的简单性

> 原文：<https://javascript.plainenglish.io/writing-pure-functions-af217690ce94?source=collection_archive---------12----------------------->

## JavaScript，React，Redux

![](img/0721b3ff262ab430e2353a27909e666f.png)

Photo by [Samara Doole](https://unsplash.com/@samaradoole?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/clear-water?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 纯函数介绍

纯函数由于其可预测性和重用的潜力，在函数式编程中作为构建块受到高度青睐。

有几个特征将一个函数定义为“纯的”。

1.  如果给定相同的输入，你将得到相同的输出。
2.  没有副作用。

## 可预测的产量

我们可以将一个参数传递给一个纯函数，并且知道无论我们运行这段代码多少次，我们都会得到相同的结果。请看下面这个例子。不管我们把 x 赋值给什么，我们都会把 x 的平方取回来。

```
function getSquare(x) {
 return x * x;
}
```

## 无副作用/独立

纯函数总是不可变的。他们不应该改变任何外部变量。考虑映射数组和返回新数组与使用。按()将新项目铲入现有数组。阶梯会改变原始数组，被认为是不纯的。改变共享状态的危险在于，另一个函数可能依赖于原始状态。现在它已经被改变了，这可能会引入一些可能难以追踪的错误。

## 纯度测试

让我们将这种洞察力付诸行动。
这些例子哪个是纯的，哪个是不纯的？

```
Option A:
function double(x) {
    return x + x;
}
function doubleAll(list) {
    return list.map(double);
} Option B:
function double(x) {
    networkCall(x);
    return x + x;
}

function doubleAll(list) {
    for (let i = 0; i < list.length; i++) {
        list[i] = double(list[i]);
    }
    return list;
}
```

答案:
选项 A 是纯粹的！我们可以期望得到一个全新的数组，它是通过映射原始数组而创建的，但没有以任何方式改变它。

选项 B 是不纯的，因为它依赖于网络调用的输出，并且它循环遍历一个项目列表以返回一个改变的原始列表。如果任何其他函数依赖于该列表，这可能会导致我们的程序崩溃或不按我们预期的方式运行。

## 反应和重复使用

使用 React 时，您的应用程序可以由类组件和功能组件组合而成。不同之处在于，功能组件由纯功能组成，因此不包含任何状态。它们不应该改变任何外部或共享状态。尽可能实现功能组件是优化的首选，因为它们是最简单和最可重用的组件。同样值得注意的是，这些功能组件并不意味着包含生命周期方法。

Redux 的一个关键原则是，它需要纯函数来用任何传入的更改更新全局状态树。我们称这些纯函数为归约函数。它们采用前一个状态和我们提供的返回下一个状态的动作，保持前一个状态不变。

在下图中，您可以看到我们如何在 Redux 中更新状态，而不改变原始状态。我们分派一个传递给 reducer 的动作。reducer 获取当前状态的信息，并使用一个 pure 函数根据我们作为参数给出的动作用新状态更新存储。

![](img/ed1f342d2d126f1c8af934edc9b20f70.png)

[Brad Westfall @ css-tricks.com](https://css-tricks.com/learning-react-redux/)

## 最后

根据 Redux 文档，“手工编写不可变的更新逻辑*是*困难的，并且在 Redux 用户中偶然改变状态是最常见的错误”。

有一个名为 Immer 的 JavaScript 库，它将我们的可变代码转换成可接受的纯函数。甚至有一种叫做 createSlice 的特殊方法，你可以使用它自动合并 Immer。你可以在这里了解更多关于[的信息。](https://redux.js.org/tutorials/essentials/part-2-app-structure)

在此之前，为了获得更优化、可重用的代码，在任何可能的地方熟悉识别和公式化纯函数都是值得的。

## 资源

[什么是纯函数](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)
[纯函数与不纯函数](http://net-informations.com/js/iq/pure.htm)