# JavaScript 最佳实践—箭头函数和类

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-arrow-functions-and-classes-87d7563d9481?source=collection_archive---------0----------------------->

![](img/25e0a6eca3343c6a47fd01ef7e766f68.png)

Photo by [Paweł Czerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将探讨使用 arrow 函数和类的最佳方式。

# 对匿名函数使用箭头函数

匿名函数应该使用箭头函数。

这样就不用处理`this`的不同值，而且更短。

例如，不写:

```
[1, 2, 3].map(function (x) {
  return x + 1;
});
```

我们写道:

```
[1, 2, 3].map((x) => x + 1);
```

它要短得多而且`this`的值和外面的一样。

# 删除一个语句长的箭头函数的花括号

对于只有一条语句的箭头函数，我们不需要花括号。

例如，我们可以写:

```
[1, 2, 3].map((x) => x ** 2);
```

返回是隐式的，所以我们也不需要写`return`。

# 如果表达式跨越多行，请用括号将它括起来

如果我们有一个一个语句长的 arrow 函数，那么我们必须把它放在括号里，这样我们就可以返回整个表达式。

例如，我们可以写:

```
[1, 2, 3].map((x) => ({
  [x]: x
}));
```

这样，我们将每个数组条目的键和值设置为数组条目的值。

我们可以对任何长表达式这样做。

# 为清晰和一致起见，在参数两边加上括号

在参数周围添加括号使我们的箭头函数签名更清晰。

这也符合没有参数或有多个参数的箭头函数。

例如，我们写道:

```
[1, 2, 3].map((x) => x ** x);
```

而不是:

```
[1, 2, 3].map(x => x ** x);
```

# 避免将箭头函数语法与比较运算符混淆

我们应该给比较表达式加上括号，这样我们就知道它们是比较表达式。

例如，不写:

```
[1, 2, 3].map((x) => x <= 2);
```

我们写道:

```
[1, 2, 3].map((x) => (x <= 2));
```

正如我们所看到的，省略括号使得我们的箭头函数更难阅读。

# 类和构造函数

我们应该接受现代句法，而不是使用旧句法。

# 使用类语法而不是原型

使用类语法比操纵原型属性更清楚。

原型是很多人都很困惑的东西。

向构造函数添加实例方法，而不是编写:

```
function Person(name) {
  this.name = name;
}Person.prototype.getName = function() {
  return this.name;
}
```

我们写道:

```
class Person {
  constructor(name) {
    this.name = name;
  } getName() {
    return this.name;
  }
}
```

我们应该将方法放入类中，而不是将方法添加到`prototype`属性中。

实例变量应该在`constructor`方法中初始化，而不是在构造函数中。

在用给定的声明一个类之后，我们不能用相同的名字声明另一个类。

构造函数就不是这样了。

# 使用继承扩展

有了类语法，继承变得容易多了。

同样，`instanceof`也会正常工作。

例如，不写:

```
function Animal(name) {
  this.name = name;
}
Animal.prototype.getName = function() {
  return this.name;
}function Dog(name) {
  Animal.call(this, name);
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Animal;
```

我们应该写:

```
class Animal {
  constructor(name) {
    this.name = name;
  }
  getName() {
    return this.name;
  }
}class Dog extends Animal {
  constructor(name) {
    super(name);
  }
}
```

现在，当我们创建一个`Dog`实例时，我们可以使用`instanceof`来检查它是否是`Animal`的实例。

我们可以写:

```
const dog = new Dog();
console.log(dog instanceof Animal)
```

查看控制台日志显示`true`，以便正确完成继承。

构造函数语法给出了相同的结果，但是这个过程更加复杂并且容易出错。

![](img/fecaa8c31627c37b5588a6b0ef5f3fc7.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 方法可以返回`this`来帮助方法链接

在类中，我们可以有返回`this`的方法来让我们链接它们。

例如，我们可以写:

```
class Cube {
  setLength(val) {
    this.length = val;
    return this;
  } setWidth(val) {
    this.width = val;
    return this;
  } setHeight(val) {
    this.height = val;
    return this;
  }
}
```

然后我们可以通过编写以下代码来链接`Cube`实例的方法:

```
const cube = new Cube()
  .setHeight(10)
  .setWidth(20)
  .setLength(30)
```

而`cube`就是`{height: 10, width: 20, length: 30}`

# 结论

类语法和箭头函数是 JavaScript 的两个最好的特性，所以我们应该尽可能多地使用它们。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**