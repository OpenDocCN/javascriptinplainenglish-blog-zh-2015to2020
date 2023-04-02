# 面向对象的 JavaScript——构造器和快捷键

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-constructors-and-shorthands-95fdb824cddc?source=collection_archive---------7----------------------->

![](img/d86129c770ff377204551fd16ff800b7.png)

Photo by [Tim Scalzo](https://unsplash.com/@tjscalzo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看构造函数和对象简称。

# 构造函数属性

`constructor`属性是实例对象的一部分。

它包含对构造函数的引用。

所以如果我们有:

```
function Person(name, occupation) {
  this.name = name;
  this.occupation = occupation;
  this.whoAreYou = function() {
    return `${this.name} ${this.occupation}`
  };
}const jane = new Person('jane', 'writer');
```

然后:

```
jane.constructor
```

是`Person`构造函数。

# 运算符的实例

`instanceof`操作符让我们检查对象是否是用给定的构造函数创建的。

例如，我们可以写:

```
jane instanceof Person
```

然后我们得到`true`。

如果我们写下:

```
jane instanceof Object
```

那也是`true`。

# 返回对象的函数

返回对象的函数称为工厂函数。

例如，我们可以通过编写以下代码来创建工厂函数:

```
function factory(name) {
  return {
    name
  };
}
```

那么我们可以这样称呼它:

```
const james = factory('james');
```

那么我们得到`james.names`就是`'james'`。

也可以编写构造函数来返回一个对象，而不是返回一个`this`的实例。

例如，我们可以写:

```
function C() {
  this.a = 1;
  return {
    b: 2
  };
}
```

那么如果我们调用构造:

```
const c = new C()
```

然后我们得到:

```
{b: 2}
```

# 传递物体

我们可以将对象传递给函数。

例如，我们可以写:

```
const reset = function(o) {
  o.count = 0;
};
```

然后我们可以通过写来使用它:

```
const obj = {
  count: 100
}
reset(obj);
console.log(obj);
```

我们得到了:

```
{count: 0}
```

从控制台日志中。

# 比较对象

我们不能用`===`比较对象，因为如果它们有相同的引用，它只返回`true`。

例如，如果我们有:

```
const james = {
  breed: 'cat'
};
const mary = {
  breed: 'cat'
};
```

然后:

```
james === mary
```

返回`false`，即使它们具有完全相同的属性。

它返回`true`的唯一方式是我们将一个对象赋给另一个对象。

例如，我们写道:

```
const james = {
  breed: 'cat'
};
const mary = james;
```

然后:

```
james === mary
```

返回`true`。

# ES6 对象文字

ES6 为我们提供了一个更短的定义对象文字的语法。

例如，如果我们有:

```
let foo = 1
let bar = 2
let obj = {
  foo: foo,
  bar: bar
}
```

那么我们可以简化为:

```
let foo = 1
let bar = 2
let obj = {
  foo,
  bar
}
```

我们也可以简化方法。

例如，不写:

```
const obj = {
  prop: 1,
  modifier: function() {
    console.log(this.prop);
  }
}
```

我们写道:

```
const obj = {
  prop: 1,
  modifier() {
    console.log(this.prop);
  }
}
```

我们也可以使用计算属性键。

例如，我们可以写:

```
let vehicle = "car";
let car = {
  [`${vehicle}_model`]: "toyota"
}
```

我们可以对方法做同样的事情。

![](img/55f565164387cb436be91c32320f84a5.png)

Photo by [Jake Ingle](https://unsplash.com/@ingle_jake?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

对象文字可以用不同的方式编写。

我们可以用`constructor`属性和`instanceof`来检查构造函数。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)