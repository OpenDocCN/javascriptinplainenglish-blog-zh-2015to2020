# JavaScript 干净代码—快速最佳实践

> 原文：<https://javascript.plainenglish.io/javascript-clean-code-best-practices-461c24c53cae?source=collection_archive---------0----------------------->

![](img/894bd4208ad0b7b353dcc6eaa8dfb080.png)

## 让你的代码容易被人理解

# 介绍

如果你关心代码本身和它是如何被编写的，而不是仅仅担心它是否工作，你可以说你实践并关心干净的代码。

> 专业开发人员将为未来的自己和*“其他人”*编写代码，而不仅仅是为机器

基于此，干净的代码可以被定义为以这样一种方式编写的**代码，这种方式是自解释的，易于人类理解，并且易于更改或扩展**。

> 即使糟糕的代码也能运行，但是如果代码不干净，它会使开发组织陷入困境。

在本文中，重点将放在 **JavaScript** 、*上，但是这些原则也可以应用于其他编程语言。*

# 1.强类型检查

用`===`代替`==`

# 2.变量

> 用一种能揭示变量背后意图的方式来命名变量

这样，当一个人看到它们后，它们变得可搜索且更容易理解。

**不良**👎

**好的**👍

## 不要在变量名中添加多余的不需要的单词

**不良**👎

```
let nameValue;let theProduct;
```

**好的**👍

```
let name;let product;
```

## 不要强调记忆可变上下文的必要性

**不好**👎

**好的**👍

## 不要添加不必要的上下文

**坏**👎

**好的**👍

## 对相同类型的变量使用相同的词汇

**坏**👎

```
getUserInfo();getClientData();getCustomerRecord();
```

**好的**👍

```
getProduct();
```

# 3.功能

使用长的描述性名称。

考虑到它代表了某种行为，函数名应该是一个动词或一个短语，充分暴露其背后的意图以及参数的意图。

他们的名字应该说明他们做什么。

**坏**👎

```
function email(user) {
  // implementation
}
```

**好的**👍

```
function sendEmailUser(emailAddress) {
  // implementation
}
```

## 避免大量的争论

理想情况下，函数应该指定两个或更少的参数。

参数越少，测试函数就越容易。

**坏**👎

```
function getProducts(fields, fromDate, toDate) {
  // implementation
}
```

**好的**👍

## 使用默认参数而不是条件

**糟糕的**👎

```
function createShape(type) {
  const shapeType = type || "circle";
  // ...
}
```

**好的**👍

```
function createShape(type = "circle") {
  // ...
}
```

## 避免在一个函数中执行多个操作

严重的👎

**好的**👍

## 使用`Object.assign’` 设置默认对象

**不良**👎

**好的**👍

## 不要使用标志作为参数，因为它们告诉你这个函数做的比它应该做的要多

**坏**👎

**好的**👍

## 不要污染地球

如果你需要扩展一个现有的对象，使用 ES 类和继承，而不是在本地对象的原型链上创建函数。

**糟糕的**👎

```
Array.prototype.myFunction = function myFunction() {
  // implementation
};
```

**好的**👍

```
class SuperArray extends Array {
  myFunc() {
    // implementation
  }
}
```

# 4.条件式

避免消极条件句。

**坏**👎

```
function isPostNotPublished(post) {
  // implementation
}if (!isPostNotPublished(post)) {
  // implementation
}
```

**好的**👍

```
function isPostPublished(user) {
  // implementation
}if (isPostPublished(user)) {
  // implementation
}
```

## 使用条件速记

这可能是微不足道的，但值得一提。

这种方法仅用于布尔值，并且如果您确定该值不会是`undefined`或`null`。

**不良**👎

```
if (isValid === true) {
  // do something...
}if (isValid === false) {
  // do something...
}
```

**好的**👍

```
if (isValid) {
  // do something...
}if (!isValid) {
  // do something...
}
```

## 尽可能避免条件句

用**多态性**和**继承**代替。

**坏**👎

**好的**👍

# 5.ES 类

类是 JavaScript 中新的语法糖。

使用 prototype 时，一切都和以前一样，只是现在看起来不同了，你应该更喜欢它们而不是 ES5 的普通函数。

**坏**👎

**好的**👍

## 使用方法链接

很多库比如 **jQuery** 和 **Lodash** 都使用这种模式。

因此，您的代码不会太冗长。

在你的类中，只需在每个函数的末尾返回`this`，你就可以将更多的类方法链接到它上面。

**坏**👎

**好的**👍

# 6.避免使用 Eval

> **Eval 函数**允许我们将一个字符串传递给 JavaScript 编译器，并让它作为 JavaScript 执行

简单地说，您在运行时传入的任何东西都会像在设计时添加的一样得到执行。

```
eval("alert('Hi');");
```

这将弹出一个警告框，其中有消息“嗨”。

> 应该避免使用 Eval 函数，因为它不安全，并且为恶意程序员提供了潜在的威胁。

# 7.使用 JSLint

JSLint 是一个很棒的工具，可以帮助你识别 JavaScript 代码中的常见问题，它会挑出被浏览器屏蔽的**坏代码**。

你可以使用不同的网站，比如 JavaScriptLint.com T42，或者你可以使用众多可下载的 JSLint 工具之一。

例如， **Visual Studio 有一个 JSLint** 的插件，它允许你在编译时(或者手动)检查错误。

这将使你的代码更干净，并有助于防止那些讨厌的 bug 出现在产品中。

# 8.总体上避免

一般来说，**你应该尽力不重复自己**，意思是你不应该写**重复的代码**，不要留下尾巴，比如**未使用的函数和死代码**。

在这种情况下，删除重复的代码意味着抽象出差异，并在那个层次上处理它们。

而关于**的死码**，正如它的名字所说。

它是在我们的代码库中没有做任何事情的代码，因为在开发的某个时候，**你已经决定它不再有目的**。

您应该在代码库中搜索这些部分，并删除所有不需要的函数和代码块。

我能给你的一个建议是，一旦你决定不再需要它，就删除它。

# 结论

这只是改进代码的一小部分。

在我看来，这里所说的原则是人们经常不遵守的。

他们尝试着去做，**但是一直保持精神力量并做出伟大的代码并不容易**。

也许在项目开始时，代码是**整洁和干净的**，但是当涉及到满足最后期限时，这些原则经常被忽略，并被移到一个***【TODO】*或*【REFACTOR】*部分**。

感谢您的阅读，下一篇文章再见！

## 进一步阅读

[](/code-documentation-is-broken-but-i-think-swimm-may-have-fixed-it-daaa7547d834) [## 代码文档被破坏了——但是我认为 Swimm 可能已经修复了它

### 传统的文档管理系统让软件开发人员失望了，是时候来点新的了。游泳吗…

javascript.plainenglish.io](/code-documentation-is-broken-but-i-think-swimm-may-have-fixed-it-daaa7547d834) 

*更多内容请看* [***说白了就是***](https://plainenglish.io/) *。报名参加我们的* [***免费每周简讯***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)，[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*，*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*，以及* [***不和***](https://discord.gg/GtDtUAvyhW)*****。*****

***有兴趣规模化你的软件创业*** *？检查* [***电路***](https://circuit.ooo?utm=publication-post-cta) *。*