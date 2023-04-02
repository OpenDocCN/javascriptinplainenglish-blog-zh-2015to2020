# JavaScript 中解析和代码执行的工作方式

> 原文：<https://javascript.plainenglish.io/javascript-parsing-and-code-execution-f92a08498ec1?source=collection_archive---------2----------------------->

JavaScript 总是被托管在一些环境中执行，这些环境最初是从浏览器开始的，然后是使用 Node.js 的服务器端脚本，还有一些接受 JavaScript 代码作为输入的应用程序。这里我们将重点讨论浏览器。不同的浏览器有不同的 JavaScript 引擎来解析和执行我们的代码。

> **JavaScript 引擎**是一个执行 JavaScript (JS)代码的计算机程序。第一个 JavaScript 引擎仅仅是解释器，但是所有相关的现代引擎都利用即时编译来提高性能

![](img/570158b0b923f6433ed4a3c8d9dca2d6.png)

JavaScript 引擎中发生的第一件事是通过解析器解析我们的代码。一个**解析器**基本上一行一行地读取我们的代码，检查它是否有错误，如果有，就停止执行，如果代码没有语法错误，解析器产生一个叫做抽象语法树的数据结构，然后被翻译成机器码，从而输出结果。

> **抽象语法树** ( **AST** )，或简称**语法树**，是用编程语言编写的源代码的抽象语法结构的树形表示。树的每个节点表示源代码中出现的一个构造— [维基百科](https://en.wikipedia.org/wiki/Abstract_syntax_tree)

为了理解 JavaScript 代码执行，我们需要仔细阅读并理解两个重要的概念:执行上下文和执行堆栈。

**执行上下文:** 我们的代码运行的环境被称为执行上下文，定义为以下
1。全局—首次执行代码的默认环境。
2。局部/函数—函数中的代码何时执行。
3。Eval —当执行 Eval 中的文本时。

```
*// Global context**var* person = 'john';*const* func1 = *function*() {*// execution context.**let* address = 'New York';func2();console.log(`${person} ${address}`);}*const* func2 = *function*() {*// execution context.**let* designation = 'Software Developer';*let* address = 'Melbourne'console.log(`${person} ${designation} ${address}`);}func1();
console.log(window.person === person)Output:
 john Software Developer Melbourne
 john New York
 true
```

让我们理解上面的例子，变量 person 是在全局上下文中定义的，全局上下文用于执行不属于任何函数的代码。每当调用一个函数时，就会创建一个新的执行上下文，并将其推到当前上下文之上，从而创建所谓的执行堆栈。

**注意:**这里我们看到 window.person ===person 给出的结果为 true，但是如果我们做 window.func1()，这将会给出 TypeError。下面是关于这种行为的简介。

> [**全局环境记录**](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-global-environment-records) 实际上由两条环境记录组成:
> 一条声明性环境记录和一条对象环境记录。
> 对象环境以全局对象 window 为后盾，包含 var 声明和浏览器提供的其他全局。声明性环境包含 let、const、class 等声明。—
> [学分堆栈溢出](https://stackoverflow.com/questions/55030498/why-dont-const-and-let-statements-get-defined-on-the-window-object)。

**执行堆栈:**
我们所知的 JavaScript 在单线程上运行其操作，这意味着一次只能发生一个动作，其余的被推到一个称为执行堆栈的堆栈上，该堆栈遵循堆栈 **LIFO 的原则。** 简而言之，我们可以说执行堆栈的重要方面是其同步行为的本质，以及一个全局上下文和许多功能/局部上下文。

**执行上下文阶段:** 执行上下文分为创建和执行两个阶段。
创建阶段:在运行任何代码之前调用函数时定义该阶段。在创建阶段，首先是创建变量对象，然后是作用域链，最后是确定和设置“**this”**，它们共同构成执行上下文对象的属性。
按照流程中的以下步骤创建变量对象

1.  添加包含函数调用中传递的所有对象的参数对象。
2.  对于每个函数，在变量对象中创建一个属性，指向该函数，即在代码开始执行之前，所有函数都将存储在对象中。
3.  查找变量声明，并在变量对象中为每个变量设置一个属性，并设置为未定义。

最后两点一般称为吊装。函数和变量被提升，这意味着它们在执行阶段开始之前就可用，但是有一个小问题，两者的提升方式不同，变量只在执行阶段定义，而函数已经定义了。让我们看一个例子，也有点惊讶。

```
*// with var/ ES5**function* func1() {
  console.log('Args:', typeof *arguments*); console.log('typeof func2:', typeof func2); console.log('typeof variable:', typeof person); console.log('Args:', *arguments*); console.log('variable:', person); console.log('function assignment:', getName); *var* person = 'Paul';
  *var* getName = _getName(); *function* func2() {
    console.log('USA');
  } func2();
}*function* _getName() {
  *return* 'Steve';
}func1('George', 1982, 'USA');Output: 
  Args: object
  typeof func2: function
  typeof variable: undefined
  Args: [Arguments] { '0': 'George', '1': 1982, '2': 'USA' }
  variable: undefined
  function assignment: undefined
  USA// With let/const ES6*function* func1(...args) {
  console.log('Args:', typeofargs); console.log('typeof func2:', typeof func2); console.log('typeof variable:', typeof person); console.log('Args:',args); console.log('variable:', person); console.log('function assignment:', getName); *let* person = 'Paul';
  *const* getName = _getName(); *function* func2() {
    console.log('USA');
  } func2();
}*function* _getName() {
  *return* 'Steve';
}func1('George', 1982, 'USA');Output: 
  Args: object
  typeof func2: function
  ReferenceError: Cannot access 'person' before initialization
```

> 这个错误的答案在 ECMAScript 规范中。
> `**let**`和`**const**`声明定义了作用于[运行执行上下文](https://www.ecma-international.org/ecma-262/9.0/index.html#running-execution-context)的词汇环境的变量。变量在包含它们的[词法环境](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-lexical-environments)被实例化时被创建，但是在变量的[词法绑定](https://www.ecma-international.org/ecma-262/9.0/index.html#prod-LexicalBinding)被求值之前不能以任何方式被访问— [ECMAScript](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-let-and-const-declarations)

简单地说，词汇上声明的变量保持**未初始化**。这意味着当您尝试访问 ReferenceError 时，会引发该异常。只有当 let/const 语句被求值时，它才会被初始化，之前(上面)的一切都被称为时间死区。

如果你觉得这篇文章很有帮助并且喜欢它，请随意与你的朋友和同事分享。

你有任何问题、建议或想要联系我吗？在 LinkedIN[上给我留言或者在下面评论。](https://www.linkedin.com/in/muneer-zargar-fe-dev/)

我也可以在推特上通过@zargarmuneer90 找到我。

下面是我在写这篇文章时参考的一些好的资源。

 [## AST 浏览器

### 在线 AST 浏览器。

astexplorer.net](https://astexplorer.net/) [](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/) [## JavaScript 中的执行上下文&栈是什么？

### JavaScript 中的执行上下文&栈是什么？-在这篇文章中，我将深入探讨一个最…

davidshariff.com](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)