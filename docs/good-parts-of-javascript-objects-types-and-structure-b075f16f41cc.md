# JavaScript 的优点——对象类型和结构

> 原文：<https://javascript.plainenglish.io/good-parts-of-javascript-objects-types-and-structure-b075f16f41cc?source=collection_archive---------5----------------------->

![](img/905544962122c9c1e6b9714bf7be9312.png)

Photo by [fran hogan](https://unsplash.com/@franagain?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。它可以做很多事情，并且有一些领先于许多其他语言的功能。

在本文中，我们将研究 JavaScript 对象的一些优秀部分，包括类型和结构。

# 获取属性值的类型

我们可以使用`typeof`操作符来获取属性的值。

例如，给定以下对象:

```
const obj = {
  'foo': 1,
  'bar': {
    'baz': 2
  }
}
```

我们可以这样写:

```
typeof obj.foo
```

那么我们应该得到`'number'`。

然而，我们要小心使用这个操作符，因为原型链上的属性也可以返回值。

例如，我们可以写:

```
typeof obj.toString
```

我们会看到`'function'`回归。

因此，如果我们不想意外获得对象原型的类型，我们应该首先使用`hasOwnProperty`方法来检查属性是否在对象中。

例如，我们可以写:

```
obj.hasOwnProperty('foo')
```

检查`'foo'`是否是`obj`本身的属性。

然后返回`true`，因为它是在`obj`本身中定义的。

另一方面，如果我们写:

```
obj.hasOwnProperty('toString')
```

那么我们应该让`false`返回，因为它是`obj`原型的一部分。

# 列举

JavaScript 中有几种枚举对象属性的方法。

我们可以使用 for-in 循环来遍历对象的属性。

然而，有两个条件。枚举的顺序不能保证。此外，for-in 循环遍历原型链上所有对象原型的所有属性。

例如，我们可以写:

```
for (const key in obj) {
  //..
}
```

循环通过`obj`的按键及其原型按键。

为了防止我们遍历一个对象的原型属性，我们可以再次使用`hasOwnProperty`方法来检查该属性是否是自己的属性。

例如，我们可以写:

```
for (const key in obj) {
  if (obj.hasOwnProperty(key)) {
    //..
  }
}
```

# 对象.键

如果我们只想遍历一个对象自己的键，我们可以使用`Object.keys`方法来获取它们。

它返回一个对象自身可枚举属性的键数组。

只返回字符串键。

例如，我们可以写:

```
for (const key of Object.keys(obj)) {
  //...
}
```

我们使用 for-of 而不是 for-in 循环来遍历`obj`自己的键。

# Reflect.ownKeys

`Reflect.ownKeys`跟`Object.keys`很像。它返回给定对象自己的键的数组。

不同之处在于字符串和符号键都被返回。

例如，我们可以写:

```
for (const key of Reflect.ownKeys(obj)) {
  //...
}
```

# 删除

`delete`操作符用于从对象中删除属性。

如果一个对象有给定的属性，那么如果我们使用`delete`操作符，它将被移除。

例如，给定以下对象:

```
const obj = {
  'foo': 1,
  'bar': {
    'baz': 2
  }
}
```

我们可以使用:

```
delete obj.foo;
```

从`obj`中移除`foo`属性。

![](img/089bf448199ef3b80e7328552b32b019.png)

Photo by [Levi XU](https://unsplash.com/@xusanfeng?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 全球减排

JavaScript 的一个不好的地方是定义全局变量太容易了。

这使得我们的程序在名称空间冲突方面不够健壮，并使错误难以跟踪。

我们可以用几种方法来最小化全局变量的使用。

我们可以在对象中取出我们的本地代码。我们可以把它们放在块中，或者放在模块中。

如果我们创建一个对象:

```
const obj = {
  'foo': 1,
  'bar': {
    'baz': 2
  }
}
```

那么`obj.foo`不是全局对象。

我们也不应该在节点应用程序中附加任何类似于`window`或`global`的属性。

同样，如果我们把它们放在块中，那么我们可以使用`let`和`const`来声明块范围的变量和常量。

它们不会在全球范围内提供。

模块中的项目也不是全局可用的。

# 结论

我们可以通过使用`typeof`操作符来获取属性值的类型。

可以使用 for-in、for-of、`Object.keys`或`Reflect.ownKeys`方法进行枚举。

`delete`操作符可用于删除对象的属性。

为了减少全局变量的使用，我们可以将变量放在块、模块或对象中。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**