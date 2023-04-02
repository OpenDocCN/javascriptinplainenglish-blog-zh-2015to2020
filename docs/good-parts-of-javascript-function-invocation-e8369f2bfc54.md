# JavaScript 的优点——函数调用

> 原文：<https://javascript.plainenglish.io/good-parts-of-javascript-function-invocation-e8369f2bfc54?source=collection_archive---------6----------------------->

![](img/fd98b421c33b8089a37dfb2c3531733a.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。它可以做很多事情，并且有一些领先于许多其他语言的功能。

在本文中，我们将看看 JavaScript 函数的一些好的部分。

# 函数调用

函数必须被调用来做一些事情。

它们可能会被调用，也可能不会被调用。除了我们传入的参数，还有`this`和`arguments`值。

`this`是函数驻留的对象或者构造函数的实例。

也可以是它所在的函数。

调用操作符是一对括号，参数之间用逗号分隔。

如果参数太多，多余的参数将被忽略。

这意味着函数的签名不必与传入的参数匹配。

参数值没有类型检查。这意味着我们可以传递任何东西。

因此，当我们向函数传递参数时应该小心。

# 方法调用模式

当一个函数存储在一个对象中时，这个函数就叫做方法。

当一个方法被调用时，`this`被绑定到对象上。

我们可以使用点符号或括号符号来调用方法。

例如，给定以下对象:

```
const obj = {
  value: 0,
  increment(val) {
    this.value += val;
  }
};
```

我们可以这样写:

```
obj.increment(2);
```

那么`obj.value`将增加 2，因为在`increment`方法中`this`被设置为`obj`。

在`this`的对象上下文中的方法被称为公共方法。

# 函数调用模式

JavaScript 函数可以是独立的函数。

例如，我们可以创建如下函数:

```
const add = (a, b) => {
  return a + b;
}
```

那么我们可以这样称呼它:

```
add(1, 2);
```

传统函数被绑定到全局对象，所以顶层传统函数的`this`在浏览器中是`window`。

然而，箭头函数没有绑定到`this`，所以它将是顶层的`undefined`。

因此，我们应该尽可能多地使用箭头函数，以便`this`的值与外部的值相同。

所以我们可以写:

```
const obj = {
  val: 0,
  foo() {
    const helper = () => {
      this.val = 1;
    }
  }
}
```

在嵌套函数中访问`this.val`。

`this`仍在`helper`箭头功能内`obj`。

如果我们用一个传统来定义`helper`，我们可以写成:

```
const obj = {
  val: 0,
  foo() {
    const that = this;
    const helper = function() {
      that.val = 1;
    }
  }
}
```

我们必须将`this`分配给`that`，这样`this`将成为`obj`而不是`helper`。

![](img/f11c59e2cd9df81bd6d23c247f5f7164.png)

Photo by [Andrés Yves](https://unsplash.com/@mygmag?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 构造函数调用模式

JavaScript 是一种典型的继承语言。对象可以直接从其他对象继承属性。

JavaScript 中没有类。类只是语法糖。

JavaScript 的原型继承会让来自具有经典继承的语言的人感到困惑。

JavaScript 和其他面向对象语言一样有`new`操作符。

然而，它只是用来以一种特殊的方式调用一个函数。

它创建了一个指向构造函数的`prototype`成员值的隐藏链接。

前缀`new`修改了`return`语句的行为。

前缀为`new`的函数被称为构造函数。

它们保存在名字大写的变量中。

如果我们调用一个没有前缀`new`的构造函数，我们会遇到问题，因为`this`的值不是我们所期望的。

除了意外行为之外，我们没有得到任何关于此的警告，所以我们需要小心。

要定义一个构造函数，我们可以写:

```
const Foo = function(string) {
  this.status = string;
};
```

上面的代码是最基本的构造函数。它只接受一个`string`参数并将`string`设置为`this.status`。

然后，我们可以通过编写以下内容来调用它:

```
const foo = new Foo('good');
```

那么`foo.status`就是`'good'`。

要添加实例方法，我们可以编写:

```
const Foo = function(string) {
  this.status = string;
};Foo.prototype.getStatus = function() {
  return this.status;
};
```

然后我们调用`foo.getStatus()`返回`'good'`。

# 结论

JavaScript 中有多种调用函数的方法。

方法是对象中的函数。我们可以用点或括号符号来称呼它。

同样，我们可以使用`function`关键字创建一个构造函数。

要定义实例方法，我们可以将其添加到函数的`prototype`属性中。