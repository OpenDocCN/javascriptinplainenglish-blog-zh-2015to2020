# JavaScript 基础——高阶函数

> 原文：<https://javascript.plainenglish.io/javascript-basics-higher-order-functions-bf793924880?source=collection_archive---------5----------------------->

![](img/0caf8cc1367518b28f468411d2d5999d.png)

Photo by [Eva Waardenburg Photography](https://unsplash.com/@cantusamator?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究高阶函数。

# 高阶函数

高阶函数是以其他函数作为参数或返回一个函数的函数。

这在 JavaScript 中是可能的，因为函数是常规对象，所以它们可以作为参数接受并返回。

例如，我们可以写一个高阶函数:

```
const foo = (fn) => {
  //...
  fn();
  //...
}
```

我们调用了一个作为参数传递给另一个函数的函数。

我们也可以在函数中返回一个函数:

```
const foo = () => {
  //...    
  return (a, b) => {
    //...
  }
}
```

当我们处理重复的函数调用时，这给了我们灵活性。

# 抽象重复

高阶函数为我们抽象重复代码提供了很好的方法。

我们可以通过使用高阶函数来缩短重复代码，使它们更加灵活。

例如，不使用如下循环:

```
const results = [];
for (const a of arr) {
  if (a % 2 === 0) {
    results.push(a);
  }
}
```

我们可以使用高阶函数来创建一个更一般的循环，它可以接受任何条件:

```
const filter = (arr, predicate) => {
  const results = [];
  for (const a of arr) {
    if (predicate(a)) {
      results.push(a);
    }
  }
  return results;
}
```

`predicate`是检查`a`是否满足某些条件的函数。

这样，我们可以使用同一个循环来检查不同的条件，以便用我们正在寻找的条件返回一个数组。

例如，我们可以写:

```
const arr = filter([1, 2, 3], a => a % 2 === 0);
```

这就是高阶函数有用的原因。

我们把它们当作论点，想怎么叫就怎么叫。

# 使用地图转换

我们可以用与上面例子相同的原理创建我们自己的`map`函数。

例如，我们可以写:

```
const map = (arr, fn) => {
  const results = [];
  for (const a of arr) {
    results.push(fn(a));
  }
  return results;
}
```

在我们的函数中，我们在数组的每个条目上调用`fn`,并将结果推入一个新的数组。

然后我们返回结果数组。

一旦我们做到了这一点，我们可以这样称呼它:

```
const arr = map([1, 2, 3], a => a ** 2);
```

并得到`arr`的`[1, 4, 9]`。

# 用 Reduce 组合数组数据

同样，我们可以使用高阶函数将数组的条目组合成一个结果。

例如，我们可以写:

```
const reduce = (arr, combine, start) => {
  let result = start;
  for (const a of arr) {
    result = combine(result, a);
  }
  return result;
}
```

在上面的代码中，我们使用了`reduce`函数来设置初始值。

然后我们遍历数组来调用传入的`combine`函数。

并且我们将组合结果设置为`result`。

然后我们可以用它来组合数组整体，如下所示:

```
const result = reduce([1, 2, 3], (total, a) => total + a, 0);
```

那么`result`就是 6，因为我们传入了一个函数来将累积的条目和数组相加。

# 构成函数

高阶函数的好处是我们可以合成它们。

它们都是返回结果的纯函数。

所以我们可以把它们连接起来。

例如，我们可以将`map`和`reduce`链接在一起，然后调用它们:

```
const mapped = map([1, 2, 3], a => a ** 2)
const result = reduce(mapped, (total, a) => total + a, 0);
```

然后我们得到`result`是 14，因为我们平方了`[1, 2, 3]`的条目，然后将它们加在一起。

![](img/fd69458344b8e303880927da00f20dcb.png)

Photo by [sergee bee](https://unsplash.com/@sergeebee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

在 JavaScript 中，高阶函数是一个强大的特性，我们可以用它来减少编写重复代码时的重复。

我们可以通过创建一个函数来抽象重复的动作，这个函数允许我们把做重复工作的函数作为一个参数传入。

高阶函数之所以存在，是因为它们像其他任何东西一样是常规对象。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**