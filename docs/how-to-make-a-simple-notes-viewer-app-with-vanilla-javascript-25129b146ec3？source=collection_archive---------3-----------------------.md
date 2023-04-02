# 如何用普通的 JavaScript 制作一个简单的笔记查看器应用程序

> 原文：<https://javascript.plainenglish.io/how-to-make-a-simple-notes-viewer-app-with-vanilla-javascript-25129b146ec3?source=collection_archive---------3----------------------->

## 学会编码

## 对于那些刚开始使用 JavaScript 的人来说

![](img/fe0081ebb5bbfb99f709fd3dc601088d.png)

Photo by [Reinhart Julian](https://unsplash.com/@reinhartjulian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/studying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

今天一位同事问了我的建议。他想显示`json` 文件中的注释。他还想过滤和分类笔记。他告诉我，他可能想用手柄来做模板，而且他已经在使用 jQuery 了。

> -也许我应该使用类似`lodash`的东西？你能推荐一些简单的解决方案吗？——他问道。

我的同事不是 JavaScript 开发人员。他想为自己建造一些有用的东西。JavaScript 通常是压倒性的，尤其是对于那些刚刚起步的人。

本文将允许您用普通的 JavaScript 编写一个简单有用的应用程序。不`handlebars`、不`jquery`、不`lodash`。我认为，如果您刚刚起步，就不需要这些库——先学习普通的 JavaScript。

您可以在沙箱中使用生成的 notes 应用程序(最后的链接)。或者您可以通过[查看本次](https://github.com/baranovxyz/simple-notes) GitHub 回购。

# HTML 文件

首先，让我们制作尽可能简单的`html`文件。

当浏览器加载该文件时，它从`src/styles.css`请求样式表，并从`src/script.js`文件请求和执行 JavaScript。

# 脚本文件

我们的脚本文件放置在 HTML 中`body`标签的末尾。运行时`body`已经有了`input`元素和 id 为`notes`的`div`。

我们将在 id 为`notes`的`div`中呈现注释。为此，我们需要用`const notesEl = document.[querySelector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)('#notes');`获取 notes 元素。不需要`jquery`。现在我们有了对 notes 元素的引用。我们可以用内置的方法`notesEl.[innerHtml](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)`来代替它里面的东西。

为了渲染音符，我们使用了`render`函数。这很简单。我们`map`越过`notes`，对于每一个我们一起返回`html`字符串和`join`。不需要映射数组，我们使用内置的映射方法。

我们在上面的代码中使用了函数`Note`。它得到一个注释并返回一个`html`字符串。这是一个非常简单的模板函数。不需要`handlebars`。

为了获取笔记，我们使用本地的`[fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)` api。这是一个简单的应用程序，所以我们只取笔记一次——当脚本加载时。`fetch` api 返回一个承诺，我们从中获取 json 并赋给`notes`变量。

为了添加过滤，我们用`const filterEl = document.querySelector('#filter');`获取过滤输入的引用。我们监听这个元素上的`keyup`事件。对于每个事件，我们执行`render`功能`filterEl.addEventListener('keyup', render);`。

现在我们修改`render`函数来使用来自过滤器`input`的文本。如果过滤器`input`为空，我们显示所有注释。如果它有一些文本，我们只显示标题或文本`[includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes)`过滤文本的注释。

现在当你在`filter`输入栏输入时，你会看到实时更新。

# 样式文件

为了设计我们的应用程序，我们使用 css。从模板函数`Note`中可以看到，每个笔记标题都有类`Note-Title`，每个笔记文本元素都有类`Note-Text`。我们可以使用类选择器来设计它们的样式。

***就是这样！现在我们有了一个带有基本样式的工作应用程序。***

我们没有在这个应用中实现排序。试着在下面的沙盒里自己做。

> **如果您遇到问题:**在任何搜索引擎中键入`*mdn your question*`。从 MDN 开始，而不是从 StackOverflow 开始。阅读完 MDN 文档后，您将获得最新的解决方案。在 StackOverflow 上阅读后，您可能会得到过时的问题解决方案。

你可以在下面的沙箱里玩代码。