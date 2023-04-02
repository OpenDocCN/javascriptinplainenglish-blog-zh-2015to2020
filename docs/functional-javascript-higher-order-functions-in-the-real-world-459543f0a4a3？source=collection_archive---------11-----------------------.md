# 函数式 JavaScript——现实世界中的高阶函数

> 原文：<https://javascript.plainenglish.io/functional-javascript-higher-order-functions-in-the-real-world-459543f0a4a3?source=collection_archive---------11----------------------->

![](img/73c8bbada7fb343bd153507c6c8c1df7.png)

Photo by [Kyle Glenn](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将研究如何使用高阶函数。

# 现实世界中的高阶函数

高阶函数在现实世界中被大量使用。

例如，数组有许多高阶函数的实例方法。

其中之一就是`every`法。

`every`接受一个返回布尔表达式的回调，以检查每一项是否都是我们要找的。

例如，我们可以这样使用它:

```
const allEven = [1, 2, 3].every(a => a % 2 === 0);
```

我们传入一个回调函数来检查每个条目是否能被 2 整除。

这应该返回`false`，因为我们有 1 和 3 是奇数。

另外，`every`方法可以用我们自己的代码实现:

```
const every = (arr, fn) => {
  for (const a of arr) {
    if (!fn(a)) {
      return false;
    }
  }
  return true;
}
```

我们循环遍历`arr`的条目，然后调用`fn`来检查条目是否匹配给定的条件。

如果是，那么我们返回`false`,因为我们至少有一项与回调中的给定项不匹配。

我们可以通过书写来使用它:

```
const allEven = every([1, 2, 3], a => a % 2 === 0)
```

而`allEven`应该是`false`。

# 一些功能

`some`方法与`every`相似。

它也是数组实例的一部分。

例如，我们可以通过编写以下代码来调用数组实例的`some`方法:

```
const hasEven = [1, 2, 3].some(a => a % 2 === 0)
```

我们在数组上调用`some`方法。

回调返回我们正在寻找的条件。

它检查是否至少有一项符合给定的条件。

所以，既然 2 是偶数，`hasEven`就应该是`true`。

还有，我们可以用自己的方式实现。

例如，我们可以写:

```
const some = (arr, fn) => {
  for (const a of arr) {
    if (fn(a)) {
      return true;
    }
  }
  return false;
}
```

我们循环遍历这些项目，并检查`fn(a)`是否返回`true`。

如果有，那么我们返回`true`。

否则，我们返回`false`。

我们可以通过编写以下代码来调用我们自己的`some`函数:

```
const hasEven = some([1, 2, 3], a => a % 2 === 0)
```

然后我们得到`true`。

我们传入数组和回调函数，回调函数返回我们正在检查的函数。

# 分类

数组实例`sort`方法采用一个函数，让我们比较两个条目并对它们进行排序。

回调使用两个参数，这是数组的两个条目。

如果第一个参数应该在第二个之前，那么我们返回一个负数。

如果我们保持同样的顺序，那么我们返回 0。

否则，我们返回一个正数。

我们可以通过创建一个返回比较器函数的函数来改进这一点。

例如，我们可以写:

```
const sortBy = (property) => {
  return (a, b) => a[property] - b[property];
}const arr = [{
    foo: 3
  },
  {
    foo: 1
  },
  {
    foo: 2
  }
]const sorted = arr.sort(sortBy('foo'));
console.log(sorted);
```

我们有`sortBy`函数，它返回一个函数让我们比较一个属性值。

然后我们用我们的`sortBy`函数调用`arr.sort`，它返回我们想要的属性的比较器函数。

那么`sorted`应该是:

```
[
  {
    "foo": 1
  },
  {
    "foo": 2
  },
  {
    "foo": 3
  }
]
```

我们可以看到物品已经分类。

![](img/6c34fda14e4072bcf2bdc3ff435740d3.png)

Photo by [Brett Zeck](https://unsplash.com/@iambrettzeck?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用自己的方式实现各种数组方法。

高阶函数在现实世界中有很多应用。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**