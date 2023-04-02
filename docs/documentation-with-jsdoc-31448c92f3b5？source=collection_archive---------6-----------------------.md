# JSDoc 文档

> 原文：<https://javascript.plainenglish.io/documentation-with-jsdoc-31448c92f3b5?source=collection_archive---------6----------------------->

## 最近，我的团队做了一点重构，决定将我们的页面对象模型和 API 交互功能分解到不同的库中。

![](img/dbb2e2678a26c3139ec8b2170fbddb74.png)

虽然这种抽象允许在我们的步骤定义中有更干净的代码，但它也带来了副作用，即团队成员将库视为一个神秘的功能盒，他们需要通过混淆来找到他们需要的东西。

为了向团队传达什么是可用的，我给自己设定了任务[用 JSDoc](http://usejsdoc.org/about-getting-started.html) 向代码库添加文档，这样就有一个简单的方法来访问代码的分解和它的用途。

# 编写文档字符串

我觉得写文档字符串是一门很难掌握的艺术；编写如下内容通常很容易:

```
/*
* Adds one to the supplied number
*
* @param {number} number - Number to add one to
* @returns incremented number
*/
addOneToNumber(number){
  return number + 1;
}
```

虽然这个 docstring 正确地解释了提供给它的值会发生什么，但它并没有真正解释为什么希望使用该函数的人会使用它。没有关于为什么写它的上下文。

该函数可以被编写为更容易将基于 0 的数组索引转换为第 n 个子选择器索引，并且最初的开发人员给该函数取了一个非常通用的名称。

```
/*
* Adds one to the supplied number to allow nth-child index access
* when selecting elements in a list
*
* @param { number } number - Number to add one to
* @returns incremented number
*/
addOneToNumber(number){
  return number+1;
}
```

随着功能用例的增长，它们也应该被添加到 docstring 中。这使得重构变得更加容易，因为对于从事这项工作的开发人员来说，改变函数的后果是显而易见的。

我个人倾向于让所有的类和函数都有 docstrings，不管它们看起来有多小，多不重要。随着代码的增长，对这些函数的依赖也会增加，在事情变得不可收拾之前，养成尽早更新 docstrings 的习惯是个好主意。

有一个[很棒的 eslint 插件，名为 eslint-plugin-jsdoc](https://github.com/gajus/eslint-plugin-jsdoc) ，它将增加对文档字符串的检查，作为林挺进程的一部分。

最终决定 docstring 是否有用和准确的是那些使用它的人，所以如果你在一个筒仓中构建一个库，让外部的人参与代码评审是很重要的，即使他们只是在那里检查文档是否可用。

# 向文档中添加结构

JSDoc 在第一次运行时的输出很可能是一个类的集合和一个巨大的“globals”部分，尤其是如果您已经有了一个非常实用的代码库。

默认情况下，JSDoc 将所有这些函数都归入一个类别，不管你的文件布局如何，当我以一种非常函数化的方式将 JSDoc 添加到我们的 API 库时，我发现这是一种痛苦。

幸运的是 [JSDoc 有一个](http://usejsdoc.org/tags-module.html) `[@module](http://usejsdoc.org/tags-module.html)` [关键字](http://usejsdoc.org/tags-module.html)，允许你为不同的函数声明一个结构。这需要更多的努力，但是您可以将声明添加到。js 文件和文件中的所有函数都将继承它。

# 添加教程

JSDoc 最好的部分之一是创建教程的能力。这些单独的页面解释了如何使用库的功能，但也可以从 docstrings 链接到，这有助于添加我前面提到的上下文。

要添加教程，请执行以下操作:

*   添加一个包含教程降价文件的文件夹
*   添加解释如何使用该库及其功能的降价文件
*   添加一个 JSON 文件，将元数据添加到 markdown 文件中(比如提供一个清晰的标题或者设置一个教程层次结构)
*   添加一个将 docstring 链接到教程页面的关键字`@tutorial`

然后，您可以使用这些教程来交流团队成员应该如何使用该库，如果他们发现自己在查找某个特定函数是如何工作的，他们可以通过该教程的链接来帮助他们。

# 自动化事物

如果文档不是最新的，没有正确的版本，并且不容易获得，那么它就毫无意义。

幸运的是，我们生活在一个存储廉价的时代，当谈到持续集成解决方案时，我们被宠坏了。

我的团队使用 Jenkins 来运行测试，所以我设置了一个任务来监听对库的 git repo 的主分支的更新，构建文档并在一个设置的 URL 上发布文档。

为了确保我们不会覆盖不同版本的文档，我使用了 J [SDoc 的 package.json 功能](http://usejsdoc.org/about-including-package.html)来发布基于包版本的文档，因为库正在开发这个版本，所以那些使用旧版本的人仍然可以访问准确的文档。

# 最终用户测试

如前所述，如果所提供的文档是有价值的，将使用库和消费文档的人是做出判断的最佳人选。

在从我的团队得到反馈之前，我留出了一些时间对我所采用的文档方法进行了强化，并根据他们的反馈进行了修改。然后，我为他们希望作为开发过程一部分的文档制定了规则。

# 摘要

在总结之前，我认为有几点值得一提:

*   与将要使用你的图书馆的人交谈。如果他们没有从文档中获得任何价值，那么这就是白费力气
*   当编写 docstrings 时，为函数的使用添加尽可能多的上下文，这不仅有助于将来的开发，也有助于那些使用这个库的人
*   JSDoc 有很多非常棒的工具和特性，它们并不明显，如果你想生成文档，花点时间测试一下是值得的
*   在生成包含大量“全局函数”的文档时要小心，要对这些函数进行结构化，这样才能更清楚地知道哪个函数属于哪个模块
*   自动生成文档，但要确保它是有版本的，这样旧版本的文档就不会看到不准确的信息。

## 进一步阅读

[](https://plainenglish.io/blog/5-dev-tools-for-documenting-react-code-like-a-pro) [## 像专家一样记录 React 代码的 5 个开发工具

### React 作为构建复杂前端应用程序的库非常有用，但它也需要大量的工具…

简明英语. io](https://plainenglish.io/blog/5-dev-tools-for-documenting-react-code-like-a-pro) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)***，以及****[***不和***](https://discord.gg/GtDtUAvyhW) **

*****用*** [***电路***](https://circuit.ooo/?utm=publication-post-cta) *为你的科技创业建立认知和采用。***