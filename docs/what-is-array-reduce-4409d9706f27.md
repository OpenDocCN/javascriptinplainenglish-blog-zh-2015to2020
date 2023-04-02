# JavaScript 中的 Array.reduce()是什么？

> 原文：<https://javascript.plainenglish.io/what-is-array-reduce-4409d9706f27?source=collection_archive---------5----------------------->

![](img/3e242c6601d15084407cb97043045f76.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

发布版 [ECMAScript 2015 语言规范](https://www.ecma-international.org/ecma-262/6.0/)(通常称为 ES6)为 JavaScript 引入了一系列新功能，包括在`Array`原型上的一些新功能。当我第一次发现这些并开始尝试在我的代码中使用它们时，它们中的大多数看起来非常简单。当您需要以某种统一的方式转换数组的每个元素时，`map`是显而易见的选择。如果我们需要根据一些标准过滤掉一些元素，那么很容易假设`filter`会完成这个任务。但是`Array.reduce`是做什么的，它是如何工作的，我们什么时候想要使用它？这篇文章旨在回答这些问题，并提供一些如何在现实生活中应用的例子。

# 它是做什么的？

事实证明，这个函数的通用性和强大之处在于它缺乏专门化。像`Array.map`一样，`Array.reduce`遍历数组并调用每个元素的回调函数。不同之处在于，它不是返回包含回调在每个元素中返回的内容的大小相等的数组，而是返回回调的最终调用返回的内容。这很有用，因为除了当前元素和索引之外，回调还会接受上一次调用回调时返回的值。 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 将其描述为“累加器”,因为在大多数情况下，您将以某种有意义的方式修改这个值，并将其返回供下一次迭代使用。`reduce`函数将用于第一次迭代的初始值作为第二个参数。我意识到这个描述并不比[规范](https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.reduce)更清晰，如果你和我一样，一个简单的例子会让你更容易完全理解。

# 将数组简化为其元素之和

每个程序员在某个时候都不得不这样做，如果你没有使用`reduce`，你的解决方案可能看起来像这样:

```
let arr = [1, 2, 3, 4, 5]
let sum = 0
for (let i = 0; i < arr.length; i++) {
  sum += arr[i]
}
console.log(sum) // 15
```

下面是你如何使用`Array.reduce`来做这件事:

```
let arr = [1, 2, 3, 4, 5]
let sum = arr.reduce((prev, cur) => prev + cur, 0)
console.log(sum) // 15
```

首先，让我们快速确定函数中包含的内容。作为参考，下面是`Array.reduce`的定义:

`Array.prototype.reduce(cb(prev, cur, idx, arr)[, initialValue])`

所以我们知道`reduce`将一个回调函数和一个初始值作为它的参数，回调函数将前一个结果(我们调用`acc`作为‘累加器’)、当前索引和我们正在处理的数组的副本作为它的参数。在这种情况下，回调是箭头函数`(prev, cur) => prev + cur`，初始值是`0`。在这个例子中，我们不关心当前的索引或者原始的数组。

注意:不要害怕箭头函数，它所做的只是返回`prev`和`cur`的和。

让我们玩调试器，在数组的每个元素上逐步调用`(prev, cur) => prev + cur`:

1.  初始值是`0`，所以`callback(prev: 0, cur: arr[0])`会执行`0 + 1`，求值到`1`。
2.  该值被用作下一个调用的第一个参数，因此`callback(prev: 1, cur: arr[1])`将执行`1 + 2`并评估为`3`。
3.  `callback(prev: 3, cur: arr[2])`执行`3 + 3`并评估为`6`。
4.  `callback(prev: 6, cur: arr[3])`执行`6 + 4`并评估为`10`。
5.  `callback(prev: 10, cur: arr[4])`执行`10 + 5`并评估为`15`。

# 从数组创建地图

有时候，当您从 API 或数据库中获取一组对象时，您需要能够通过 id 或其他一些属性轻松地访问它们。这是我最喜欢使用的`reduce`之一。假设我们收到了以下数组:

```
const data = [{
    id: 01395,
    name: 'Adam', 
    phone: 1233454567
  }, {
    id: 8593,
    name: 'Beth',
    phone: 3452348765
  }, {
    id: 3824,
    name: 'Carol'
  },
  ...
]
```

我们有了这些数据，我们需要能够使用`id`找到一个人的`phone`和`name`。因为我们的程序需要多次这样做，所以遍历数组直到找到一个带有我们正在寻找的`id`的对象是没有意义的。相反，我们可以迭代一次，并使用`reduce`创建一个键为`id`的对象。

```
const users = data.reduce((acc, cur) => {
  acc[cur.id] = cur
  return acc
}, {})
```

在这种情况下，我们使用一个空对象作为初始值，并在每次迭代中向它添加属性，使用`cur.id`作为键，使用`cur`作为值。这使得`users`看起来像这样:

```
{ 
  01395: {
    id: 01385,
    name: 'Adam',
    phone: 1233454567
  }, {
...
}
```

现在我们可以通过元素的`id`来查找其中一个元素:

```
let user = users[id]
```

# 警告:不要什么都用它

当我第一次知道这个函数是如何工作的时候，我有点兴奋，并开始用它来做各种实际上应该用传统循环来做的事情。像我一样发疯，开始使用`reduce`将对象数组聚合成其他漂亮的对象，这可能不是一个好主意。代码很容易失去可读性和性能，所以您可以这样做并不意味着您应该这样做。

编码快乐！