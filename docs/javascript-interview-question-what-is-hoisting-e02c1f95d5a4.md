# JavaScript 面试问题:什么是吊装？

> 原文：<https://javascript.plainenglish.io/javascript-interview-question-what-is-hoisting-e02c1f95d5a4?source=collection_archive---------4----------------------->

![](img/37c8b7ff050492a0107da030935b292c.png)

Photo by [Ignacio Amenábar](https://unsplash.com/@pataun?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/question?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

即使提升不是你作为一个 web 开发人员每天都要处理的事情，特别是如果你像我们中的许多人一样正在使用框架，它也是一个非常受欢迎的面试主题。正确回答这个问题表明你对你正在使用的语言有很好的了解，但更重要的是它显示了你的好奇心，并且表明你是那种不仅能让事情运转起来，而且对**事情如何运转**感兴趣的开发人员。

# 三言两语，什么是吊装？

JavaScript 是一种解释型语言。如果我们忘记了框架、超集语言、编译器等等，在最基本的普通 JS 项目中，你在 IDE 中写的东西就是用户的浏览器将要接收和运行的东西。

在真正开始执行你的代码之前，浏览器会先看一下它，并给自己标记重要的东西(它正在创建所谓的执行上下文)。在这个阶段，解释器首先扫描函数声明的上下文。找到的每个函数都存储在内存中，解释器保持一个指向那个位置的指针。然后它寻找变量声明，如果找到一个，它就把它的名字保存在内存中。

这意味着当它开始实际执行代码时，JavaScript 引擎已经知道它在执行上下文创建期间找到的函数和变量。您可能会在其他来源中读到，浏览器正在将变量和函数的声明“移动”到它们的执行上下文的顶部。这在技术上是不正确的，浏览器实际上并没有移动任何东西，但是这是理解提升的后果的一个很好的心理表现。

# 为什么对我们开发者很重要？

理解你的浏览器如何解释你的代码，以及解释的顺序，这本身就很有趣，但是这会给你带来什么后果呢？

对于函数来说，它有一个相当大的含义:如果一个函数是在作用域中定义的，你就可以使用它，甚至是在声明之前。这意味着您可以在脚本底部声明助手函数，并且仍然可以在代码顶部的高级逻辑中使用它们。但是注意，只有直接声明函数才有效，而不是把函数存储在变量里。

此功能被提升:

```
function b() {
  return 3;
}
```

这个不是(只有变量`c`被提升，所以`undefined`也是，直到达到这个代码)。

```
var c = function() {
  return 4;
}
```

对于变量，它会导致一些棘手的错误。你可能会错误地使用一个还没有初始化的变量。因为变量存在，所以不会抛出错误。例如，这将在您的控制台中记录`undefined`:

```
(function() {
  console.log(a);
  var a = 2;
}());
```

在一些更复杂的情况下，可能需要大量的调试来找出为什么没有得到预期的结果。知道吊装的存在是很好的第一步。

# 提升和 ES6 关键字

这可能是一个棘手的面试问题:在这种情况下，控制台中记录了什么:

```
(function() {
  console.log(a);
  let a = 2;
}());
```

如果你不知道提升，你可能会回答抛出了一个错误。如果你知道提升，你可能会说。

实际上，第一个答案是正确的，抛出了一个错误，但不是因为用`let`声明的变量没有被提升(或者用`const`声明，甚至是用`class`声明的类)。当用`var`声明的变量在执行上下文中被初始化为`undefined`时，用 ES6 关键字声明的类和变量不会被初始化。JavaScript 引擎知道它们，但是你不能使用它们。

而使用未声明的变量`c`会抛出这个错误:

```
Uncaught ReferenceError: c is not defined
```

尝试记录前面示例中的`a`会引发:

```
Uncaught ReferenceError: Cannot access 'a' before initialization
```

它在某种程度上修复了意外提升变量的问题，同时仍然清楚地区分了提升但未初始化的变量和未声明的变量。

这里用几句话简单地解释了吊装，就像你在面试中会做的那样。要真正深入了解执行上下文和提升，我推荐以下资源:

[](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/) [## JavaScript 中的执行上下文&栈是什么？

### JavaScript 中的执行上下文&栈是什么？-在这篇文章中，我将深入探讨一个最…

davidshariff.com](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/) 

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的喜爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****