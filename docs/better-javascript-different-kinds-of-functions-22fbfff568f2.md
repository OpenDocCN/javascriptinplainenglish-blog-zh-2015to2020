# 更好的 JavaScript——不同种类的函数

> 原文：<https://javascript.plainenglish.io/better-javascript-different-kinds-of-functions-22fbfff568f2?source=collection_archive---------6----------------------->

![](img/cb282db95ad0a9c5a703a2a04f0f5750.png)

Photo by [Dan Meyers](https://unsplash.com/@dmey503?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 了解函数、方法和构造函数调用之间的区别

我们应该理解函数、方法和构造函数之间的区别。

这些都是功能，但它们服务于不同的目的。

函数是一些我们可以调用的可重用代码。

他们可能接受输入并返回一些东西。

例如，函数是这样的:

```
function hello(username) {
  return `hi ${username}`;
}
```

那么我们可以这样称呼它:

```
hello('james');
```

JavaScript 中的方法是对象中的函数。

例如，我们可以写:

```
const obj = {
  hello() {
    return `hi ${this.username}`;
  },
  username: 'jane smith'
};
```

我们有`obj.hello`方法。

`this`是`obj`，因为方法在`obj`对象中。

那么我们可以这样称呼它:

```
obj.hello();
```

我们把`'hi jane smith'`还回来了。

我们可以在对象外部定义一个函数，然后将其作为方法添加。

例如，我们可以写:

```
const hello = function() {
  return `hi ${this.username}`;
}const obj = {
  hello,
  username: 'jane smith'
};
```

然后`obj.hello()`返回相同的结果。

我们可以重用`hello`在另一个函数中定义一个方法。

例如，我们可以写:

```
const hello = function() {
  return `hi ${this.username}`;
}const obj = {
  hello,
  username: 'jane smith'
};const obj2 = {
  hello,
  username: 'may wong'
};
```

然后当我们打电话时:

```
obj2.hello()
```

它返回`‘hi may wong’`。

带有`this`的函数在对象之外没有用。

例如，如果我们有:

```
const hello = function() {
  return `hi ${this.username}`;
}console.log(hello())
```

那么如果我们直接调用`hello`，我们就会得到`'hi undefined'`，因为如果严格模式关闭，那么`this`就是`window`。

如果我们打开了严格模式:

```
const hello = function() {
  'use strict';
  return `hi ${this.username}`;
}
```

然后我们得到不同的东西。

我们得到“未捕获的类型错误:无法读取未定义的属性‘username ’”,因为在严格模式打开的情况下，`this`位于顶级`undefined`。

JavaScript 函数的另一个用途是构造函数。

它们让我们创建给定类型的对象。

例如，我们可以写:

```
function Person(name) {
  this.name = name;  
}
```

然后我们可以用`new`操作符创建一个新的`Person`实例。

例如，我们可以写:

```
const james = new Person('james');
```

那么`james.name`就是`'james'`。

# 高阶函数

高阶函数是将函数作为参数或返回函数的函数。

例如，数组实例的`sort`方法是一个高阶函数。

我们可以通过书写来使用它:

```
const sorted = [3, 1, 2, 5, 5, 9].sort((a, b) => a - b);
```

那么`sorted`就是`[1, 2, 3, 5, 5, 9]`。

我们传入了对`sort`方法的回调，以便它用于对数组中的数字进行排序。

`a`和`b`是正在处理的数组中的条目。

![](img/f60750062013414e2bab402b12d2f571.png)

Photo by [Randy Fath](https://unsplash.com/@randyfath?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

不同种类的功能做不同的事情。

高阶函数是将函数作为参数或返回函数的函数。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**