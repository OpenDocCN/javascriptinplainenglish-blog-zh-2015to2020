# 理解 JavaScript 中的生成器

> 原文：<https://javascript.plainenglish.io/understanding-generators-in-javascript-4a1c1011905b?source=collection_archive---------11----------------------->

## 通过实例了解 JavaScript 生成器

![](img/5e338e356c20a20b551f383993d45450.png)

Photo by [Nubelson Fernandes](https://unsplash.com/@nubelsondev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

**生成器**看起来像一个普通的函数，但是它允许我们暂停函数的执行，稍后再继续。也许你在 ES6 中听说过这个功能，或者你只是没有时间玩它。然而，这是你需要明白的事情。

在本文中，我们将通过一些实例来了解 JavaScript 中的生成器。让我们开始吧。

# 发电机功能

生成器函数提供了一个强大的选择:它们允许您通过编写一个执行不连续的函数来定义一个迭代算法。

下面你可以看到一个发电机的例子，我们将分解它，看看它是如何工作的:

```
**function*** avengersGenerator() { // Declaring the generator
  **yield** "Hulk"; // Pausing the execution
  **yield** "Thor";
  **yield** "Iron man";
  **return** "Ultron"; // Example of finishing the generator
  **yield** "Spiderman";
}

const **iterator** = avengersGenerator(); // Creating iterator

iterator.**next()**; // Iterating on the generator
```

生成器看起来与普通函数相似，唯一的区别是我们必须在关键字`function`后定义一个`*`(星号)。

```
function***** avengersGenerator() {
  ...
}
```

# 关键词“产量”

我们可以`yield`这个函数，它基本上会在到达第一个`yield`时停止函数的执行。

这里有一个例子:

```
function***** avengersGenerator() {
  **yield** "Hulk" // The execution would pause here.
  **yield** "Iron man" // When we resume we would start here.
}
```

如你所见，关键字`yield`用于停止函数的执行。

# 创建迭代器

在迭代器上，我们可以简单地调用我们的函数。这样，我们就可以准备好发电机了。

这里有一个例子:

```
const iterator = avengersGenerator();
```

所以迭代器对于生成器来说非常重要。具体来说，迭代器是通过使用一个返回具有两个属性的对象的`**next()**`方法来实现[迭代器协议](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterator_protocol)的任何对象:

属性`value`是迭代序列中的下一个值。以及属性`done`，如果序列中的最后一个值已经被消耗，则该属性为`true`。如果“值”出现在“完成”旁边，它就是迭代器的返回值。

# 下一个方法

这使我们能够继续执行该功能。此外，该方法还为我们提供了具有生成值的对象，以及生成器是否已经生成了它的最后一个值，如布尔值。

这里有一个例子:

```
iterator.next(); // [1] Object {value: "Hulk", done: false}
iterator.next(); // [2] Object {value: "Thor", done: false}
iterator.next(); // [3] Object {value: "Iron man", done: false}
iterator.next(); // [4] Object {value: undefined, done: true}
```

# 返回/退出

一旦调用了 return，它将完成生成器。它基本上将`done`属性设置为`true`。

这里有一个例子:

```
function* avengersGenerator() {
  yield "Hulk";
  return "Ultron"; // Example of finishing the generator
  yield "Thor"; //  Sad Thor and Spiderman wouldn't be called
  yield "Spiderman";
}

iterator.next(); // [1] Object {value: "Hulk", done: false}
iterator.next(); // [2] Object {value: "Ultron", done: true}
```

# 结论

正如您所看到的，发电机是一个很酷的玩物，或者至少知道它是做什么的。确保这只是对生成器的介绍，我鼓励你从 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators) 文档中学习更多。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/how-to-use-axios-in-vanilla-javascript-2dbf176e08d4) [## 如何在普通 JavaScript 中使用 Axios

### 通过实例了解 Axios

medium.com](https://medium.com/javascript-in-plain-english/how-to-use-axios-in-vanilla-javascript-2dbf176e08d4)