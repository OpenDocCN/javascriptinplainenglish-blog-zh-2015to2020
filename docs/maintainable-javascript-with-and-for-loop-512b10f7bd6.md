# 可维护的 JavaScript——使用 and 和 for 循环

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-with-and-for-loop-512b10f7bd6?source=collection_archive---------11----------------------->

![](img/2d98f4cc9a3056ce5d3e75e3a2b08bbc.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看各种块语句来了解创建可维护 JavaScript 代码的基础。

# with 语句

永远不要使用`with`语句。

它通过创建一个`with`块来在自己的上下文中操作对象。

例如，我们可以写:

```
var book = {
  title: "javascript for beginners",
  author: "james smith"
};with(book) {
  message = `${title} by ${author}`;  
}
```

将`message`属性添加到`book`。

然而，这在严格模式下是不允许的，因为它的范围令人困惑。

从代码中我们无法确定`message`是全局变量还是`book`的属性。

同样的问题阻碍了优化的完成，因为 JavaScript 引擎可能会猜错。

所以，千万不要用这个。

这在所有风格指南中也是禁止的。

Linters 可以检查这一点，这样我们就不会不小心写了`with`语句。

# for 循环

`for`循环是 JavaScript 中的一种循环，继承自 C 和 Java。

还有 for-in 和 for-of 循环，让我们分别遍历对象的属性和可迭代对象的条目。

例如，我们可以写:

```
const values = [1, 2, 3, 4, 5],
  len = values.length;for (let i = 0; i < len; i++) {
  console.log(values[i]);
}
```

我们创建了一个 for 循环，通过定义`values`数组并将其`length`设置为`len`来缓存它，从而遍历一些数字。

有两种方法可以改变循环进行的方式。

一种情况是使用`break`语句。

`break`将结束循环，不继续下一次迭代。

例如，我们可以写:

```
const values = [1, 2, 3, 4, 5],
  len = values.length;for (let i = 0; i < len; i++) {
  if (i === 2) {
    break;
  }
  console.log(values[i]);
}
```

当`i`为 2 时结束循环。

另一种改变循环行为的方法是使用`continue`关键字。

这让我们跳到循环的下一次迭代。

例如，我们可以写:

```
const values = [1, 2, 3, 4, 5],
  len = values.length;for (let i = 0; i < len; i++) {
  if (i === 2) {
    continue;
  }
  console.log(values[i]);
}
```

然后当`i`为 2 时，我们将跳到下一次迭代。

一些风格指南如 Doug Crockford 的风格指南禁止使用`continue`。

他的理由是，有条件才能写得更好。

例如，不写:

```
const values = [1, 2, 3, 4, 5],
  len = values.length;for (let i = 0; i < len; i++) {
  if (i === 2) {
    continue;
  }
  console.log(values[i]);
}
```

我们可以写:

```
const values = [1, 2, 3, 4, 5],
  len = values.length;for (let i = 0; i < len; i++) {
  if (i !== 2) {
    console.log(values[i]);
  }
}
```

他说程序员理解条件比理解`continue`更容易。

不经常用作循环控制语句，所以我们可以不用它而使用条件语句。

![](img/8e2defc7ad1f081a0d1b6950f949b8bd.png)

Photo by [Reza Tahvili](https://unsplash.com/@shotbyrez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`with`永远不要使用语句。在严格模式下也是禁用的。

在循环中使用`continue`关键字之前，我们应该三思。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**