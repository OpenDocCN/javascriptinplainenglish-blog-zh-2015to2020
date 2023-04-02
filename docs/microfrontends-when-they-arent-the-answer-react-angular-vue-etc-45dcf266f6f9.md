# 微前端—当它们不是答案时(反应、角度、Vue 等)

> 原文：<https://javascript.plainenglish.io/microfrontends-when-they-arent-the-answer-react-angular-vue-etc-45dcf266f6f9?source=collection_archive---------1----------------------->

## 看看使用微前端的潜在缺点，以及何时采用微前端没有意义。

![](img/d1e89b8c1750d3eb2f5abf2f4bb3b70d.png)

前端开发已经变得很复杂……在现代 web 应用开发(React/Angular/Vue 等)的世界中，对 HTML、CSS 和 JavaScript 的简单认识已经不足以让你过关。

**微前端**的概念引入了*一种将巨大的单块 UI 分割成更小、更易管理的块的方式*。

**想想前端的微服务。**

然而，这并不是什么灵丹妙药——在这篇以砖块为主题的文章中，我将看看**什么时候不应该**为您的项目考虑微前端。

我已经发表了很多关于微前端作为一种架构设计模式的文章，并讨论了相对于单一 UI 的许多优势。

[](https://medium.com/javascript-in-plain-english/microfrontends-bringing-javascript-frameworks-together-react-angular-vue-etc-5d401cb0072b) [## 微前端——整合 JavaScript 框架(React、Angular、Vue 等)

### 反应，角，Vue，余烬，骨干，模板，预作用…事实上，现在可能又有一部正在上映！

medium.com](https://medium.com/javascript-in-plain-english/microfrontends-bringing-javascript-frameworks-together-react-angular-vue-etc-5d401cb0072b) [](https://medium.com/javascript-in-plain-english/create-micro-frontends-using-web-components-with-support-for-angular-and-react-2d6db18f557a) [## 使用 Web 组件创建微前端(支持 Angular 和 React)

### 如果你是 React 或 Angular，Ember 或 Vue，让我们创建一个地方，让他们都可以生活在一起，在完美的和谐使用…

medium.com](https://medium.com/javascript-in-plain-english/create-micro-frontends-using-web-components-with-support-for-angular-and-react-2d6db18f557a) 

我收到了很多关于微前端作为设计模式的有趣反馈。反馈在很大程度上是积极的，因为许多与微前端试图解决的问题有关。

然而，正如一些人正确指出的，有些情况下采用这样的架构设计模式是没有意义的——让我们看看本文中的一些例子。

# 微前端不会增加构建复杂度吗？

复杂性并不是 UI 开发中的新问题。

以 NPM ( *JavaScript 包管理器*)为例……一个典型的 Python 项目会有一些依赖项，Angular (UI 框架)开箱即用就有 1300 多个！

为了构建一个 UI 项目，你只需**必须**有大量的工具供你使用，来获取、渲染、测试、更新、构建、优化以及其他一切。

![](img/e19e4ae83e5f43984ed38bbc5e53a911.png)

**微前端的引入将复杂性提升到了一个全新的水平，现在您必须管理每个微前端的多个实例，*对吗*？**

不幸的是，这是真的，但有办法减轻这种复杂性。

在我看来，如果没有强大的 CI(持续集成)/ CD(持续交付)流程，你根本无法在微前端上执行。

当你的应用程序扩展成几十甚至几百个小前端(*每个都有自己的构建过程*)时，你将需要标准化一些围绕它们的工具，否则你将陷入痛苦的世界！

坚持一个或几个 UI 框架(例如 Angular / React)可能是一个好的开始，所以所有的构建过程都使用相似的步骤。

Docker / Kubernetes 等集装箱化工具也在简化这种复杂性方面做了大量工作。

我会暂停一下这个想法，并承诺接下来会写一篇关于微前端的可靠 CI/CD 流程的文章。

# 我做一个不太可能成长的小应用，采用微前端有意义吗？

简而言之，这里的答案是明确的*不*。

我经常发现成功采用架构设计模式的团队(*像 Microfrontends* )通过强烈地认同它声称要解决的问题而自然地被它所吸引。

让我们来看看微前端的品质(*类似于微服务*):

*   高度可维护和可测试
*   松散耦合
*   可独立部署
*   围绕业务能力组织

为了成功地采用微前端，您的应用程序应该通过大部分(如果不是全部)改进。

*例如*，如果你独自开发一个没有其他依赖关系的应用程序，那么你就不需要担心松散耦合或独立部署，这样你就不需要战斗了，省下你的精力和热情去做别的事情吧！

![](img/a2a5d74a9ca3a4c363250fa572266efd.png)

# 组件/库是否会在微前端之间重复，从而增加文件大小并对性能产生负面影响？

在您的应用程序可能由单个框架或多个框架的几个实例组成的场景中，在您的整个应用程序中可能会有重复的组件和库，特别是在封装或松散耦合是一个因素的情况下。

以 Angular 为例，它已经与 RxJS 库捆绑在一起。如果您有 5 个基于 Angular 的微前端，那么您可以为您的用户提供 5 个 RxJS 实例。

如何避免这种情况将在很大程度上取决于您的应用程序是如何构建的。在这个例子中，可以延迟加载一些库，并在每个微前端之间共享它们。

在其他情况下，你可能只是不得不接受这种情况，并在做出任何决定时考虑到这一点。

![](img/ff3c35a25d5b16e4dcbad3082783afc6.png)

# 如果我们所有的微前端都是独立的项目，我们如何能够加强设计外观和感觉的一致性？

这是使用微前端最大的顾虑之一。如果用户由于不一致的用户界面设计和/或用户体验(UX)而注意到有多个项目，该怎么办？

谢天谢地，有一些库可以帮助提高一致性。 [Material UI](https://material-ui.com/) 和 [Bootstrap](https://getbootstrap.com/) 是两个比较流行的例子。

这些都为按钮、表格、标题等的外观和行为提供了一致的设计。

从这些中的一个开始作为基线，并在此基础上为您的公司或应用程序构建，这可能是有意义的。

![](img/1bb87b420a0fd3e49d9f0065feddd6b6.png)

就像我们在设计和构建软件时所做的每一个决定一样，在您的应用程序环境中会有优势和权衡。对一个人有意义的东西，不一定对另一个人有意义。

如果我们掌握了事实，这些决定就会更有教育意义，在实施时更有可能成功。

> 感谢您花时间阅读我的文章。