# 9 用几句话解释常见术语的反应

> 原文：<https://javascript.plainenglish.io/9-react-common-terms-explained-in-a-few-words-90f237952070?source=collection_archive---------5----------------------->

## 以防你明天有个面试

![](img/b8afcac8a801a099843e1b9c78f0aac2.png)

The React universe can be daunting

学习一门编程语言与学习一门“真正的”语言非常相似。有要使用的单词、动词和规则。尤其是单词，我们把它们组合在一起，形成了一个我们日常使用的字典。

无论是如何说“你好吗”，还是如何定义用户界面，字典是我们用语言做的更复杂的事情的基础。

这里有一个快速指南，试图教您在学习反应时常用术语的含义。它将形成你今天从图书馆开始，以后掌握它的基本语法。

## #1 库

您可能已经看过一百篇比较最好的 [Javascript 前端框架](https://medium.com/javascript-in-plain-english/angular-vs-react-a-component-based-battle-f5245fa2f544)的文章。反应总是在那里。但这在技术上是不正确的，因为当我们谈论它的时候，我们在和一个图书馆打交道。

库只是一个类定义的集合，帮助用户完成特定的任务。在反应的情况下，定义用户界面。

## #2 组件

反应组分与功能非常相似。他们以道具的形式接受输入，然后产生一些输出。该输出定义了屏幕上应该出现的内容。组件对于构建用户界面非常有用，因为它们允许您定义它的独立部分，您可以按照自己想要的方式使用和处理它们。

一个组件可以是一个类组件，这意味着它基本上是一个将返回一些东西在屏幕上呈现的类

或者类似的，一个函数。这与描述 UI 的目的是一样的，但是在涉及到生命周期方法或钩子等其他因素时就有些不同了。

## #2 道具

如果不能定制一点，没有什么是真正有趣的，对吗？就像你把数据传递给`add(val1, val2)`函数来增加数字一样，你把它们传递给反应组件。

道具就是你传递给反应组件的有意义的输入。

## #3 州

当组件需要某种方式来存储本地数据时，它就需要状态，而本地数据可能会随着时间而改变。就像一个`CheckBox`组件的状态信息。或者`show`在一个`Accordion`的状态之内。

记住，国家和道具之间最大的区别在于它们来自哪里。道具来自父组件，而状态由您自己定义的组件管理。道具不能改变，但是国家可以。

## #4 JSX

React 组件不是用您可能熟悉的语法定义的。React 使用 [JSX](https://reactjs.org/docs/introducing-jsx.html) ，这是 Javascript 的语法扩展。您的 JSX 将被编译成`React.createElement()`调用，这些调用将返回名为“React elements”的普通 JavaScript 对象。

## #5 Javascript 编译器

Javascript 编译器获取 Javascript 代码，对其进行转换，然后以不同的格式返回。最常见的情况是当您想要使用最新的 ES6 特性时，这些特性仍然不受旧浏览器的支持。 [Babel](https://babeljs.io/) 是 React 最常用的编译器。

## #6 ES6

首字母缩略词，指的是最新版本的 [ECMAScript 语言规范标准](https://www.ecma-international.org/publications/standards/Ecma-262.htm)，JavaScript 语言是其实现。这个版本包含了许多新闻，如 [let 和 const](https://tylermcginnis.com/var-let-const/) 、[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)和[模板文字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)。

## 7 号捆扎机

bundler 帮助你获取不同的 JS 和 CSS 模块，并把它们组合成几个为浏览器优化的文件。React 最常用的捆绑器是 [Webpack](https://webpack.js.org/) 。

## #8 元素

React 元素是 React 应用程序的构建块。人们可能会混淆元素和组件。一个元素描述了你想在屏幕上看到什么。记住，元素是不可变的。

## #9 生命周期方法

组件可以像生物一样被考虑。他们会经历生命的各个阶段，我们可以通过生命周期方法来了解这些阶段。例如，当一个组件诞生时，我们称之为挂载，并使用`componentDidMount()`方法来管理其生命周期的这一部分。或者当它改变时，我们说它更新，所以我们使用`componentDidUpdate()`。

## 结论

希望这是 React 开发人员使用的常用词词典的一个有用的鸟瞰图。这可以让您了解 React 的工作原理及其背后的主要概念。

—皮耶罗

## 资源

*   所有的图标都是由神奇的网站制作的

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****