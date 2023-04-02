# 有用的 JavaScript 技巧—电子邮件和异常

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-emails-and-exceptions-89a7d3c3fa4a?source=collection_archive---------5----------------------->

![](img/a0b6bf29038160c0dcfca3bd39b63d21.png)

Photo by [Louis Renaudineau](https://unsplash.com/@louisrdn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 验证电子邮件

我们可以用一个简单的正则表达式来验证电子邮件。

例如，我们可以写:

```
const expression = /\S+@\S+/
expression.test('abc@example.com'.toLowerCase())
```

`\S`代表任何非空白字符。

`+`至少代表其前面的一种字符类型。

`@`是&符号。

这是一个简单的正则表达式，可以让我们检查大多数电子邮件地址。

我们可以毫无困难地使用这个。

# 引用

JavaScript 允许 3 种引用。

我们可以写单引号、双引号和反引号。

单引号和双引号是一样的。

反勾号用于模板字符串。

# JavaScript 类

我们可以创建类，这是构造函数的语法糖。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this.name = name
  } greet() {
    return `hi, ${this.name}`;
  }
}
```

我们有`Person`构造函数。

它有`constructor`方法来初始化`Person`实例。

`greet`是我们可以参考`this`的方法。

`this`是`Person`实例。

所以我们可以写:

```
const joe= new Person('joe');
console.log(joe.greet());
```

然后我们让`'hi, joe'`登录到控制台。

# 类继承

我们可以使用`extends`关键字来继承一个父类。

例如，我们可以写:

```
class Student extends Person {
  greet() {
    return `Hi, ${this.name}. I am a student.`;
  }
}
```

我们有`Student`类，它是`Person`的子类。

我们已经覆盖了`Person`中的`greet`方法。

例如，我们可以写:

```
const joe = new Person('joe');
console.log(joe.greet());
```

然后我们会看到:

```
'Hi, joe. I am a student.'
```

已登录控制台。

# 静态方法

类可以有静态方法。

我们可以写:

```
class Person { 
  static greet() {
    return 'hi';
  }
}
```

该方法由类的所有实例共享。

然后我们可以写:

```
Person.greet()
```

调用方法。

# Getters 和 Setters

我们可以在一个类中添加 getters 和 setters。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this._name = name
  } set name(value) {
    this._name = value
  } get name() {
    return this._name
  }
}
```

我们有`_name`字段，我们使用`name` setter 来设置它。

设置器用关键字`set`表示。

Getters 用关键字`get`表示。

我们可以有一个只有吸气剂的场。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this._name = name
  } get name() {
    return this._name
  }
}
```

那么我们只能得到`name`的值而不能设置它。

我们也可以有一个只有 setter 的字段。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this._name = name
  } set name(value) {
    this._name = value
  }
}
```

那么我们只能设置`name`而不能得到它的值。

Getters 和 setters 可以方便地创建从其他属性派生的属性。

# 例外

像许多编程语言一样，JavaScript 代码可以抛出异常。

它有`throw`关键字让我们抛出异常。

例如，我们可以写:

```
throw value
```

其中`value`可以是任意值。

# 处理例外

我们可以用 try-catch 块处理异常。

例如，我们可以写:

```
try {
  // code that may throw exceptions
} catch (e) {}
```

然后，我们可以捕获 try 块中抛出的任何异常。

`e`有`throw`抛出的值。

`finally`块包含不管程序流如何执行的代码。

我们可以使用`finally`而不用`catch`。

它是清理`try`块中打开的任何资源的一种方式。

`try`图块可以嵌套。

在这种情况下，会处理最近的异常。

例如，我们可以写:

```
try {
  // code try {
    // more code
  } finally {
    // more code
  }} catch (e) {}
```

![](img/e6b6c786dce2def2116ea2aa9df5a1bc.png)

Photo by [Victor Malyushev](https://unsplash.com/@malyushev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用吸气剂和设置剂来获取和设置属性。

此外，我们可以抛出和处理带有异常的错误。

JavaScript 类使继承更容易。它们相当于构造函数。

## 坦白地说

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**