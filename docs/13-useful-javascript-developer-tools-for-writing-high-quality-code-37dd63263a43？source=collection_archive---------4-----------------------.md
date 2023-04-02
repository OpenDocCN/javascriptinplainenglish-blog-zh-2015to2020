# 编写高质量代码的 13 个有用的 JavaScript 开发工具

> 原文：<https://javascript.plainenglish.io/13-useful-javascript-developer-tools-for-writing-high-quality-code-37dd63263a43?source=collection_archive---------4----------------------->

## 让我们创造一个人人都喜欢继承的遗产。

![](img/fcf1877b942f14512307d03c0647db86.png)

Photo by [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天，我将向您展示 13 个众所周知的流行工具，它们可以编写更好、更简洁的 JavaScript 代码。

这些是我现在在 JavaScript 开发者之旅中使用的工具。

因此，如果你想提高你的项目质量，这个列表是给你的。

# 1.npm

[NPM](https://www.npmjs.com) 是 Nodejs 的一个包管理器，里面有几十万个包，你也可以用它来做前端开发。

使用 NPM 的主要目的是自动化依赖和包管理。如果您通过直接导入库而不使用 NPM 来使用库，那么每次您想要更新库时都要这样做。随着项目的增长，这是一项非常耗时的工作。不要把时间花在无价的任务上，只需安装一次 NPM，它就会为你处理所有的事情。

# 2.JSFiddle

当我为我的项目想出一个新主意时，我会在 [JSFiddle](https://jsfiddle.net) 上测试一下，看看一切进展如何。对于在向真实项目添加东西之前的快速测试，JSFiddle 是完美的解决方案。

更多 JavaScript 在线编辑器:

[](https://medium.com/javascript-in-plain-english/25-javascript-playgrounds-for-testing-new-ideas-de8735d963a7) [## 测试新想法的 25 个 JavaScript 平台

### 测试你的新想法，甚至开发整个产品。

medium.com](https://medium.com/javascript-in-plain-english/25-javascript-playgrounds-for-testing-new-ideas-de8735d963a7) 

# 3.Chrome 开发工具

这个工具我不需要多说。在它的帮助下，您可以检查项目的几乎所有内容，以便您可以调试并知道某个页面上有什么问题。

关于 Chrome DevTools 的更多信息:

[](https://medium.com/javascript-in-plain-english/13-super-useful-chrome-devtools-tips-to-speed-up-your-developing-workflow-e9ecb60fda5c) [## 13 个超级有用的 Chrome DevTools 技巧来加速你的开发工作流程

### 一套很棒的 web 开发工具。

medium.com](https://medium.com/javascript-in-plain-english/13-super-useful-chrome-devtools-tips-to-speed-up-your-developing-workflow-e9ecb60fda5c) 

# 4.自动预混合器

[Autoprefixer](https://github.com/postcss/autoprefixer) 是一个 CSS 后处理器。它解析 CSS 文件，并使用[caniuse.com](http://caniuse.com)数据库为 CSS 规则添加供应商前缀。在编写 CSS 代码时，您不必编写任何供应商前缀。Autoprefixer 会帮你搞定一切。

你所要做的就是编写常规的 CSS 代码，比如:

```
.my-box {
  box-sizing: border-box;
}
```

让 Autoprefixer 完成它的工作:

```
.my-box {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
```

不想干脏活？将 Autoprefixer 添加到您的列表。

# 5.多科

我必须强调，每个项目都必须有一个文档，我们有时会跳过这个文档。

考虑到这一点，为了简化创建文档的过程， [Docco](http://ashkenas.com/docco/) 就是为您准备的。它是一个快速而肮脏的文档生成器，可以获取您的源代码并创建带注释的文档。

如果你想为某样东西创建文档，你所要做的就是写描述性的注释，Docco 会为它们生成一个清晰的布局。

想要一个使用 Docco 的结果？查看[这一页](http://ashkenas.com/docco/)。

# 6.巴比伦式的城市

[Babel](https://babeljs.io) 是一款 JavaScript 编译器。它用于将 ES6 代码转换成不同环境下兼容的 JS 版本。

不是所有东西都支持 ES6。如果你使用像 React 这样的现代语法，你需要有 Babel 在手。

我们希望有一天所有的浏览器都支持 ES6。但是现在，为了确保一切都完美运行，您的项目中不能缺少 Babel。

# 7.Visual Studio 代码

VS Code 是我用过的最好的代码编辑器。根据 Stackoverflow 2019 年开发者调查，它是最受欢迎的编辑器， [50.7%的受访者使用它](https://en.wikipedia.org/wiki/Visual_Studio_Code#cite_note-StackOverflow2019-10)。

此外，VS 代码附带了许多优秀的插件，让你的开发更加容易。

[](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) [## Visual Studio 代码的 9 大 JavaScript 扩展加速您的开发

### 谁想更快更容易地编码？

medium.com](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) 

# 8.代码战争

有抱负的开发人员定期练习。除此之外别无他法。

练习 JavaScript 技能(和其他编程语言)的最好地方之一是 [Codewars](https://www.codewars.com) 。这是一个提供编程挑战的网站，叫做“形”来提高你的技能。

所以是时候不找借口，尝试一些形了。

# 9.咕哝

Grunt 是一个构建在 NodeJS 之上的 JavaScript 任务运行器。那么，为什么要使用 Grunt 呢？

如果你和我一样，你不喜欢重复的任务。他们很无聊，效率也不高。所以，把它留给 Grunt 之类的东西吧，这个工具将帮助你以最小的努力自动完成重复的任务，让你有更多的时间关注真正重要的事情。

一旦你为这个项目完成了你的 Gruntfile.js，你的生活会变得容易得多。

# 10.玩笑

由脸书创建的 [Jest](https://jestjs.io/) 是一个令人愉快的 JavaScript 测试库，也是目前最受欢迎的测试程序之一。反应过来就更有用了。

不用说，测试是开发过程中最重要的部分之一。对于我的团队中不经过测试就编写代码的人来说，这是一种耻辱。幸运的是，我的团队成员喜欢编写测试，因为他们一开始加入团队时就有了正确的心态。

# 11.Snyk

在开发项目时，安全性应该是重中之重。我们热爱开源，但我们也不能百分之百确定我们使用的库在安全性方面是否安全。在没有检查漏洞的情况下导入一个库是非常危险的，因为黑客可以利用它来攻击您的应用程序。

为了防止对你的网站的危险攻击，最好的解决方案之一是使用 Synk。它将帮助您找到并修复开源库中的漏洞。

# 12.埃斯林特

网址:【https://eslint.org T3

每个程序员都喜欢干净的代码。在一天结束的时候，你想对你写的每一行代码微笑。当然，你也不想从别人那里继承乱七八糟的遗产。

通过使用 ESLint，您可以使您的源代码更加一致。通过遵循 lint 规则，即使团队中有许多开发人员，并且每个人都有自己的编码风格，项目也会看起来像是由一个人编写的。

# 13.PageSpeed 洞察

谈到绩效审计， [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/) 就是你需要的工具。它旨在帮助优化您的网站性能。

你肯定希望你的网站越快越好。PageSpeed Insights 将向您展示您网站的性能指标，以及您可以用来修复网站的建议。

# 结论

所以你现在有了我的清单:13 个写高质量代码的工具。

您现在正在使用它们吗？或者它们中的任何一个对您来说都是新的吗？

或者你可能使用了一些不在列表中的工具。如果你能在下面的评论中留下你正在使用的工具，我将不胜感激。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a) [## 每个 Web 开发人员都应该知道的 11 个 JavaScript 概念，让他们的技能更上一层楼

### 不了解这些概念，就无法掌握 JavaScript。

medium.com](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a) 

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**