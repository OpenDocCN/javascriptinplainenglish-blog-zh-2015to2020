# 用 JavaScript 扩展数组

> 原文：<https://javascript.plainenglish.io/spreading-arrays-with-javascript-6a2e987323?source=collection_archive---------5----------------------->

![](img/b882b18be4b39e68b52e8b81ae8040e9.png)

Photo by [Amber Maxwell Boydell](https://unsplash.com/@ambermaxwellboydell?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

spread 操作符是最近最好的 JavaScript 特性之一。

在本文中，我们将看看我们可以用 spread 操作符做些什么。

# 传播算子

spread 语法允许我们在各种情况下扩展像数组和字符串这样的可迭代对象。

# 合并数组

我们可以用它合并数组。

例如，我们可以写:

```
const a = [1, 2, 3];
const b = [4, 5, 6];
const c = [...a, ...b[;
```

我们将来自`a`和`b`的所有项目放入一个新的数组，并将其分配给`c`。

所以`c`是:

```
[1, 2, 3, 4, 5, 6]
```

# 克隆阵列

我们可以使用它的另一种方法是浅层复制一个数组。

例如，我们可以写:

```
const arr = [1, 2, 3];
const arrCopy = [...arr];
```

我们用 spread operator 复制了一份。它将`arr`条目扩展到一个新数组中。

所以`arrCopy`也是`[1, 2, 3]`。

但是它们在内存中引用不同的对象。

# 数组的参数

`arguments`对象是一个 iterable 对象，它将参数传递给一个 JavaScript 函数。

它只在传统函数中可用。

例如，我们可以写:

```
function foo() {
  const args = [...arguments];
  console.log(args);
}
```

`args`是一个数组。

所以如果我们有:

```
foo(1, 2, 3)
```

那么`args`就是`[1, 2, 3]`。

适用于所有函数的更现代的替代方法是 rest 操作符:

```
const foo = (...args) => {
  console.log(args);
}
```

那么如果我们以同样的方式称呼它:

```
foo(1, 2, 3)
```

`args`会有相同的值。

# 字符串到数组

我们可以用 spread 操作符将一个字符串转换成一个字符数组，因为字符串是可迭代的。

例如，我们可以写:

```
const string = 'hello world';
const array = [...string];
```

那么`array`就是:

```
["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
```

# 设置为数组

集合是不能有重复条目的可迭代对象。

因为它是可迭代的，所以我们可以对它使用 spread 操作符。

例如，我们可以写:

```
const set = new Set([1, 2, 2, 3, 3]);
const arr = [...set];
```

那么`arr`就是`[1, 2, 3]`，因为重复项已从集合中移除。

# 映射到数组

映射是存储在对象中的键值对。

它也是可迭代的，所以我们可以对它使用 spread 操作符。

例如，我们可以写:

```
const map = new Map([
  ['foo', 1],
  ['bar', 2]
]);
const arr = [...map];
```

我们用操作员展开地图，那么`arr`将是:

```
[
  ['foo', 1],
  ['bar', 2]
]
```

# 数组的节点列表

节点列表是 DOM 节点的列表。

它是一个可迭代的对象，但不是一个数组。

要将它们转换成数组，我们可以写:

```
const nodeList = document.querySelectorAll('div');
const nodeArr = [...nodeList];
```

我们用 spread 操作符将从`querySelectorAll`返回的节点列表转换成一个数组。

![](img/d93787f3a6fe524d939384a7280543b4.png)

Photo by [Jessy Smith](https://unsplash.com/@jessysmith?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 spread 操作符来扩展或复制数组，并将可迭代对象转换为数组。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**