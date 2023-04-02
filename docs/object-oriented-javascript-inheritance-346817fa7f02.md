# 面向对象的 JavaScript —继承

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-inheritance-346817fa7f02?source=collection_archive---------9----------------------->

![](img/5d3b0a0ad939b7e00c06cd557a981c9f.png)

Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看继承。

# 原型链接

一个对象有一个原型链。

从它们继承属性。

例如，我们可以通过编写以下代码来创建构造函数链:

```
function Shape() {}
Shape.prototype.name = 'Shape';
Shape.prototype.toString = function() {
  return this.name;
};function Square() {}
Square.prototype = new Shape();
Square.prototype.constructor = Square;
Square.prototype.name = 'Square';
```

我们有带有一些原型属性的`Shape`构造函数。

然后我们创建了一个`Square`构造函数，将`prototype`设置为`Shape`构造函数。

然后我们将`constructor`设置为`Square`，如果我们将`instanceof`与`Square`实例一起使用，那么`instanceof sqaure`将返回`true`。

如果我们想在子构造函数中调用父构造函数来填充它的属性，我们可以修改代码。

我们可以写:

```
function Shape(name) {
  this.name = name;
}
Shape.prototype.toString = function() {
  return this.name;
};function Square(name, length) {
  Shape.call(this, name);
  this.length = length
}
Square.prototype = Object.create(Shape.prototype);
Square.prototype.constructor = Square;
```

用`name`属性创建`Shape`构造函数。

然后我们有了带有`name`和`length`参数的`Square`属性。

我们用`call`调用`Shape`构造函数，我们将第一个参数设置为`this`，所以用`Square`构造函数作为`this`调用它。

`name`是我们传递给`Shape`构造函数的内容。

为了创建原型，我们调用`Object.create`来继承`Shape.prototype`的属性。

我们以同样的方式设置`constructor`，以便`instanceof`仍然能够正确报告。

# 类别语法

我们可以用类语法来简化这个过程。

有了它，我们可以重写:

```
function Shape(name) {
  this.name = name;
}
Shape.prototype.toString = function() {
  return this.name;
};function Square(name, length) {
  Shape.call(this, name);
  this.length = length
}
Square.prototype = Object.create(Shape.prototype);
Square.prototype.constructor = Square;
```

收件人:

```
class Shape {
  constructor(name) {
    this.name = name;
  } toString() {
    return this.name;
  };
}class Square extends Shape {
  constructor(name, length) {
    super(name);
    this.length = length
  }
}
```

我们使用`extends`关键字从`Square`类中的`Shape`属性继承属性。

`super(name)`同`Shape.call(this, name);`。

# 原型

我们可以用`__proto__`属性得到链中的原型。

例如，我们可以写:

```
console.log(square.__proto__);
```

然后我们得到了`Shape`构造函数。

我们可以得到`__proto__`属性的`__proto__`，所以我们可以得到:

```
console.log(square.__proto__.__proto__);
```

然后我们得到了`Shape.prototype`对象。

如果我们再次调用它:

```
console.log(square.__proto__.__proto__.__proto___);
```

我们得到了`Object.prototype`对象。

我们可以通过写信来证实这一点:

```
console.log(square.__proto__ === Square.prototype);
console.log(square.__proto__.__proto__ === Shape.prototype);
console.log(square.__proto__.__proto__.__proto__  === Object.prototype);
```

然后他们都登录`true`。

# 对象从对象继承

对象可以直接从其他对象继承。

为此，我们使用`Object.create`方法返回一个对象，该对象返回一个具有给定原型对象的对象。

例如，我们可以写:

```
const proto = {
  foo: 1
};
const obj = Object.create(proto);
```

然后我们可以用`__proto__`属性检查`obj`的原型:

```
console.log(obj.__proto__ === proto);
```

然后那个日志`true`。

![](img/752d7d1d663d942e68ec631eae64a111.png)

Photo by [Hush Naidoo](https://unsplash.com/@hush52?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以创建从其他对象继承的对象。

此外，我们可以检查对象的原型链，看看从。

然后，类语法使构造函数的继承变得更加容易。

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**