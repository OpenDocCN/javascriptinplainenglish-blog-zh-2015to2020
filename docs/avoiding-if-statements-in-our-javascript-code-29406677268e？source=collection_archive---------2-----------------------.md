# 在我们的 JavaScript 代码中避免 If 语句

> 原文：<https://javascript.plainenglish.io/avoiding-if-statements-in-our-javascript-code-29406677268e?source=collection_archive---------2----------------------->

![](img/c48f99991a09a2c4e6862a010aec5273.png)

Photo by [Rafael Rodrigues Machado](https://unsplash.com/@rcorneto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 代码中的 If 语句可能会很麻烦。它们很啰嗦，在很多地方，我们不需要它们。

我们可以用其他类型的代码替换`if`语句，使我们的代码更短更简洁。

在本文中，我们将看看可以替换 JavaScript 代码中的`if`语句的地方。

# 三元运算符

如果我们想用一个条件返回一个东西，否则返回另一个东西，三元运算符就很有用。

例如，我们可以写:

```
const bar = foo ? 'foo' : 'bar';
```

在上面的代码中，如果`foo`是`true`，那么我们返回`'foo'`。否则，我们返回`'bar'`。

那么如果`foo`是`true`，那么`bar`就是`'foo'`。否则，`bar`就是`'bar'`。

这比以下短得多:

```
const bar = ((foo) => {
  if (foo) {
    return 'foo'
  }
  return 'bar'
})(foo);
```

取而代之。有了三元运算符，我们不必定义一个匿名函数，如果`foo`为`true`则返回`'foo'`，否则返回`'bar'`。

此外，我们不必在定义它之后马上用`foo`调用它。因此，读和写就容易多了。

然而，我们不应该滥用三元运算符，像这样嵌套它们:

```
const foo = a ? b ? c ? 'foo' : 'bar' : 'baz' : 'qux';
```

上面的代码很难阅读，因为我们必须在自己的头脑中把它们放入括号中。

这对我们的大脑来说很难，所以我们不应该有像上面这样的表达。

# 短路

我们可以使用`&&`或`||`操作符作为短路操作符。

如果第一个操作数为真，那么`&&`操作符对于在第二个操作数中运行代码很有用。如果第一个操作数是 falsy，那么`||`操作符对于在第二个操作数中运行代码很有用。

例如，我们可以如下使用它:

```
const isOnline = true;
const makeRequest = () => {
  //...
}isOnline && makeRequest();
```

在上面的代码中，如果`isOnline`是`true`，我们就调用`makeRequest`。所以，既然`isOnline`是`true`，我们就调用`makeRequest`函数。

这取代了下面的`if`语句:

```
if (isOnline) {
  makeRequest();
}
```

正如我们所见，使用`if`语句，在给定相同的`isOnline`和`makeRequest`值的情况下，只需要 3 行代码就可以做同样的事情。

使用`||`操作符，如果第一个操作数是 falsy，我们可以用它来设置默认值。

例如，我们可以编写以下内容:

```
const foo = null;
const bar = foo || 'abc';
```

在上面的代码中，我们将`foo`设置为`null`，这是 falsy。因此，在第 2 行中，`||`运算符将计算`foo`常数，也就是`null`，所以它是 falsy。

然后因为它是 falsy，它将继续计算第二个操作数，然后返回它。

因此，`bar`的值将是`'abc'`。代码:

```
const bar = foo || 'abc';
```

替换以下代码:

```
const bar = ((foo) => {
  if (!foo) {
    return 'abc'
  }
  return foo;
})(foo)
```

正如我们所看到的，我们必须定义一个匿名函数，然后调用它。

该函数以`foo`为参数，然后在函数内部，我们检查`foo`是否为 falsy。如果是 falsy，那么我们返回`'abc'`。否则，我们返回`foo`。

`||`操作符的替代选项更长更复杂。

![](img/17e18a731d9e58e8a38b99562e7cf9e3.png)

Photo by [Warren Coetzer](https://unsplash.com/@wxco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 避免使用 Switch 语句

`switch`语句的许多用法可以用更简单的东西来代替。例如，代替编写下面的`switch`语句:

```
const getDescription = (breed) => {
  switch (breed) {
    case 'border':
      return 'kind dog';
    case 'pitbull':
      return 'angry dog';
    case 'german':
      return 'smart dog';
    default:
      return ''
  }
}const desc = getDescription('border');
```

我们可以将值放入对象中，如下所示:

```
const descs = {
  'border': 'kind dog',
  'pitbull': 'angry dog',
  'german': 'smart dog',
}const desc = descs['border'];
```

在上面的代码中，我们将事例值作为属性，将返回值作为属性的值。

然后，我们可以用与上一行相同的方式获取值，而不是调用冗长的`switch`语句。

如果我们想得到默认情况，我们可以如下使用`||`:

```
const desc = descs['foo'] || '';
```

现在我们将一个冗长的`switch`语句压缩成一个小对象。

# 结论

在许多情况下，我们可以将`if`和`switch`语句简化成更短的代码。我们应该记得使用类似三元运算符、短路运算的快捷方式，并将`switch`语句改为字典对象，以使我们的代码更短。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****