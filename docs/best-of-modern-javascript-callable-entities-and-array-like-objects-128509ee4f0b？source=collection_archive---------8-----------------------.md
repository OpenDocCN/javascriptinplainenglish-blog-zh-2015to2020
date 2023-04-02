# 现代 JavaScript 的精华——可调用的实体和类似数组的对象

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-callable-entities-and-array-like-objects-128509ee4f0b?source=collection_archive---------8----------------------->

![](img/e767f5c64b420502e7af2522a0fc75cc.png)

Photo by [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中的 spread 操作符和函数。

# 将可迭代或类似数组的对象转换为数组

我们可以将可迭代或类似数组的对象转换成数组。

我们可以用 spread 操作符来实现这一点。

例如，我们可以写:

```
const set = new Set([1, 2, 6]);
const arr = [...set];
```

那么`arr`就是 `[1, 2, 6]`。

我们可以用同样的方法将其他可迭代对象转换成数组。

例如，我们可以写:

```
const obj = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  }
};
const arr = [...obj];
```

那么`arr`就是`[1, 2, 3]`。

如果我们有一些不是可迭代的，而是类似数组的东西，我们可以用`Array.from`方法把它转换成一个数组。

例如，我们可以写:

```
const arrayLike = {
  '0': 'a',
  '1': 'b',
  '2': 'c',
  length: 3
};const arr = Array.from(arrayLike);
```

那么`arr`就是`[“a”, “b”, “c”]`。

我们不能使用 spread 运算符将`arrayLike`扩展到一个数组中。

如果我们尝试，我们会得到一个类型错误。

# ES6 中的可调用实体

ES6 或更高版本有几种类型的可调用实体。

它们是函数、方法和构造函数。

函数不是任何对象的一部分，所以我们通过编写`foo(1)`来调用它们。

方法是作为对象属性的函数。

比如我们可以通过写`obj.foo(81)`来称呼他们。

构造函数是我们可以作为构造函数实例的对象的函数。

比如我们可以写`new Constr(8)`来调用它。

`super`通话仅限于特定地点。

我们可以通过编写以下代码来调用超级方法调用:

```
super.method('abc')
```

我们可以这样调用超级构造函数:

```
super(8)
```

如果一个类是使用`extended`关键字创建的子类，我们可以这样做。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this.name = name;
  } greet() {
    console.log(`hello ${name}`)
  }
}class Employee extends Person {
  constructor(name, title) {
    super(name);
    this.title = title;
  } greet() {
    super.greet()
  }
}
```

我们调用了`super`构造函数来从子`Employee`类中调用`Person`构造函数。

我们用`super.greet()`调用了`Person`实例的`greet`方法。

# 更喜欢将箭头函数作为回调函数

回调通常不需要自己的`this`值，所以我们可以用箭头函数来定义。

它也更紧凑，所以我们打字更少。

一些 API 使用`this`作为回调的隐式参数。

这阻止了我们使用箭头函数作为回调函数。

例如，`addEventListener`可能在他们的回调中使用`this`:

```
document.addEventListener('click', function() {
  console.log(this);
})
```

`this`是`document`对象。

然而，我们可以将`this`改为`document`，这样我们就可以在回调中使用箭头函数:

```
document.addEventListener('click', () => {
  console.log(document);
})
```

# 更喜欢将函数声明作为独立函数

如果我们有独立的函数，我们应该使用函数声明，这样我们可以在任何地方使用它们。

所以我们可以写:

```
function foo(arg1, arg2) {
  // ···
}
```

它们可以在任何地方使用，它们看起来像发电机功能。

然而，对于独立的功能，我们通常不需要`this`。

如果我们不需要自己的`this`函数内部，那么我们可以将 arrow 函数设置为一个变量。

例如，我们可以写:

```
const foo = (arg1, arg2) => {
  //...
};
```

那么我们就不用担心函数内部`this`的值了。

![](img/d9ffade9e4aa51e94dc0a88597bb8c2b.png)

Photo by [Vivint Solar](https://unsplash.com/@vivintsolar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以将不可迭代的类似数组的对象转换成数组。

此外，现代 JavaScript 有各种各样的可调用实体。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**