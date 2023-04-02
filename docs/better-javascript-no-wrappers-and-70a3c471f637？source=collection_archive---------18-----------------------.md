# JavaScript 中避免使用的 2 个构造

> 原文：<https://javascript.plainenglish.io/better-javascript-no-wrappers-and-70a3c471f637?source=collection_archive---------18----------------------->

## 更好的 JavaScript——没有包装器并且==

![](img/4d9b1045e8617def0bd859acd1efbdb0.png)

Photo by [Sara Dubler](https://unsplash.com/@ahungryblonde_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 避免原始包装

我们应该避免原始的包装器，这样我们就不必用它们来处理问题。

所以我们不应该有这样的代码:

```
const s1 = new String("foo");
const s2 = new String("foo");
```

那么如果我们有:

```
s1 === s2;
```

然后它返回`false`，因为它们是不同的对象。

它们只适用于隐式调用方法。

所以如果我们有:

```
"foo".toUpperCase();
```

然后字符串被转换成一个字符串包装器对象，然后在其上调用`toUpperCase`方法。

因为原语可以用包装器自动包装，所以我们可以给它们添加属性。

例如，我们可以写:

```
"foo".bar = 200;
```

但是如果我们写:

```
"foo".bar
```

我们得到`undefined`。

每次添加属性时都会进行包装。

一旦这样做了，那么包装的对象立即被丢弃。

所以我们永远看不到我们设定的值。

这是 JavaScript 隐藏类型错误的另一个地方。

# 使用== or！=混合类型

如果我们想要比较不同类型的操作数，那么我们必须小心操作数将被转换。

例如，如果我们有:

```
const today = new Date();
if (form.month.value == (today.getMonth() + 1) &&
  form.day.value == today.getDate()) {
  // ...
}
```

然后，操作数的类型将被转换为数字并进行比较。

很容易错过数据强制。

因此，最好通过编写以下代码来明确地进行转换:

```
const today = new Date();
if (+form.month.value == (today.getMonth() + 1) &&
  +form.day.value == today.getDate()) {
  // ...
}
```

现在我们知道双方都有数字。

更好的是，我们应该使用`===`而不是`==`来进行等式比较，这样两个不同类型的东西都是`false`。

所以我们写道:

```
const today = new Date();
if (+form.month.value === (today.getMonth() + 1) &&
  +form.day.value === today.getDate()) {
  // ...
}
```

那么我们知道只有两个数字可以进行比较。

当两个参数的类型相同时，那么`==`和`===`就没有区别。

但是如果用于比较的操作数是不同的类型，我们就会遇到问题。

类型强制规则并不明显。

它们也很复杂。

计算机无法读取我们的思想，因此`==`很可能会使用错误的假设。

对于`==`操作符有几个规则。

如果第一个操作数是`null`，第二个是`undefined`，那么就没有强制。

如果第一个操作数是`null`或`undefined`，第二个操作数是除了`null`或`undefined`之外的任何值，那么就没有强制。

如果第一个操作数是一个原始字符串、数字或布尔值，而第二个操作数是`Date`对象，那么原始数据被转换成一个数字。

通过首先尝试`toString`，然后再尝试`valueOf`，将`Date`对象转换为图元。

如果第一个操作数是字符串、数字或布尔值，而第二个操作数是非`Date`对象，则原语被转换为数字。

通过首先尝试`toString`，然后再尝试`valueOf`，将非`Date`对象转换为原语。

如果两个操作数都是基本的字符串、数字或布尔值，那么它们都被转换为数字。

我们只需使用`===`并忘记所有这些规则。

![](img/f168e0c29940a5f1bc3078bcfd38f0c0.png)

Photo by [Edgar Castrejon](https://unsplash.com/@edgarraw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`==`或者`!=`没有太多好处，问题很多。

`===`或`!==`更好。

原始包装器也应该避免。

![](img/787be6c671be8d345dc786dad8729ce5.png)

Enjoyed this article? If so, get more similar content by [**subscribing to Decoded, our YouTube channel**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)**!**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**