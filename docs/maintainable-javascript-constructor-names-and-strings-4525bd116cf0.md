# 可维护的 JavaScript —构造函数名称和字符串

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-constructor-names-and-strings-4525bd116cf0?source=collection_archive---------5----------------------->

![](img/ed567ff66a12e44508ac8039421889ea.png)

Photo by [Bekky Bekks](https://unsplash.com/@bekky_bekks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将研究通过改进命名来创建可维护的 JavaScript 代码的基础。

# 构造函数和类

构造函数用于创建对象。

它们是用`new`操作符调用的函数。

类语法是构造函数的语法糖。

JavaScript 的标准库有许多内置的构造函数，如`Object`或`RegExp`。

它们都有 Pascal 大小写，这是我们用自己的构造函数应该遵循的约定。

我们为构造函数写`SomeName`，而不是`someName`。

构造函数名通常是名词，因为它们创建给定类型的对象。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
}Person.prototype.greet = function() {
  console.log(`hello ${this.name}`);
};
```

或者:

```
class Person {
  constructor(name) {
    this.name = name;
  } greet() {
    console.log(`hello ${this.name}`);
  }
}
```

`Person`从大写字母开始，这样我们可以将它们与其他实体(如变量和函数)区分开来。

遵循这一约定使得代码更容易阅读。

错误也更容易被发现。

几乎所有的 JavaScript 风格指南像 Airbnb，Google 等。所有的构造函数和类都推荐这种大小写。

# 文字值

JavaScript 代码可以有各种文字值。

它们包括字符串、数字、布尔值、`null`或`undefined`。

它也有对象和数组文字。

布尔值只能是`true`和`false`，所以我们不需要为它们设置自己的约定。

其他类型的值需要一些约定，以便它们保持一致并减少混淆。

# 用线串

字符串可以用单双反斜杠分隔。

单引号或双引号分隔传统字符串。

而反斜杠分隔模板文字。

字符串的单引号和双引号没有区别。

只有当我们想要创建包含其他类型表达式的字符串时，传统字符串才能被连接起来。

所以我们可以写:

```
const greeting = 'hello ' + name;
```

当我们有多个表达式时，很难使用串联。

制作多行字符串也比传统字符串更难，因为我们必须自己添加换行符。

例如，我们可以写:

```
const greeting = 'hello\n' + name;
```

所以如果我们想创建一个有很多行的字符串，那么我们会很困惑。

我们仍然可以用它们来弹奏短弦。

为了使它们易于使用，我们在任何地方都坚持使用单引号或双引号。

我们不应该使用`\`符号来创建多行字符串，因为它是一种非标准的语法，并不能在所有引擎上工作。

所以我们不应该写这样的东西:

```
let longStr = "The quick brown fox jump \
 over the dog.";
```

我们有一个带`\`的长字符串。

这不是一个标准。

我们应该做的是使用模板文本来创建我们的字符串。

它们可以包含表达式，我们不需要做任何特殊的工作来创建多行字符串。

例如，我们可以写:

```
const greeting = `hello ${name}`;
```

将`name`变量的值嵌入字符串中。

要创建多行字符串，我们只需编写:

```
let longStr = `The quick brown fox jump
 over the dog.`;
```

该字符串将与新行一起完整地返回。

![](img/bc0fc39520d3e5c622871b0ee02b195c.png)

Photo by [Laura Johnston](https://unsplash.com/@lauramakoj?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用模板文字使创建字符串变得更容易。

构造函数和类都是 Pascal 大小写。

## **简明英语 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**