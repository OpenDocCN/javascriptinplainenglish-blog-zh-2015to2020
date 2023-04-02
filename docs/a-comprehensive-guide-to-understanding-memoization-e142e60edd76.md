# 理解记忆的综合指南

> 原文：<https://javascript.plainenglish.io/a-comprehensive-guide-to-understanding-memoization-e142e60edd76?source=collection_archive---------9----------------------->

## 让我们从基础开始，从概念上定义记忆化到底是什么。

根据维基百科，记忆化是一种优化技术，主要用于通过存储昂贵的函数调用的结果并在相同的输入再次出现时返回缓存的结果来加速计算机程序。

![](img/6597a2c9d2b52cbfb4f9eaf682c48eb9.png)

简单地说，记忆化是使长递归/迭代函数运行得更快的编程实践。

在本文中，我将解释我们如何使用 JavaScript 实现这一点，并通过一个非常耗时的算法让它快速运行来给你一个例子。真的很快！

让我们从一个非常基本的例子开始:

```
const add = (a, b) => a + b;console.log(add(2,3));   // => returns 5
```

每次我们调用这个函数时，都会执行一次计算并返回一个值。如果我们多次调用同一个函数，计算也要进行多次。

在这种情况下，这是一个非常简单的计算，但想象一下，如果你有复杂的东西在那里。这需要一些时间来执行。这就是记忆可以帮助的地方。

现在让我们想一想。如果我们用相同的参数反复运行同一个函数，并且这个函数没有副作用，那么我们已经知道结果了，对吗？因此，我们可以简单地缓存第一次运行的结果，并在随后的调用中使用相同的参数立即返回它，而不是一次又一次地运行计算。

那是记忆化！现在，让我们创建一个简单的通用 memoize 函数，它接受另一个函数并修改它来 memoize 调用。

```
const memoize = (fn) => { 
  let cache = {}; 
  return (...args) => { 
    let n = JSON.stringify(args); 
    if (n in cache) { 
      console.log('Fetching from cache', n); 
      return cache[n]; 
    } else { 
      console.log('Calculating result', n); 
      let result = fn(...args); 
      cache[n] = result; 
      return result; 
    } 
  } 
}
```

现在我们可以很容易地记忆任何纯函数，比如我们的 *add* 函数。由于 *memoize* 是一个高阶函数，我们有两种方法来实现。一个是创建一个新的记忆函数，如下所示。

```
const add = (a, b) => a + b; 
const memoAdd = memoize(add); console.log(memoAdd(2,3));  // => Calculating result 5 console.log(memoAdd(2,3));  // => Fetching from cache 5
```

或者我们可以改变*增加*这个功能:

```
const add = memoize((a, b) => a + b); console.log(add(2,3));  // => Calculating result 5 console.log(add(2,3));  // => Fetching from cache 5
```

您想看看性能提升的效果吗？看看下面这个例子。

```
*// recursive fibonacci function*
const fibonacci = (n) => {
  if (n === 0 || n === 1) {
    return n;
  }
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```

这个 *fibonacci* 函数接受一个表示 fibonacci 序列位置的数字，并计算相应的值。因为它是递归的，当你设置的位置越高，它就越慢。

```
console.log(fibonacci(3));  // => super fast.
console.log(fibonacci(6));  // => super fast.
console.log(fibonacci(9));  // => super fast.
```

让我们疯狂一下，用序列中的第 50 个元素来测试它，好吗？

```
console.log(fibonacci(50)); // Call the function and go for a walk!
```

在我的电脑上，这段代码运行了将近 4 分钟。递归函数必须调用 400 亿次以上(具体是 40730022147 次)！这太疯狂了。

现在让我们记住它。由于斐波那契是一个递归函数，我们不能通过创建一个新的记忆函数来实现这一点，比如下面的代码。

```
const memoFibonacci = memoize(fibonacci);console.log(memoFibonacci(50)); // insane amount of time
```

这是因为我们只记住了第一个调用。对 fibonacci 的所有后续递归调用都不会被记忆。记忆递归函数的唯一方法是用我们的 memoize fn 包装 main 函数。

```
*// recursive and memoized fibonacci function*
const fibonacci = memoize((n) => {
  if (n === 0 || n === 1) {
    return n;
  }
  return fibonacci(n - 1) + fibonacci(n - 2);
})console.log(fibonacci(50)); // runs fast!
```

现在，它运行不到一秒钟，结果完全相同。太神奇了！正如您在控制台中看到的，许多调用都被缓存，防止了数十亿次递归调用。

# 记忆化和缓存一样吗？

是的，有点。记忆化实际上是一种特定类型的缓存。虽然高速缓存通常指的是将来使用的任何存储技术，但是记忆化特别涉及到*高速缓存*函数的返回值。

# 仔细的

请记住，记忆化并不是对所有的函数调用都有意义。对于不纯的函数，也就是有副作用的函数，这种方法效果不好。Memoizing 只允许我们缓存一个结果，所以任何其他副作用都会在后续调用中丢失。也就是说，您可以通过返回一个包含副作用的函数来绕过这个约束，您需要在获得结果后执行这个函数。

此外，内存开销更大，因为我们必须存储缓存的结果，以便以后可以调用它们，并且使用内存化增加了复杂性，因此它只对计算量大的函数有意义。

就是这样！感谢您的阅读和愉快的回忆！

*更多内容尽在*[***plain English . io***](http://plainenglish.io/)