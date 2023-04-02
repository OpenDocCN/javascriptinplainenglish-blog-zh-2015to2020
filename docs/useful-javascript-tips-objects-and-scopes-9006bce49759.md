# 有用的 JavaScript 技巧—对象和范围

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-objects-and-scopes-9006bce49759?source=collection_archive---------11----------------------->

![](img/3478165f885eaaa64d5d6ac75b25829c.png)

Photo by [Josephine Baran](https://unsplash.com/@jma1053?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 污染全球范围

我们不应该定义全局变量来声明全局范围。

为了避免这种情况，我们使用`let`和`const`来声明变量。

例如，我们写道:

```
ler x = 1;
```

或者:

```
const y = 2;
```

我们不应该声明前面有`var`或者没有关键字的变量。

它们都声明了全局变量，这是我们不想要的。

同样，我们不应该使用函数声明。

相反，我们应该使用函数表达式。

所以与其写:

```
function foo() {}
```

我们写道:

```
const foo = () => {}
```

我们还可以将变量放在 IIFEs 中，使它们保持私有:

```
(() => {
  let  = 1;
})()
```

现在它们在函数之外是不可访问的。

# 使用严格模式

我们应该使用严格模式来禁止代码中的一些不好的结构。

例如，我们不能将事物分配给保留的关键字，如:

```
undefined = 1;
```

此外，我们不能在没有声明全局变量的情况下，通过给变量赋值来意外地定义全局变量。

所以我们不能创建像这样的全局变量:

```
foo = 1;
```

`foo`是一个全局变量，因为我们没有用关键字来声明它。

# 严格平等

我们应该使用`===`或`!==`操作符来比较值，因为数据类型转换不会在与它们比较之前完成。

例如，不写:

```
a == b
```

我们写道:

```
a === b
```

比`==`操作员安全多了。

同样，对于不等式，我们应该使用`!==`运算符。

所以我们可以写:

```
a !== b
```

而不是:

```
a != b
```

# 使用&&或||有条件地计算表达式

如果左操作数为 falsy，我们应该使用`||`操作符返回右操作数。

例如，我们可以写:

```
const str = '' || 'foo';
```

返回`'foo'`因为空字符串是 falsy。

类似地，如果左表达式为真，我们可以使用`&&`操作符来计算右表达式。

所以我们可以写:

```
const baz = obj.foo && obj.foo.baz;
```

这样，`obj.foo.baz`只有在`obj.foo`为真时才返回。

这可以避免在访问嵌套属性时出现大量未定义的错误。

# 转换值类型

我们可以使用`+`操作符将字符串转换成数字。

例如，我们可以写:

```
const foo = +'100'
```

得到 100 作为`foo`的值。

同样，我们可以使用`!!`操作符将任何东西转换成布尔值。

例如，我们可以写:

```
!!0
```

转换成`false`因为它是假的。

# 遵循风格指南

有许多好的风格指南，我们可以遵循它们来编写好的 JavaScript 代码。

例如，我们可以遵循[谷歌 javascript 风格指南](https://google.github.io/styleguide/javascriptguide.xml)、 [airbnb/javascript](https://github.com/airbnb/javascript) 指南等，这样我们就可以在编写 JavaScript 时遵循一些好的实践。

为了自动检查坏货，我们可以使用 ESLint co check 来检查它们。

# 在 JavaScript 中创建不可变对象

我们可以用`Object.seal`方法在 JavaScript 中创建不可变的对象。

例如，我们可以写:

```
Object.seal(foo);
```

使`foo`不可变。

它使对象不可变，并且不返回任何内容。

它防止修改属性描述符。

还有防止给对象添加属性的`Object.preventExtensions`。

例如，我们可以写:

```
Object.preventExtensions(foo);
```

此外，我们可以写:

```
Object.freeze(foo);
```

然后我们可以做所有`seal`和`prevenExtensions`做的事情。

# 创建没有副作用的哈希映射

我们可以通过使用带有`null`参数的`Object.create`来创建一个没有原型的纯对象。

例如，我们可以写:

```
const map = Object.create(null);
```

那么`map`对象就不会有原型。

我们可以放心地把它当字典用。

# 克隆不可变对象

我们可以使用`clone`包来克隆一个对象。

例如，我们可以写:

```
const clone = require('clone');const a = { foo: { bar: 1 } };
const b = clone(a);
```

现在我们可以克隆`a`并使克隆值为`b`，这样我们就可以在不影响`a`的情况下操纵`b`。

![](img/e6dd2e3500535af38079bdc9b53d8ca7.png)

Photo by [Brienna Scott](https://unsplash.com/@bbhjk123?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一些`Object`方法来防止对象被修改。

同样，我们可以使用`clone`属性来克隆对象。

此外，我们可以使用`&&`或`||`来做除了创建布尔表达式之外的事情。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**