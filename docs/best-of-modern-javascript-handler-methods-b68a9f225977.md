# 现代 JavaScript 的最佳处理程序方法

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-handler-methods-b68a9f225977?source=collection_archive---------8----------------------->

![](img/637d27e1fabae96bdb6718e8cb1b9365.png)

Photo by [Fezbot2000](https://unsplash.com/@fezbot2000?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究如何控制 JavaScript 对象操作。

# 使对象不可配置

我们可以使对象不可配置。

这意味着我们不能改变对象的属性描述符。

属性描述符包括属性值、属性是否可写或是否可配置。

可配置意味着能够更改对象的属性描述。

要禁用可配置性，我们可以写:

```
const obj = {};
Object.defineProperty(obj, 'foo', {
  value: 'bar',
  writable: false,
  configurable: false
});
```

我们用`Object.defineProperty`方法定义了`obj.foo`属性。

第三个参数具有属性描述符。

`configurable`设置为`false`让我们禁止改变属性描述符。

我们还将`writable`设置为`false`，这样我们就不能改变`foo`属性的值。

# `enumerate`陷阱

ES6 代理最初打算包括`enumerate`陷阱，让我们改变 for-in 循环的行为。

但它被简化为代理。

# 代理 API

有两种方法来创建代理。

一种方法是使用`Proxy`构造函数。

我们可以写:

```
const proxy = new Proxy(target, handler)
```

`Proxy`构造函数接受一个`target`对象和一个`handler`对象来让 trap 对象操作。

另一种方法是使用`Proxy.revocable`工厂功能。

我们可以写:

```
const {
  proxy,
  revoke
} = Proxy.revocable(target, handler)
```

去使用它。

不同之处在于有一个`revoke`函数让我们撤销对代理的访问。

# 处理程序方法

`handler`对象可以有各种方法让我们控制各种对象操作。

`defineProperty(target, propKey, propDesc)`让我们控制如何用`Object.defineProperty`定义属性。

`target`是代理控制的目标对象。

`propKey`有属性键。

`propDesc`是带有属性描述符的对象。

它返回一个布尔值。

`deleteProperty(target, propKey)`让我们控制如何定义属性。

它们具有与`defineProperty`相同的前两个参数。

它让我们控制`delete`操作符的工作方式。

让我们控制如何检索属性。

`receiver`是代理对象。

它返回我们想要返回的属性值。

`getOwnPropertyDescriptor(target, propKey)`让我们控制属性描述符的返回方式。

我们可以用它来回报我们的方式。

它控制着`Object.getOwnPropertyDescriptor(proxy, propKey)`如何行动。

`getPrototypeOf(target)`返回`target`对象的原型。

它控制`Object.getPrototypeOf(proxy)`如何动作。

`has(target, propKey)`让我们控制`in`操作符的行为。

它应该返回一个布尔值。

`isExtensible(target)`让我们修改`Object.isExtensible(proxy)`方法的行为。

它应该返回一个布尔值。

`ownKeys(target)`可以改变几种方法的行为。

它返回一个属性键数组。

它控制`Object.getOwnPropertyPropertyNames(proxy)`返回字符串键的行为。

它还可以控制`Object.getOwnPropertyPropertySymbols(proxy)`返回符号键的行为。

并且，它可以控制`Object.keys(proxy)`方法的行为。

`preventExtensions(target)`让我们改变`Object.preventExtensions(proxy)`的行为。

它返回布尔值。

`set(target, propKey, value, receiver)`控制如何设置对象属性。

它还返回一个布尔值。

`setPrototypeOf(target, proto)`让我们改变`Object.setPrototypeOf(proxy, proto)`方法的行为。

![](img/1df4c13918dac160ec1732377d71a754.png)

Photo by [Joyce Romero](https://unsplash.com/@joyceromero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用代理处理器方法控制许多对象操作。

`Object`静态方法也可以用代理处理程序方法来控制。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**