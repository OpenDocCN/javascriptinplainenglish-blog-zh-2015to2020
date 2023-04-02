# Cypress 和移动应用？

> 原文：<https://javascript.plainenglish.io/easy-mobile-app-automated-tests-509e9cde311f?source=collection_archive---------3----------------------->

![](img/5a4e9d68e0cd746328512f6eb5b41736.png)

What mobile app testing feels like. Ow. (Pen and paper? Seriously? :) ) Photo by [freestocks](https://unsplash.com/@freestocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/testing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

设置移动应用的自动化测试是很棘手的。也许你在做一个小项目，而你的 Jest + Enzyme 单元测试没有给你想要的 ROI。也许你想测试网络错误场景或时间敏感的逻辑，但是在你当前的应用框架中没有这样做的方法。你在 web 开发领域的朋友告诉你关于 Cypress 的事情，但是他们没有一个 React-Native 移动应用程序可以测试。

有一种方法可以弥补这个差距，并且不需要重新构建你的应用。在这篇文章中，我展示了如何将最好的 web 应用程序测试应用到 React-Native 移动应用程序中，并提供了一些技巧和窍门。

# 柏树

(Cypress 是一个面向开发的 **web-app** 端到端集成测试工具。在[的 Cypress.io 文档](https://docs.cypress.io/guides/overview/why-cypress.html)中阅读更多信息

在为 web 应用程序选择 e2e 测试解决方案时，我们会面临这样的问题“我应该选择基于 Selenium 的工具，还是应该选择 Cypress？”这是一个错误的二分法——尽管他们都测试 web 应用程序，但他们解决不同的问题。**听听 Cypress.io docs 在 FAQ 板块给出的答案:**

> … Cypress 可能无法在您不做任何改变的情况下为您提供 100%的覆盖，但这没关系。使用不同的工具测试应用程序中不太容易访问的部分，让 Cypress 测试其余的 99%。([来自 Cypress.io 常见问题](https://docs.cypress.io/faq/questions/general-questions-faq.html#We-have-the-most-complex-most-outrageous-authentication-system-ever-will-Cypress-work-with-that))

如果你是帕累托法则(或多或少“20%的努力，80%的结果”)的粉丝，你会开始看到柏树的吸引力。如果您的需求要求最后 20%的结果(跨平台/跨浏览器、跨来源、多标签等)。)，没有人阻止你拿起硒来覆盖赛普拉斯无法解决的案件。(我发现软件测试与经济和投资回报率的关系比软件更大，但是[那是另外一篇文章](https://medium.com/javascript-in-plain-english/testing-react-hooks-be74ec3fc0c4)

**简而言之:Cypress 是关于从你的测试中获得更多的 ROI。**

(更不用说 Cypress 解锁的强大嘲讽能力——[个人非常喜欢网络请求-响应嘲讽功能](https://medium.com/javascript-in-plain-english/mocking-request-body-in-cypress-4592c971231f))

如果在测试您的移动应用程序时，您也能获得同样的理念和能力，那不是很好吗？

# 反应本地 Web

还记得帕累托原理吗？它是 React 本地项目的核心。编写一次应用程序，获得 2 个平台的大部分结果:iOS 和 Android。React Native Web 又增加了一个平台:浏览器。React 本机 Web“技巧”使用 react-dom 将本机应用程序反应为在浏览器中呈现。当然，并不是所有的移动 API 都可以在 Web 中实现，但是看看[这个令人敬畏的兼容性表](https://github.com/necolas/react-native-web#compatibility-with-react-native)——你可能会惊讶于它的翻译量。还是那句话，20%的努力，80%的结果:帕累托法则。

有 React 原生 app 吗？现在你可以在浏览器**中运行它，并用 Cypress** 测试用户流量。手机 app，满足稳定、经济、强大的高水平自动化测试。

“这是否意味着我也必须维护一个完整的 web 版本”，你问？"我的意思是，维护所有这些屏幕尺寸和操作系统已经够困难的了！"我完全同意。但是，请记住帕累托原则:*你不必为了获得大部分结果而全力以赴*。考虑一下关于你的新网页版本的想法:

*   **它不必见天日**。顾客永远不需要知道它的存在。把它当成你、你的应用和 Cypress 之间的秘密。
*   **不一定要漂亮**。它只需要基本上工作。
*   **不一定要全功能奇偶性**。但是，您可以在网页版中保留的功能可以进行测试。

同样，就像在“Selenium vs. Cypress”问题中一样，**它不必完全取代您现有的工具**。使用 Cypress 来测试和模拟您的功能边缘案例和流程，如果需要的话，使用其他工具来覆盖您的特定于平台的功能。

# 我的证明

一年前，我将我的副项目应用重构为 3 个项目:1 个共享 redux 项目、一个 Expo.io 应用和一个 knee cap web 应用。我厌倦了通过 React Native Web 发布带有 beta web 支持的 Expo 版本 33(是的，我记得发布号),我很激动。不久之后，我有了一个 web 和移动代码库。(这是一个附带项目，所以我还是部署了网络版。)

虽然我不得不放入一些特定于平台的逻辑来禁用一些功能，但我能够解锁 Cypress 测试。对于一个副业项目，传统的应用程序测试框架非常复杂/昂贵，Jest 单元测试只能做到这一步，所以这是自动化测试的游戏规则改变者。

# 片段

您会发现这些 package.json“脚本”很有帮助:

```
{
  // ...
  "web:start": "expo start --web-only",
  "web:e2e:ci": "cypress run",
  "web:e2e": "start-server-and-test web:start 19006 web:e2e:ci",
  // ...
}
```

(您可以在持续集成或每夜构建中使用`yarn web:e2e`)

Expo web 应用程序是渐进式的 web 应用程序，一些浏览器端的资产缓存使用服务工作者。虽然这是一个很酷的特性，但是这种缓存有时会干扰您的自动化测试。请尝试使用此代码片段。

clear-service-workers.js, be sure to import this in your cypress/support/index.js file!

你的应用也依赖位置 API 吗？试着模仿地理定位 API。

Add this to your cypress/support/commands.js file, use like `cy.mockGeolocation(0, 0)`.

# 是时候来点柏树超能力了

时间旅行？(其实只是在浏览器里戳时间功能)查看 [cy.clock()](https://docs.cypress.io/api/commands/clock.html) 。

控制互联网？(实际上，只是在浏览器中掐灭 XHR 的请求)Cypress 有[一个伟大的指南](https://docs.cypress.io/guides/guides/network-requests.html#Stub-Responses)，我有[一篇有用的文章，以防你在形成回应](https://medium.com/javascript-in-plain-english/mocking-request-body-in-cypress-4592c971231f)时需要更多的力量。给自己留一天时间自己想办法解决吧！

# 那太好了，但是我希望我知道更多关于测试的知识

我有一个坏习惯，就是在看似具体的文章里教的太多。我关于测试 React Hooks 的文章讲述了自动化测试的哲学和斗争，以及你在单元测试框架中获得的一些能力。

嘿，你已经读到这里了。不妨。

[](https://medium.com/javascript-in-plain-english/testing-react-hooks-be74ec3fc0c4) [## 测试反应挂钩

### 嗯……一般的测试。相信我，图像是有道理的！

medium.com](https://medium.com/javascript-in-plain-english/testing-react-hooks-be74ec3fc0c4)