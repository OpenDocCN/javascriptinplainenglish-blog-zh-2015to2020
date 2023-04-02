# 面向未来的 React 应用

> 原文：<https://javascript.plainenglish.io/future-proof-your-react-app-55fc6ac2cb10?source=collection_archive---------17----------------------->

## 第 2 部分:使用类型脚本

不过，很快，为了方便起见，这个系列的第一部分就在这里 🥳

## 我最近在做什么

你们中的一些人可能知道，我参加了熨斗学校的软件工程沉浸式奖学金，我上周刚刚从该项目毕业！我也答应了这个系列的第二部分，我想为这篇文章的延迟发表向大家道歉！项目的最后几周是最具挑战性的部分。顶点项目、所需的时间管理技能以及克服所有挑战的纪律是对我的性格以及我对软件开发的热爱程度的考验。现在我正在学习算法和数据结构(痛苦但有趣)，并准备开始找工作。

好了，我的最新消息到此为止。让我们浏览一下这篇文章中将要涉及的内容。

![](img/0e5a27833d84b0633b61316d05c504cf.png)

Photo by [Cole Keister](https://unsplash.com/@coleito?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 议程

*   Javascript 和 TypeScript 的历史——什么是 TypeScript？
*   TypeScript 能帮助我们完成什么？
*   类型推理
*   键入形状
*   键入“任何”
*   键入注释
*   关闭

## JavaScript 和 TypeScript 的历史——什么是 TypeScript？

当有人谈到 TypeScript 时，JavaScript 几乎总是在对话中出现。这是因为 TypeScript 被认为是 JavaScript 的*超集*，为语言增加了*类型*功能。JavaScript 在 1995 年被发明，它花了几年时间才能够完成我们今天看到的用户和网页之间的动态交互。

对于初学者来说，JavaScript 是一种很好的学习语言，因为它很灵活，而且很多工作都是在幕后完成的。然而，灵活的代价是，当涉及到类型时，它没有其他编程语言那么严格。当“错误类型”的值被传递到函数中时，JavaScript 不会告诉程序员有错误。JavaScript 将尝试返回一些值，这有时可能没有多大意义。

这就是微软在 2012 年介入并开发 TypeScript 的地方，它在 JavaScript 原有的灵活性基础上增加了更严格的行为。TypeScript 有一个*类型系统*，它执行*类型检查*来突出潜在的错误并提供有用的提示。很快你就会明白为什么根据 StackOverflow 的统计，TypeScript 是排名第二的最受欢迎的语言。

> 从我的参考资料中阅读更多关于 JavaScript 和 TypeScript 的历史，以扩展您的知识！

## TypeScript 能帮助我们完成什么？

TypeScript 语法与 JavaScript 语法相同。使用 TypeScript 还有一个额外的步骤，需要一个编译器将 TypeScript 代码转换成 JavaScript 代码。

```
To install TypeScript, run $ **npm install -g typescript**To run/compile TypeScript code run $ **tsc <filename>**
```

通过编写类型脚本代码并运行编译器:

```
**TS code:** let firstName = 'wilson'**Terminal:** $ tsc <filename>**JS code generated from compiler after running the terminal cmd:** let fistName = 'wilson'
```

编译器生成一个以**结尾的新文件。js** 包含有效的 JavaScript 代码。他们大多数时候看起来都差不多！现在让我们探索一下 TypeScript 的一些关键功能。

**类型推断** — TypeScript 可以在赋值时推断变量的数据类型，而无需指定类型。

要了解 TypeScript 可以快速识别的“原始”数据类型:

*   布尔型
*   线
*   数字
*   空
*   不明确的

```
**TS code:**
let boolean = 'true'
boolean = true**After trying to run $ tsc:** error: Type 'boolean' is not assignable to type 'string'.
```

上面是一个 TypeScript 不允许程序员因为类型推断而重新分配不同类型的值的例子。它期望该变量的数据类型总是与声明时最初分配的数据类型相匹配。

**类型形状** — TypeScript 知道我们正在处理的对象的*形状*，形状指的是一个对象定义/内置了什么属性和方法。

```
**TS code:** let array = ['wilson', 'ng', 'the', 'avatar']
array.toLowerCase()**result after running $ tsc:** error:Property 'toLowerCase' does not exist on type 'string[]'.**JS code:** let array = ['wilson', 'ng', 'the', 'avatar'];
array.toLowerCase();**result in:** TypeError: array.toLowerCase is not a function**A more helpful example would be something like a spelling error:****TS:** 'wilson'.lenght 
*Property 'lenght' does not exist on type '"wilson"'. Did you mean 'length'?***versus****JS:** 'wilson'.length returns *undefined* - **breh**(sorry for the lack of a better expression)
```

好吧，当我们想声明一个变量但还没有赋值的时候会发生什么呢？亲爱的朋友，这是个很好的问题。TypeScript 不会推断该变量的类型。在这种情况下，变量将被视为类型 **any。**

```
let whatsMyType;  <-- **this becomes type 'any'**whatsMyType = 'string'whatsMyType = 10Above TypeScript code will not encounter any error, and it's valid.The value of 10, which is a type of **number**, will override the value of 'string' safely.
```

**好的，那么我们怎样才能防止一个变量变成‘any’类型呢？**嗯，又是一个好问题，输入“任何”不一定是错的。尽管如此，在这种情况下，TypeScript 仍无法正常工作——这是为了防止程序员将错误类型的值赋给现有变量。这里我们可以利用 TypeScript 告诉一个变量使用 ***类型注释*** *可以设置什么数据类型。下面是它的样子。*

```
**TS code:****let variableMustBeNumber:number** <-- colon ':' after variable followed by data type for annotations**variableMustBeNumber = 5**^valid code**let variableMustBeNumber:number**  <-- colon ':' after variable followed by data type for annotationsCan also be written with space if that helps readability for you:**let variableMustBeNumber : number** <-- spaces allowedvariableMustBeNumber = 'not a number' <-- assigning a type 'string'**results in:** error: Type 'string' is not assignable to type 'number'.
```

## 关闭

我知道我们没有谈到这对我们的 React 项目有什么帮助，但是一旦我们涉及到其他一些更复杂的数据类型，比如对象和数组，我们肯定会谈到的。在涉足 React 国家水域之前，我们还需要覆盖函数。我希望你们都能看到学习 TypeScript 的价值以及错误消息的帮助！最后，请随时在 LinkedIn 上联系我。我喜欢建立有意义的联系，这些职业关系将帮助我在科技行业获得第一份工作！

感谢您阅读我的文章🥳

## 参考

 [## 类型-程序员概要

### 考虑两种著名的静态类型语言:Go 和 Haskell。Go 的类型系统缺少泛型:类型是…

www.destroyallsoftware.com](https://www.destroyallsoftware.com/compendium/types?share_key=baf6b67369843fa2)  [## JavaScript 的历史

### 要想很好地使用 JavaScript，有必要了解它的性质、历史和局限性。JavaScript 的创始人是…

www.w3schools.in](https://www.w3schools.in/javascript-tutorial/history-of-javascript/) [](https://www.tutorialsteacher.com/typescript/typescript-variable) [## TypeScript 变量声明:var，let，const

### 了解使用 var、let 和 const 关键字在 TypeScript 中声明变量的不同方式。

www.tutorialsteacher.com](https://www.tutorialsteacher.com/typescript/typescript-variable) [](https://www.typescriptlang.org/) [## 任意比例的输入 JavaScript。

### TypeScript 通过向语言中添加类型来扩展 JavaScript。TypeScript 通过以下方式加速您的开发体验…

www.typescriptlang.org](https://www.typescriptlang.org/)  [## 理解 TypeScript 的类型标记法

### 这篇博客文章是对静态类型的 TypeScript 符号的快速介绍。目录:看完这个…

2ality.com](https://2ality.com/2018/04/type-notation-typescript.html)  [## 面向知道如何构建 Todo 应用程序的 JS 程序员的 TypeScript 教程

### 2011 年，Backbone.js 是最受欢迎的 JavaScript 库之一(React 于 2013 年问世；2014 年的 Vue)。当…

ts.chibicode.com](https://ts.chibicode.com/todo/)