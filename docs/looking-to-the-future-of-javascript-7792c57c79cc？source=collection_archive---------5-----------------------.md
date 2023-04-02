# 展望 JavaScript 的未来

> 原文：<https://javascript.plainenglish.io/looking-to-the-future-of-javascript-7792c57c79cc?source=collection_archive---------5----------------------->

## ESNext:对不久的将来的建议

![](img/9e9349dc2d5dcf9d8697ee1df373a0fb.png)

一、什么是 ESNext？

通常，ECMAScript (JavaScript)的“下一个”版本指的是超出当前快照规范的持续移动的目标。

我将在本文中谈论的三个特性是在 TC39 [流程](https://tc39.es/process-document/)的第 3 和第 4 阶段。

在以前的帖子中，我已经写了新规范(ES2019-ES2020)中已经集成或即将集成的新功能:

*   [es 2019 中新增的 JavaScript 特性](https://medium.com/javascript-in-plain-english/javascript-es2019-es10-in-a-nutshell-cae6f7524519?source=your_stories_page---------------------------)。
*   [es 2020 中的新 JavaScript 特性](https://medium.com/javascript-in-plain-english/new-javascript-features-in-es2020-c2d76acf9c5a?source=your_stories_page---------------------------)。
*   [如何在 JavaScript 中使用新的 Nullish 合并运算符](https://medium.com/javascript-in-plain-english/nullish-coalescing-operator-for-javascript-8f502b970ba8?source=your_stories_page---------------------------)。
*   [JavaScript 中新的“顶级等待”功能如何工作](https://medium.com/javascript-in-plain-english/javascript-top-level-await-in-a-nutshell-4e352b3fc8c8?source=your_stories_page---------------------------)
*   [包含 JSON 和格式良好的 JSON.stringify](https://medium.com/javascript-in-plain-english/subsume-json-and-well-formed-json-stringify-323f70c9dc36?source=your_stories_page---------------------------)

这个新帖子是一个更新，我将包括该语言的三个新特性。前两个已经准备好包含在下一个规范中，最后一个必须遵循验收流程。

索引:

*   import.meta(阶段 4)
*   承诺.原型.最终(阶段 4)
*   逻辑赋值||=，&&=，？？=.(第三阶段)

# import.meta

作者:多米尼克·德尼科拉

这个提议是为了给 JavaScript 添加一个导入元属性，用于保存关于当前模块的特定于主机的元数据，即关于该模块的信息。这个新属性是一个具有空原型的扩展对象，它的属性是可写的、可枚举的和可配置的。

宿主环境返回一组属性(作为键/值对)，这些属性将被添加到对象中。

在浏览器中我们可以使用这个属性，例如以如下方式:

```
<script type="module" src="one_module.mjs"></script>
```

现在，您可以使用 import.meta 对象来访问关于该模块的元信息，在本例中，该对象返回一个指示该模块的基本 URL 的对象:

```
console.log(import.meta);
//{url:"file:///home/one-module.mjs"}
```

import.meta 元属性在模块中语法上是有效的，因为它意味着关于当前运行的模块的元信息。

# 承诺.原型.最终

作者:乔丹·哈班德。

许多 promise 库都有一个“finally”方法来注册一个回调函数，当一个 promise 被实现或拒绝时调用这个函数。例如，一个用例可以是记录操作已经完成，而不管它是否成功。

“finally”方法不会收到任何争论，因为没有可靠的方法来确定承诺是被履行还是被拒绝。在这种情况下，您只想始终执行一个任务。

在本例中，当我们执行 Fetch 操作时，我们希望在页面上显示一个 loading 微调器，但是当 Fetch 请求以“fulfilled”或“rejected”结束时，我们希望删除它:

```
let loading = true;fetch(myRequest)
.then( (response) => response.json))
.then( json => { 
   //Do some stuff
 }.catch( (error) =>{ console.error(error) }; 
.**finally**(() => { loading = false; });
```

# 逻辑赋值

作者:贾斯汀·里奇威尔和赫曼思·HM

直到现在，JavaScript 都不支持逻辑赋值，而现在，有了这个提议，我们可以组合逻辑运算符:“&&，”||，“and”？?"使用赋值表达式:" = "

请记住:

*[](https://medium.com/javascript-in-plain-english/nullish-coalescing-operator-for-javascript-8f502b970ba8)**符？？“的行为与运算符“||”非常相似，只是它在计算运算符时不使用“truthy”。相反，它使用“nullish”的定义，这意味着该值严格等于 null 或未定义。***

**我们来看看逻辑运算的语义。**

****逻辑“与”的语义:****

```
**a && b;
//b when a is truthy
//a when a is not truthy**
```

**“还有还有。”逻辑与赋值。**

```
**//These two lines are equivalent:
a &&= b;
a && (a = b);**
```

****逻辑或的语义:****

```
**a || b;
//a when a is truthy
//b when a is not truthy**
```

**“或者或者。”逻辑或赋值**

```
**//The two lines are equivalent
a || = b;
a || (a = b);**
```

****无效合并赋值的语义:****

```
**a ?? b;
//a when a is not nullish
//b when  a is null or undefined.**
```

**无效合并赋值:**

```
**//The two lines are equivalent
a ??= b;
a ?? (a = b);**
```

**用简单的英语说:**

```
**let a = 'Hello world!';**
```

**没有逻辑赋值:a 会不会*永远*是“Hello world！”即使 a 不是无效的:**

```
**a = a ?? 'Hello you!';
//"Hello world!**
```

**对于逻辑赋值:“a”也不会被重新赋值为“Hello you”，因为 a 不是无效的，并且“a”将保持不变。**

```
**a ??= 'Hello you!';
//Hello world!**
```

**在这种情况下，“a”将是“你好！”**

```
**let a;
a ??= 'Hello you!';
//Hello you!**
```

**逻辑赋值遵循其相应操作的短路行为。如果逻辑运算将评估右侧，则它们*仅*执行赋值。**

# **结论**

**前两个提案处于第 4 阶段，我们将很快在规范中看到它们(ES2020？)，而最后一个提案在第 3 阶段，我期待它能被纳入 ES2021。**

**非常感谢你阅读这篇文章！希望你觉得有用。**

# **参考**

**[T39](https://www.ecma-international.org/memento/tc39.htm) 网站。**

## **来自简明英语团队的一封信**

**你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！****

****此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！******