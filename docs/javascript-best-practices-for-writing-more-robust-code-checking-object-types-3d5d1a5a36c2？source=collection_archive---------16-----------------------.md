# 编写更健壮代码的 JavaScript 最佳实践—检查对象类型

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-for-writing-more-robust-code-checking-object-types-3d5d1a5a36c2?source=collection_archive---------16----------------------->

![](img/9530ddfbbe3e58a26aca013a457499fb.png)

Photo by [Paweł Czerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将了解如何检查对象的类型，以便我们只处理我们正在寻找的对象类型。

# 运算符的实例

JavaScript `instanceof`操作符让我们获得用于创建对象的构造函数。

`instanceof`操作符的语法如下:

```
x instanceof Ctor
```

其中`x`是我们正在检查的对象，`Ctor`是我们想要检查它是否是从它创建的构造函数。

例如，我们可以如下使用它:

```
function C() {}
const c = new C();
console.log(c instanceof C);
```

上面的代码有一个新的构造函数`C`，我们通过用`new`操作符调用`C`构造函数创建了`c`对象。

因此，当我们记录`c instanceof C`时，我们应该会看到`true`，因为`c`是从`C`构造函数创建的。

注意，大多数对象也是`Object`的实例，除非我们专门创建了一个没有原型的对象。

因此，`console.log(c instanceof Object);`也应该是`true`，因为我们在创建`c`时并没有特意移除原型。

`instanceof`操作符沿着原型链一直向上检查，所以如果它在原型链的任何地方找到构造函数，它将返回`true`。

由于`Object`是`C`的原型，`c`是`C`的实例，`instanceof`返回`true`。

当我们想检查某个东西，而不是某个东西的实例时，我们必须小心。

如果我们想检查`c`是否不是`C`的实例，我们应该写:

```
if (!(c instanceof C)) {
  //...
}
```

而不是:

```
if (!c instanceof C) {
  //...
}
```

这是因为在第二个例子中,`!`操作符只应用于`c`,而不是`c instanceof C`。因此，我们得到了`false instanceof C`而不是`!(c instanceof C)`，因为`c`是真的，我们否定了它。

# 构造函数名称检查

`instanceof`的另一种方法是用对象的`constructor.name`属性检查创建对象的构造函数的名称。

例如，如果我们有:

```
function C() {}
const c = new C();
console.log(c.constructor.name);
```

那么`c.constructor.name`就是`'C'`，因为我们从`C`构造函数创建了`c`。

这只返回创建它的构造函数的名字，所以我们不会得到原型链上的任何信息。

# 结构检查

为了检查对象是否是我们想要处理的，我们还应该检查给定的属性和值是否存在。

我们可以用对象实例的`hasOwnProperty`方法来完成，或者我们可以检查属性的类型是否是`'undefined'`。

它只检查属性是否是自己的属性，这意味着它不是从原型链上的对象继承的属性。

例如，我们可以编写以下代码来检查:

```
const obj = {};
obj.foo = 42;
console.log(obj.hasOwnProperty('foo'));
```

在上面的代码中，我们有`obj.hasOwnProperty(‘foo’)`。由于`obj`具有属性`foo`，因此`obj.hasOwnProperty(‘foo’)`返回`true`。

`hasOwnProperty`将返回`true`，即使属性值为`null`或`undefined`。只要它被添加到对象中，它就会返回`true`。

同样，我们可以使用 JavaScript `in`操作符来检查一个属性是否在一个对象中。

属性用于检查给定对象中是否包含继承的或自己的属性。

例如，如果我们有:

```
const obj = {};
obj.foo = 42;
console.log('foo' in obj);
console.log('constructor' in obj);
```

然后两个控制台日志都显示`true`，因为`'foo'`是`obj`自己的属性，而`'constructor'`是从`Object`继承的属性，而`Object`是`obj`的原型。

和`hasOwnProperty`一样，如果属性值为`null`或`undefined`，那么`in`操作符也会返回`true`。

如果我们想检查一个属性是否不是`null`或`undefined`，我们可以写如下:

```
const obj = {};
obj.foo = 42;
console.log(typeof obj.foo !== 'undefined' && obj.foo !== null);
```

在上面的代码中，我们有:

```
typeof obj.foo !== 'undefined' && obj.foo !== null
```

用`typoef`操作器检查`obj.foo`是否与`undefined`不在一起，用`!==`操作器检查`obj.foo`是否与`null`不在一起。

![](img/a8ca08e0dc4d641258ed25c001cf45f1.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过使用`instanceof`操作符或使用对象的`constructor.name`属性来检查对象是否是构造函数的实例。

此外，我们必须检查一个属性是否是我们正在寻找的，这样我们就知道我们正在处理正确的对象。为此，我们可以使用`hasOwnProperty`、`in`或`typeof`操作符。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English****—谢谢，继续学习！**](https://medium.com/python-in-plain-english)

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的中型用户名和您感兴趣的内容，我们将会回复您！****