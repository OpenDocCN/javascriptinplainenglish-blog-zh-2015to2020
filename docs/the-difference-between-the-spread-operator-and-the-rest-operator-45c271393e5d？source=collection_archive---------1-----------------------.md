# Spread 运算符和 Rest 运算符之间的区别

> 原文：<https://javascript.plainenglish.io/the-difference-between-the-spread-operator-and-the-rest-operator-45c271393e5d?source=collection_archive---------1----------------------->

## 用实际例子说明

![](img/e7b36e1f3581f6d2f909952117db1ce1.png)

Photo by [Adeolu Eletu](https://unsplash.com/@adeolueletu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

在 JavaScript 中，spread 操作符和 rest 操作符看起来完全一样。如果您不知道这两个操作符之间的区别，这可能会令人困惑。

在本文中，我们将通过一些实例来了解这些操作符之间的区别。让我们开始吧。

# 休息参数

Rest 参数帮助我们传递无限数量的函数参数。

这里有一个例子:

```
function sum(a,b,**...remaining**){
     console.log(a);
     console.log(b);
     console.log(remaining);
}
sum(1,2,3,4,5,6); // a => 1
// b => 2
// ...remaining => [3,4,5,6]
```

在上面的例子中，参数`a`和`b`指的是 1 和 2，而其余的参数`…remaining`指的是我们在 1 和 2 之后传递的所有内容。本例中的 rest 参数在调用函数时表示为数组`[3,4,5,6]`。

因此，我们使用 rest 操作符提取所有剩余的数组值，并将它们放入一个数组中。

# 扩展运算符

spread 操作符基本上是在 JavaScript 中传播或扩展 iterable 对象的值。它还允许我们在需要多个参数或元素的地方扩展数组和其他表达式。我们也可以在函数调用中使用这个操作符。

这里有一个例子:

```
function add(a, b) {
  return a + b;
};

const nums = [5, 6];
const sum = add(**...nums**);
console.log(sum);  // result is 11.
```

在这个例子中，我们在调用`add`函数时使用了 Spread 操作符。我们正在展开`nums`阵列。因此参数`a`的值将是 5，参数`b`的值将是 6。所以总数是 11。

我写了一整篇关于使用 spread 操作符的文章。有兴趣可以去看看。

[](https://medium.com/javascript-in-plain-english/5-ways-to-write-a-clean-javascript-using-the-spread-operator-f03a3d5f6340) [## 使用 Spread 运算符编写简洁 JavaScript 的 5 种方法

### 带有实际例子的扩展算子

medium.com](https://medium.com/javascript-in-plain-english/5-ways-to-write-a-clean-javascript-using-the-spread-operator-f03a3d5f6340) 

# 差异

spread 运算符和 rest 参数具有相同的运算符`…`。不同之处在于，使用 spread 操作符，我们将一个数组中的单个数据传递或扩散到另一个数据中。

另一方面，rest 参数在函数或数组中用于获取所有的参数或值，并将它们放入数组或提取其中的一部分。

*   如果您在函数调用上看到(…)点，那么它是一个扩展操作符。
*   如果您在函数参数上看到(…)点，那么它是一个 rest 参数。
*   spread 操作符帮助我们合并数组或对象。

# 结论

spread 操作符和 rest 参数在 JavaScript 中非常有用和重要。这就是为什么你需要了解它们，学习如何使用它们，并知道它们之间的区别。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/7-extremely-powerful-javascript-tricks-that-you-should-know-559bf2d267a2) [## 你应该知道的 7 个非常强大的 JavaScript 技巧

### JavaScript 的力量和实际例子

medium.com](https://medium.com/javascript-in-plain-english/7-extremely-powerful-javascript-tricks-that-you-should-know-559bf2d267a2)