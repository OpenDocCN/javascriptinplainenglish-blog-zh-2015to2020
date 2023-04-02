# 2020 年 JavaScript 工作技术面试题

> 原文：<https://javascript.plainenglish.io/javascript-job-technical-interview-questions-in-2020-7470d8763732?source=collection_archive---------1----------------------->

## Java Script 语言

## 你能回答几个？

![](img/63dafae9de8a1028fac373ef888a7085.png)

Photo by [Jonathan Borba](https://unsplash.com/@jonathanborba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最近几个月，我一直在努力找一份新工作。嗯，我不确定这是否是正确的表达方式，我想在另一家公司工作。我更新了我的简历，参加了一些工作面试——实际上不止几次。

在这篇文章中，我将与你分享最常见的 5 个 JavaScript 技术问题。我就写一下我是怎么回答的。如果有什么我应该知道的或者我错了，请随意比较你和我的答案并评论我！

# 什么是终结？

简而言之——闭包是一种现象或概念，它给你一个到已经完成执行的函数的访问链接。

详细回答—当在当前活动/运行的执行上下文的执行阶段执行一个函数时，会创建一个新的执行上下文，并将其置于先前活动的执行上下文之上。内部函数在外部函数中声明。当一个函数诞生时，它被赋予了一个外部变量的访问链接，我们称之为作用域链。因为内部函数作为外部函数的输出返回，所以您仍然可以使用返回的内部函数访问变量。即使外部函数的执行上下文已经消失或从执行堆栈中删除，从内部到外部的访问链接仍然有效。这被称为结束。闭包在我们的代码中无处不在，如果你以错误的方式使用或滥用它们，它可能会导致内存泄漏。

*   [关于这个话题的相关帖子](https://medium.com/better-programming/execution-context-lexical-environment-and-closures-in-javascript-b57c979341a5)——JavaScript 中的执行上下文、词法环境和闭包

# 箭头函数和普通函数有什么不同？

简而言之——箭头函数没有关键字`this`,而普通函数有。此外，`Function.prototype.bind`对箭头函数不起作用，箭头函数不能是构造函数。

详细回答——当声明一个函数时，JavaScript 引擎会计算出该函数的类型。如果函数是箭头函数，引擎会将其引用类型分类为“词法”。当声明一个函数时，引擎将引用链接设置为`this`。但是，如果函数的引用类型是“词法”，那么引擎不会创建`this`。相反，当您调用它的`this`时，该函数试图在外部执行中寻找`this`。结果，`Function.prototype.bind`也不起作用，因为`bind`所做的是改变`this`引用——反正箭头函数的`this`不存在，所以毫无意义！当引擎创建一个函数时，只有当函数类型是普通函数时，它才创建函数的原型。

*   [关于这个话题的相关帖子](https://medium.com/better-programming/javascript-whats-the-difference-between-normal-and-arrow-functions-74c367324ae1) — JavaScript:普通函数和箭头函数有什么区别？

# 什么是承诺？

简而言之——promise 是一个异步代理，它允许您异步运行代码。它通常被比作`async-await`，主要区别在于`async-await`是你可以同步执行代码的方式。

详细回答——要理解什么是承诺，你应该知道 JavaScript 中事件的基本历史。当一些代码运行时间过长时，它们会停止整个程序。所以许多开发人员应该非常小心不要挂起进程。您可以使用稍后运行代码的`setTimeout`或`setInterval`。JavaScript 将这些称为宏任务，而将其他普通任务称为任务，比如`console.log(1)`。宏任务总是在所有正常任务执行后执行。但是他们不能向你保证他们不会打扰或扰乱渲染步骤。因此，有时或有时以上，他们把渲染步骤推到当前节拍或下一节拍的末尾，因为它们需要很长时间。`requestAnimationFrame`是解决这个问题的下一代异步函数。这也是一个宏任务，在正常任务之后执行。但它肯定会在 Chrome 渲染步骤之前执行。在 Safari 中，它在渲染步骤之后执行。

但是你不能控制回调何时必须使用`requestAnimatinoFrame`运行，尽管在渲染步骤上它比`setTimeout`更好。相反，ES6 中发布了一个新功能，叫做 Promise。这也是一个异步函数，但是它并没有真正完成任何渲染步骤。无极、`setTimeout`和`requestAnimationFrame`之间有一个主要的区别。您可以通过拨打`res(), rej() or then()`来控制您的承诺何时生效。这些方法返回了一个新的承诺。承诺被称为微观任务，而不是宏观任务——不要混淆！它们必须在每个正常任务完成后才能执行。

但是，你也应该向面试官解释的一个重要事实是，无论如何，JavaScript 都是一种单线程语言。`setTimeout`、`requestAnimationFrame`、`Promise`……一切都好，但他们无法逃离一个单线程国家。也就是说,`asynchronous`这个词听起来似乎不受代码块挂起的影响，但事实并非如此。

```
setTimeout(() => {
  console.log('setTimeout-1')
  for (let i = 0; i < 10e9; i +=1) {}
})
setTimeout(() => {
  console.log('setTimeout-2')
})
```

打印`setTimeout-2`需要很长时间。

*   [关于此主题的相关帖子](https://medium.com/better-programming/be-the-master-of-the-event-loop-in-javascript-part-1-6804cdf6608f)——掌握 JavaScript 中的事件循环(第 1 部分)

# 什么是 HOF？

简短回答 HOF 是一个函数，它以一个函数作为输入，并返回一个函数作为输出。

详细答案——HOF 不仅仅是一个接受并返回函数的函数，它还可以是一个闭包(您应该知道闭包是什么意思)。HOF 返回一个函数，该函数使用声明返回函数的执行环境，换句话说，是一个外部函数。

```
const hof = (ctx) => 
  (props) => ctx.props = props;
```

此示例显示了 HOF 可以成为闭包的原因。此外，HOF 应该是一个纯函数，因为如果有可能导致副作用，那么它总是不好的，尤其是当它可能是一个闭包时。在反应中，存在来自 HOF 的 ad HOC。唯一的区别在于，HOC 返回的是一个组件，而不是一个函数。

*   [关于本主题的相关帖子](https://medium.com/javascript-in-plain-english/functional-programming-higher-order-function-hof-aaa46bb444bb)——函数式编程::高阶函数，HOF

# 常量和字母与 var 有什么不同？

简答— `const`和`let`是变量声明的阻塞范围关键字。并且`const`不允许值一旦被设置就被改变，不像`let`那样是可能的。在现代 web 编程中，为了更加稳定，你通常需要使用`const`和`let`。

详细回答——`const/let`和`var`的主要区别在于它们的范围。`var`位于其周围最近的函数内，而`const`和`let`仅位于最近的块内(`{}`)。对`cosnt`和`let`的误解之一是它们没有被吊起，这实际上并不正确。当 JavaScript 引擎收集变量并检查它们的变量关键字时，它将`const`和`let`推到上下文的一边，等待变量被赋值。这个被引擎推开关键词的区域叫做 TDZ，时间死区。当你试图访问用`var`声明的变量时，如果你没有给它赋值，它将返回`undefined`。然而，用`const`或`let`声明的变量会出错。看起来好像是因为它们没有被升起，但这不是真的。这仅仅是 TDZ 的一个特色。更有趣的是，你不能访问在全局上下文中声明的变量，比如用`const`声明的`window.x`。它总会回报你的(如果你不相信我，你自己试试吧！)

```
// on the global context
var a = 1;
let b = 2;
const c = 3;window.a // 1
window.b // undefined
window.c // undefined
```

这是因为当 JavaScript 引擎初始化变量时，它不会将`const`或`let`变量添加到最顶层的上下文中，即`window`。

*   [关于这个话题的相关帖子](https://medium.com/javascript-in-plain-english/do-you-know-why-this-x-is-undefined-when-x-is-declared-with-const-or-let-8f6e00dcd490) —知道为什么用 Const 或 Let 声明 x 时 this.x 是未定义的吗？

# 结论

当然，对于 JavaScript 开发人员来说，还有很多面试问题——如果有机会的话，我会尽量稍后与您分享更多。但是，重要的是，至少我认为是重要的，你应该能够解释相关的故事，而不仅仅是主要概念本身，因为 JavaScript 有这么多不同的概念，它们都结合在一起，构成了语言的坚实结构。

感谢你阅读这篇文章！

# 资源

*   我在面试中的个人经历

我们总是有兴趣帮助推广高质量的内容。如果你有一篇想用简单英语提交给 JavaScript 的文章，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[](mailto:submissions@javascriptinplainenglish.com)****给我们，我们会把你添加为作者。****