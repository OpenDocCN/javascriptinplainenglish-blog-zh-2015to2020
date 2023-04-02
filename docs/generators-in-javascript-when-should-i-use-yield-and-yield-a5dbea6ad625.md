# JavaScript 中的生成器:什么时候应该使用 yield，和 yield*？

> 原文：<https://javascript.plainenglish.io/generators-in-javascript-when-should-i-use-yield-and-yield-a5dbea6ad625?source=collection_archive---------4----------------------->

![](img/5757b1d4bfb00fca40498a833026e0d0.png)

即使在 ES6 发布五年后，仍然有一些方面不是每个 JavaScript 开发人员都熟悉的。这些通常是日常代码中不使用的方面。那很好。但是，即使是看似无用的知识，实际上也从来不是无用的。这个无人知晓的 ES6 特性可能是一个让你头疼的棘手问题的完美解决方案。

其中一个功能是生成器。尽管功能非常强大，但生成器大多隐藏在有用的库中，很少在日常编程中使用。尽管如此，我们大多数人至少对关键字`yield`有一个模糊的概念。`yield*`能说那么多吗？

# 发电机

`yield`和`yield*`这两个关键词，出现在生成器的语境中，在它之外是无法理解的。

生成器是一个可以产生一系列值的对象，但也可以像数组一样被迭代。更准确地说，生成器是实现迭代器和可迭代协议的对象。

具体地说，因为生成器是迭代器，我们可以这样做:

```
const generator = ... // we will see later how to create a generatorgenerator.next();
generator.next();
generator.next();
generator.next();
```

因为它们也是可重复的，我们可以这样做:

```
const generator = ... // a bit of patiencefor (const value of generator) {
  // ...
}
```

正如你可能已经从我漏掉的部分猜到的，`yield`介入了一个发电机的创造。

# 发电机功能

发生器是使用发生器函数创建的。使用`function*`或`function *`声明这些功能。

在生成器函数中，您定义了在生成器上调用`next`函数时将要返回的值。为此，您可以使用关键字`yield`:

```
function* generatorFunction() {
  yield 1;
  yield 2;
  yield 3;
}const generator = generatorFunction();generator.next(); // { value: 1, done: false }
generator.next(); // { value: 2, done: false }
generator.next(); // { value: 3, done: false }
generator.next(); // { value: undefined, done: true }
```

当调用`next`方法时，生成器执行到下一个`yield`表达式，并返回指定的值。

`yield`其实是一条双行道。您还可以使用它来将*值传递给您的生成器。假设您想要一个生成器，它可以接收一些值作为输入，然后在每次调用`next`方法时返回这个值。你可以这样使用`yield`:*

```
function* generatorFunction() {
  const a = yield; while(true) {
   yield a;
  }
}const generator = generatorFunction();generator.next();
generator.next(1); // { value: 1, done: false }
generator.next(); // { value: 1, done: false }
generator.next(); // { value: 1, done: false }
```

你可能会觉得第一个电话很奇怪，但这不是一个错误。如前所述，当调用`next`函数时，生成器一直执行到下一个`yield`表达式。我们第一次在我们全新的发电机上调用它，它一直运行到第一个`yield`，没有任何返回，所以在那里等待。第二次，我们用一个值调用它，它将变量`a`初始化为这个值，并转到下一个`yield`，在这里它返回`a`，在我们的例子中是`1`。对于之后的每次调用，它都会产生值`a`。

# 产量*

那`yield*`呢？`yield*`也被用于生成器功能，但当然是为了实现一些不同的东西。

假设您想要创建一个返回斐波那契数列的生成器。提醒一下，斐波那契数列的定义如下:

*   第一个是 0
*   第二个是 1
*   之后的每一个数字都是前两个数字的和

换句话说:`F(0) = 0; F(1) = 1; ... F(n) = F(n-1) + F(n-2);`

您将如何创建一个生成这些值的生成器？你可能想递归地做这件事。

```
function* fibonacciGeneratorFunction() {
  yield 0;
  yield 1;
  ???
}const fibonacciGenerator = fibonacciGeneratorFunction();
```

实际上，我们希望在我们的生成器中产生另一个生成器的值。这就是`yield*`的用武之地:

```
function* fibonacciGeneratorFunction(a = 0, b = 1) {
  yield a;   
  yield* fibonacciGeneratorFunction(b, b + a);
}const fibonacciGenerator = fibonacciGeneratorFunction();fibonacciGenerator.next(); // { value: 1, done: false }
fibonacciGenerator.next(); // { value: 1, done: false }
fibonacciGenerator.next(); // { value: 2, done: false }
fibonacciGenerator.next(); // { value: 3, done: false }
fibonacciGenerator.next(); // { value: 5, done: false }
```

`yield*`不仅仅用于递归情况。它通常使您能够委托给另一个生成器。来自 [MDN web 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield*)的一个简单示例是:

```
function* g1() {
  yield 2;
  yield 3;
  yield 4;
}

function* g2() {
  yield 1;
  yield* g1();
  yield 5;
}

const iterator = g2();

console.log(iterator.next()); // {value: 1, done: false}
console.log(iterator.next()); // {value: 2, done: false}
console.log(iterator.next()); // {value: 3, done: false}
console.log(iterator.next()); // {value: 4, done: false}
console.log(iterator.next()); // {value: 5, done: false}
console.log(iterator.next()); // {value: undefined, done: true}
```

希望这篇文章揭开`yield`和`yield*`的神秘面纱。虽然两者都在生成器的上下文中使用，但是`yield`和`yield*`使您能够以不同的方式生成值。第一种允许您直接返回值或将它们提供给生成器。第二种方法允许您将值生成委托给另一个生成器。