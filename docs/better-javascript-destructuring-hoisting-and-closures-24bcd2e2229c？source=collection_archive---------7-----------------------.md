# 更好的 JavaScript——析构、提升和闭包

> 原文：<https://javascript.plainenglish.io/better-javascript-destructuring-hoisting-and-closures-24bcd2e2229c?source=collection_archive---------7----------------------->

![](img/a6a0a74fae49d5e5a18bb7ee23a70e9c.png)

Photo by [Frame Harirak](https://unsplash.com/@framemily?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 使用析构将属性提取到变量中

如果我们想提取变量的属性，我们可以使用析构赋值语法。

例如，我们可以写:

```
function foo(x, y) {
  const {
    min,
    round,
    sqrt
  } = Math;
  return min(round(x), sqrt(y));
}
```

从`Math`对象中获取`min`、`round`和`sqrt`方法。

然后我们可以通过使用变量来调用方法，而不是重复引用`Math`。

# 熟悉闭包

闭包是我们可能不熟悉的东西。

当我们第一次和他们一起工作时，可能看起来有点吓人。

然而，其实很简单。

闭包只是函数内部的函数。

内部函数可以引用其父范围内的变量。

例如，我们可以写:

```
function makePie() {
  const ingredient = "peanut butter"; function make(filling) {
    return `${ingredient} and ${filling}`;
  }
  return make("jelly");
}
```

我们有一个`make`函数，它接受`filling`参数，并在将其与来自其父函数的`ingredient`组合后返回一些内容。

这意味着我们可以在函数中保存私有变量，然后从内部函数访问它们。

此外，我们可以在 JavaScript 中将函数作为变量返回。

例如，我们可以写:

```
function makePie() {
  const ingredient = "peanut butter"; function make(filling) {
    return `${ingredient} and ${filling}`;
  }
  return make;
}
```

并返回`make`函数。

然后我们可以用它来创建一个新的函数，通过编写“

```
const f = makePie();
f('banana cream');
```

然后我们从返回的`f`函数调用中得到`'peanut butter and banana cream’`。

我们可以通过编写以下内容来进一步缩短`makePie`函数:

```
function makePie() {
  const ingredient = "peanut butter"; return function(filling) {
    return `${ingredient} and ${filling}`;
  }
}
```

我们返回一个匿名函数。

JavaScript 函数不一定要有名字。

闭包可以更新外部变量的值。

例如，我们可以写:

```
function foo() {
  let val;
  return {
    set(newVal) {
      val = newVal;
    },
    get() {
      return val;
    },
    type() {
      return typeof val;
    }
  };
}
```

我们创建了`foo`函数，并用`set`、`get`和`type`方法返回了一个对象。

`set`设置`val`的值。

`get`返回`val`的值。

`type`返回`val`的数据类型。

`val`在对象之外，但是对象可以访问它，因为它在父对象的范围内。

这些闭包共享对`val`的访问。

# 可变提升

变量提升是在定义变量之前可以访问变量声明的地方。

这只适用于用`var`声明的变量。

`let`和`const`变量是块范围的，不会被提升。

例如，我们可以通过编写以下内容来查看提升的变量:

```
console.log(foo);
var foo = 1;
```

`foo`是`undefined`但是可以访问。

为了避免这种情况，我们应该只使用`let`和`const`变量。

![](img/0659f4d21296d34e71f4142d6712b318.png)

Photo by [Mark Kamalov](https://unsplash.com/@ruqqqes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该使用析构语法从对象中提取属性。

闭包是我们有时会用到的东西。

它们是函数中的函数。

可变提升是我们可能会遇到的事情，但它越来越不常见。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**