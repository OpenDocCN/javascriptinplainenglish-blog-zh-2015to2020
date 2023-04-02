# 你应该知道的 5 种高级 JavaScript 技术

> 原文：<https://javascript.plainenglish.io/5-advanced-javascript-techniques-you-should-know-4362e26f2ca8?source=collection_archive---------0----------------------->

## 让我们节省时间，用这些技术提高工作效率和专业水平

![](img/ca9c664e03a801b4c12449b526c29520.png)

Photo by [Kevin Canlas](https://unsplash.com/@kvncnls?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你在寻找更多的 JavaScript 技术吗？

如果是这样，你来对地方了。

在本文中，我将向您展示我用来提高代码库质量的 5 种高级 JavaScript 技术。

让我们开始吧。

# 1.关闭

闭包是 Javascript 中的一项重要技术。这意味着内部函数总是可以访问外部函数的变量和参数，即使外部函数已经返回。

我们使用闭包来保护不想暴露给外部范围的数据。

假设您想在每次点击按钮时将一个`counter`变量增加一个单位。类似于:

```
<button onclick=”increaseCounter()”>Increase Counter</button>
```

简单地说，我们可以用一个全局来完成这个任务:

```
let counter = 0;function increaseCounter() {
  counter++;
}
```

问题是不需要调用`increaseCounter()`函数就可以直接改变`counter`变量。

因此，让我们将`counter`变量移到函数中:

```
function increaseCounter() {
  let counter = 0;
  counter++;
}
```

不，每次函数被调用时，`counter`变量将被重置为`0`。

这就是终结发挥作用的时候:

```
const increaseCounter = (function() {
  let counter = 0;

  return function() {
    counter = counter + 1;
    console.log(counter);
  };
})();
```

IIFE 只运行一次，用`0`初始化`counter`变量，然后返回一个可以访问`counter`变量的函数。所以，当你调用`increaseCounter()`时，它会按预期更新`counter`。

# 2.功能连接

我敢打赌，你曾经面临过这样的问题:一个`this`并不是你所期望的`this`。具体来说，当您将对象方法作为回调传递时，`this`会丢失。请参见下面的示例:

```
let book = {
  title: ‘Learn JavaScript’,
  printTitle() {
    console.log(`Book’s title: ${this.title}`);
  }
}setTimeout(book.printTitle, 1000); // Book’s title: undefined
```

我们期望的打印文本是`Book’s title: Learn JavaScript`，但我们得到的是`Book’s title: undefined`。是因为程序已经不知道`this`了。现在上下文丢失了。

为了解决这个问题，我们可以使用函数绑定技术。

```
let book = {
  title: ‘Learn JavaScript’,
  printTitle() {
    console.log(`Book’s title: ${this.title}`);
  }
}let printTitle = book.printTitle.bind(book);setTimeout(printTitle, 1000); // Book’s title: JavaScript
```

我写过一篇关于函数绑定的详细文章。如果您想了解更多关于这种技术的知识，您应该查看这篇文章:

[](https://medium.com/javascript-in-plain-english/how-to-control-this-better-in-javascript-dcacd54bcf97) [## 如何在 JavaScript 中更好地控制“this”

### 使用 call()、apply()和 bind()方法

medium.com](https://medium.com/javascript-in-plain-english/how-to-control-this-better-in-javascript-dcacd54bcf97) 

# 3.任务调度

JavaScript 提供了两种简单的方法来调度任务的执行:

`setInterval()`以一定的时间间隔重复运行任务:

```
function sayHello() {
  console.log(‘Hello from the other side!’);
}setInterval(sayHello, 1000);
```

在上面的代码片段中，每隔 1000 毫秒就会重复执行`sayHello()`功能。

`setTimeout()`每隔一段时间运行一次任务*:*

```
*function sayHello() {
  console.log(‘Hello from the other side!’);
}setTimeout(sayHello, 1000);*
```

*现在，`sayHello()`功能将在 1000 毫秒后只运行一次。*

# *4.递归*

*编程中的一些任务用传统方法很难解决。它们需要大量的逻辑和思考。然而，如果您应用递归技术，事情会简单得多。这是一种将一个大任务分解成几个更小更简单的同类任务的技术，你不用想太多就能理解。*

*当你看到一个函数调用它自己时，你就会知道递归技术在使用。*

*例如，我们将编写一个以`x`和`n`为参数的函数，并返回`x`乘以自身`n`的值。*

*对于传统方法，我们将使用循环语句:*

```
*function pow(x, n) {
  let result = 1; for (let i = 0; i < n; i++) {
    result = result * x;
  } return result;
}console.log(pow(3, 5)); // 243*
```

*当我们应用递归方法时，它会是这样的:*

```
*function pow(x, n) {
  if (n === 1) {
    return x;
  } else {
    return x * pow(x, n — 1);
  }
}console.log(pow(3, 5)); // 243*
```

*当`n === 1`的时候，结果是如此的明显。这里没什么好讲的，因为`pow(x, 1)`会返回`x`。*

*如果`n`大于`1`，那么我们用数学常识将`pow(x, n)`表示为`x * pow(x, n — 1)`(例:x⁷ = x * x⁶).我们一直这样后退一步，直到`n === 1`。*

*以上只是一个简单的递归例子，你也可以用传统的方法轻松实现。然而，在您的编程生涯中，您会遇到为了简单起见而更愿意使用递归来解决的任务。*

*当然，您可以将递归实现转换为迭代实现。但在某些情况下，这需要付出很多努力。更不用说你的代码会很难看，很难读懂。*

*最后但同样重要的是，如果您不小心使用递归，它可能会损害性能。所以，明智地使用它。*

# *5.定义名称空间*

*添加到项目中的代码越多，发生冲突的可能性就越大。*

*避免冲突的最好方法之一是使用名称空间。这是一种允许您将代码的一部分包装在唯一名称下的技术。*

*你可能知道但没有意识到的命名空间的最简单的例子是对象:*

```
*let animal = {
  move: () => {
    console.log(‘Move!’);
  },
  jump: () => {
    consle.log(‘Jump!’);
  }
};*
```

*在本例中，`move()`和`jump()`函数的命名空间在`animal`下。我们用`animal.move()`和`animal.jump()`来称呼它们。*

*在有大量源代码文件的真实项目中，我们使用名称空间的方式如下:*

```
*if (typeof APP === “undefined”) {
  APP = {};
  APP.ANIMAL = {};
}APP.ANIMAL.move = () => {
  console.log(‘Move’);
};APP.ANIMAL.jump = () => {
  console.log(‘Jump’);
};APP.ANIMAL.move(); // Move
APP.ANIMAL.jump(); // Jump*
```

*通过创建这样一个独特的名称空间，您可以避免所有恼人的冲突。*

# *结论*

*以上是我想向你介绍的 5 种提高代码质量的 JavaScript 技术。如果你能把它们应用到你的实际项目中，那就太好了。*

*你知道其他很酷的技术吗？请在下面的评论中告诉我。*

*希望你喜欢这篇文章。*

*[***和我一起分享更多关于编程的有益见解。***](https://bracketshack.substack.com/)*

## *进一步阅读*

*[](https://medium.com/javascript-in-plain-english/9-tips-for-writing-scalable-javascript-code-e6bcfc791882) [## 编写可伸缩 JavaScript 代码的 9 个技巧

### 您应该从一开始就准备好扩展您的项目

medium.com](https://medium.com/javascript-in-plain-english/9-tips-for-writing-scalable-javascript-code-e6bcfc791882)*