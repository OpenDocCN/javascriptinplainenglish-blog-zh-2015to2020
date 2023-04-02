# 更好的 JavaScript——状态、数组和类数组对象

> 原文：<https://javascript.plainenglish.io/better-javascript-state-and-array-vs-array-like-objects-7396a21c28c0?source=collection_archive---------12----------------------->

![](img/91b290e4d267437da3f29fe8f4c4c38c.png)

Photo by [Vidar Nordli-Mathisen](https://unsplash.com/@vidarnm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 没有不必要的状态

API 可以分为有状态的和无状态的。

无状态 API 提供其行为依赖于输入的函数或方法。

它们不会改变程序的状态。

如果我们改变了程序的状态，那么代码就更难追踪。

改变状态的程序的一部分是有状态 API。

无状态 API 更容易学习和使用，更容易自我记录，更不容易出错。

它们也更容易测试。

这是因为当我们传入一些输入时，会得到一些输出。

这不会因为外在状态而改变。

我们可以创建具有纯函数的无状态 API。

当给定一些输入时，纯函数返回一些输出。

当两个不同调用中的输入相同时，我们每次都得到相同的输出。

所以与其写:

```
c.font = "14px";
c.textAlign = "center";
c.fillText("hello, world!", 75, 25);
```

我们写道:

```
c.fillText("14px", "center", "hello, world!", 75, 25);
```

第二种情况是一个接受输入的函数，它不像上一个例子那样依赖于`font`和`textAlign`属性。

无状态 API 也更加简洁。

有状态 API 导致了设置对象内部状态的附加语句的激增。

# 为灵活的接口使用结构化类型

JavaScript 对象是灵活的，所以我们可以只创建对象文字来创建接口，其中包含我们想要向公众公开的项目。

例如，我们可以写:

```
const book = {
  getTitle() {
    /* ... */
  },
  getAuthor() {
    /* ... */
  },
  toHTML() {
    /* ... */
  }
}
```

我们有一些方法想公之于众。

我们只是提供这个对象作为任何使用我们库的接口。

这是创建外部世界可以使用的 API 的最简单的方法。

# 区分数组和类似数组的对象

JavaScript 有数组和类似数组的对象。

它们不一样。

数组有自己的方法，可以按顺序存储数据。

它们也是`Array`构造函数的一个实例。

类似数组的对象没有数组方法。

为了检查某个东西是否是一个数组，我们调用`Array.isArray`方法来检查。

如果它返回`false`，那么它不是一个数组。

类似数组的对象可以是可迭代的，也可以不是。

如果它们是可迭代的，那么我们可以用 spread 操作符把它们转换成一个数组。

例如，我们可以将节点列表和`arguments`对象转换成一个数组:

```
[...document.querySelectorAll('div')]
[...arguments]
```

我们将 NodeList 和`arguments`对象转换成一个数组。

如果它是一个不可迭代的类似数组的对象，即一个具有非负整数键和一个`length`属性的对象，我们可以使用`Array.from`方法来进行转换。

例如，我们可以写:

```
const arr = Array.from({
  0: 'foo',
  1: 'bar',
  length: 2
})
```

那么`arr`就是:

```
["foo", "bar"]
```

![](img/77b598f0ca28c5304d027b16457bcf1a.png)

Photo by [Michael](https://unsplash.com/@michael75?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们的程序中不应该有不必要的状态。

还有，鸭式分型有利于识别类型。

我们应该区分数组和类似数组的对象。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**