# JavaScript 最佳实践—突变和安全性

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-mutations-and-security-89b543bd473b?source=collection_archive---------7----------------------->

![](img/2608034a5f81e581141b4d774ec07e66.png)

Photo by [JOSHUA COLEMAN](https://unsplash.com/@joshstyle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究为什么以及如何避免突变和复杂性。

此外，我们还关注安全问题。

# 不要使用代理

代理给对象操作增加了副作用，所以我们可能希望避免它们以降低复杂性。

例如，不写:

```
const handler = {
  get(target, key) {
    return Math.max(target[key], Infinity);
  }
};
const object = new Proxy(variable, handler);
object.a;
```

相反，我们创建了一个函数:

```
const bigProperty = (target, key) => {
  return Math.min(target[key], Infinity);
}
positiveProperty(object, 'a');
```

# 减少 Rest 参数的使用

Rest 参数非常方便，因为我们可以向函数传递任意数量的参数。

然而，最好有明确的参数，因为它们更容易处理。

带有 rest 参数的函数不太适合 currying，因为参数的数量不确定。

因此，不使用:

```
const sum = (...nums) => {
  return nums.reduce((a, b) => a + b);
}
```

我们写道:

```
const sum = (nums) => {
  return nums.reduce((a, b) => a + b);
}
```

`nums`是第二个例子中的数组。

在一个数组中有一个固定的参数比有无限多个参数要好。

# 减少这个的使用

使用`this`应该减少，因为使用`this`意味着我们有一个突变的内部状态。

另外，`this`在 JavaScript 中也很混乱。

因此，如果我们使用它，它将是一片混乱。`this`的值根据作用域而变化。

# 投掷的使用

我们不应该过多地使用`throw`。它应该保留给我们在应用程序中无法解决的问题。

它还应该用于最关键的错误，以便每个人都可以立即看到它们。

在其他情况下，我们可能希望返回数据，而不是在我们的函数中。

所以与其写:

```
const throwAnError = () => {
  throw new Error('error');
}
```

我们可以写:

```
const returnAnError() {
  return new Error('error');
}
```

# 没有未使用的表达式

我们的代码中不应该有任何未使用的表达式。

所以像这样的事情:

```
1 + 2
```

不应该出现在我们的代码中，因为它毫无用处。

相反，我们应该将表达式赋给一个变量，或者将它们作为函数的参数传入，以便使用它们。

所以我们可以写:

```
const sum = 1 + 2;
```

# 没有字段值

字段是无用的，因为它只是将存储在对象中的值转换成原始值。

这是一种迂回且令人困惑的创建原始值的方式。

相反，我们应该只创建原始值。

所以与其写:

```
const obj = {
  value: 200,
  valueOf() { return this.value; }
};
```

我们只是写:

```
const a = 200;
```

# 总是错误地使用 new

`Error`是一个构造函数，所以我们应该和`new`一起使用。

在 JavaScript 中，如果不使用类语法，很容易跳过`new`。

如果我们把它忘在其他地方，我们会得到意想不到的结果。

所以为了保持一致，我们应该在任何地方使用`new`，包括当我们创建`Error`对象的时候。

例如，不写:

```
throw Error('foo');
```

我们写道:

```
throw new Error('foo');
```

# 安全风险

像正则表达式这样的东西存在安全风险。

我们在使用它们时应该小心，以免它们破坏我们的应用程序。

# 正则表达式 DoS 和 Node.js

Node.js 在针对长字符串检查大型正则表达式方面存在问题。

这些大表达式阻塞了事件循环，因此我们的应用程序将被暂停，直到代码结束运行。

因为 Node.js 大部分是单线程的，所以我们会遇到大正则表达式检查的问题。

具有某些属性的正则表达式，比如具有重复的分组，会比其他的产生更多的问题。

因此，如果我们有一个电子邮件正则表达式，如:

```
/^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$/
```

，那么 Node.js 根据这个正则表达式检查字符串的速度会很慢。

![](img/d3037e7dbd087c632201c1ce3a4005dc.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该大部分时间使用代理来简化代码。

应尽量减少使用`this`,以减少混乱和出错的机会。

此外，我们应该在使用正则表达式之前检查它是否会停止 Node.js 程序。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到他们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**