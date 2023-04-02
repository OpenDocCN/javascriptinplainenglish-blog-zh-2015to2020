# JavaScript 面试问题—对象

> 原文：<https://javascript.plainenglish.io/javascript-interview-questions-objects-e380395dffff?source=collection_archive---------4----------------------->

![](img/6339ecea0fe427702384217ce8ca8ef1.png)

Photo by [Boris Smokrovic](https://unsplash.com/@borisworkshop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了得到一份前端开发人员的工作，我们需要搞定编码面试。

在这篇文章中，我们将看一些宾语问题。

# 如何检查某个属性是否存在于对象中？

有几种方法可以检查对象中是否存在属性。

首先，我们可以使用`in`操作符。例如，我们可以如下使用它:

```
const foo = { a: 1 };
console.log('a' in foo);
```

`in`操作符检查给定名称的属性是否存在于对象本身或原型链中的原型中。

上面的代码应该返回`true`，因为`a`是`foo`的属性。

`console.log(‘toString’ in foo);`也应该记录`true`，因为`toString`在`Object`的原型中，而`foo`继承了它。

我们也可以使用`Object.prototype.hasOwnProperty`方法。例如，我们可以如下使用它:

```
const foo = { a: 1 };
console.log(foo.hasOwnProperty('a'));
```

上面的代码使用了`foo`原型中的`hasOwnProperty`方法来检查`a`是否存在于`foo`及其自身属性中，也就是说它存在于`foo`本身而不是其原型中。

因为`a`是`foo`自己的财产，所以`console.log`记录`true`。

最后，我们可以使用括号符号进行检查，如下所示:

```
const foo = {
  a: 1
};
console.log(foo['a']);
```

如果它返回的不是`undefined`的值，那么我们知道我们把它作为一个属性添加了。

既然我们的例子就是这样，它应该返回`true`。

# `Object.seal`和`Object.freeze`的方法有什么区别？

在对象上调用`Object.seal`之后，我们停止向对象添加属性。

它还使所有现有属性不可配置，这意味着属性描述符被阻止更改。

在对象上调用了`delete`操作符之后，现有的属性也不能被删除。

对象的属性`__proto__`是对象的原型，也是密封的。

例如，如果我们有:

```
const foo = {
  a: 1
};
Object.seal(foo);
delete foo.a
```

跑完最后一行我们还会看到`foo.a`。

如果我们在严格模式下，我们会得到一个错误。

`Object.freeze`使对象不可变。不能以任何方式更改现有属性，包括每个属性的值。

它也做`Object.seal`所做的一切。

# 对象中的`in`操作符和`hasOwnProperty`方法有什么区别？

`in`操作符检查一个属性是否在对象本身中，以及它是否在原型链上的原型中。

另一方面，`hasOwnProperty`只检查一个对象是否在它所调用的对象内部，而不检查它的任何原型。

# 为什么`typeof null`会返回`object`？

`null`拥有类型`object`,因为这是它在早期版本的 JavaScript 中的行为方式。它只是保持这种方式，以防止破坏现有的代码库。

![](img/53d6b141b4e9c74a71ff1c21458ea379.png)

Photo by [Roi Dimor](https://unsplash.com/@roi_dimor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如何检查一个值是否为`null`？

我们应该使用严格的等式运算符来检查`null`，如下所示:

```
foo === null
```

# `new`关键字是做什么的？

`new`关键字用于从构造函数或类创建一个对象。

例如，如果我们有一个`Person`类:

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
};
```

然后，我们可以通过编写以下内容来创建它的新实例:

```
const person = new Person("Jane", "Smith");
```

`new`做几件事:

*   它创建一个空对象
*   将空对象分配给`this`值
*   该函数继承自构造函数的`prototype`属性。所以`Person`继承了`Person.prototype`。
*   如果函数中没有`return`语句，那么它将返回`this`。

请注意，ES2015 或更高版本中的类语法只是构造函数的语法糖。它做同样的事情，但看起来像一个类。

# 结论

我们可以用`in`操作符、`hasOwnProperty`或括号符号来检查对象中是否存在属性。

`Object.seal`防止属性描述符改变和属性被删除。

使一个对象不可变。

`null`属于`object`类型，而不是拥有自己的类型。

关键字从构造函数中创建一个新的对象并返回它。

【JavaScript 用简单的英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)