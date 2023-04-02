# React 中的测试，第 1 部分:类型和工具

> 原文：<https://javascript.plainenglish.io/testing-in-react-part-1-types-tools-244107abf0c6?source=collection_archive---------5----------------------->

![](img/ff15f9cba8d14339c80486632a604bf0.png)

Photo by [Angelina Litvin](https://unsplash.com/@linalitvina?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](/s/photos/test-taking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 本文是 React 中测试系列的一部分:
> 
> **React 中的测试，第 1 部分:类型&工具**
> 
> [React 中的测试，第 2 部分:React 测试库](https://medium.com/javascript-in-plain-english/testing-in-react-part-2-react-testing-library-f32432b93c6c)
> 
> [React 中的测试，第 3 部分:Jest & Jest-Dom](https://medium.com/javascript-in-plain-english/testing-in-react-part-3-jest-jest-dom-7a8a03ae60b)
> 
> [React 中的测试，第 4 部分:酶](https://medium.com/javascript-in-plain-english/testing-in-react-part-4-enzyme-9b030ad616ae)
> 
> [React 中的测试，第 5 部分:使用 Cypress 的端到端测试](https://medium.com/@bryn.bennett/testing-in-react-part-5-end-to-end-testing-with-cypress-bd2bf8d3385f)
> 
> [React 中的测试，第 6 部分:React 测试库、Jest、Enzyme 和 Cypress 的真实测试](https://medium.com/javascript-in-plain-english/testing-in-react-part-6-real-world-testing-with-react-testing-library-jest-enzyme-and-cypress-9c73436d95d8)

在没有正式测试的情况下编写前端代码非常容易。与后端编程不同，在整个开发过程中，通过与用户界面的交互，您自然会不断地“测试”您的前端应用程序。如果您添加一个 onClick 处理程序，您会直观地在浏览器中单击该组件。当您创建一条新路线时，很可能您之后做的第一件事就是导航到这条路线。

这是一种测试形式，既必要又有帮助。然而，它服务于一个非常具体的目的。这种边做边测试的策略提供了一种确认，即代码库的确切版本中的确切步骤集将产生预期的结果。从这个意义上说，它是快速、高效和有效的，尽管有其局限性，但显然是必要的。

为您的接口编写正式测试有助于填补遗留的漏洞，其中一些是:

*   确保代码*继续*以产生期望的结果，并且不会随着进一步的开发而中断。
*   测试各种用例，而不仅仅是您执行的那个用例。
*   提醒你为什么代码以某种方式编写(处理挑剔的第三方包渲染，或者确保函数以正确的顺序完成，等等)。)

我们都有过这样的经历——你最终为你添加的独特功能找到了解决方案，却发现以那种方式处理会破坏其他东西。这就是正式测试的用武之地。它可以确保你的应用程序继续按预期运行，并且可以帮你省去一大堆麻烦。

# React 中的测试类型

正如 [React 文档](https://reactjs.org/docs/testing.html)所述，React 组件的测试本质上有两种类型——渲染组件树和对应用的端到端测试。前者是特异反应，后者不是。然而，这两者在稳定的应用程序的生产中都很重要，所以我们将在这里讨论这两者。

渲染组件树测试给定组件的输出。编写测试是为了模拟事件或渲染，预期的输出是“断言的”。然后将实际输出(在测试环境中)与断言的输出进行比较，测试通过或失败。

相比之下，端到端测试是在浏览器环境中进行的，从头到尾测试 web 应用程序的整个工作流程。

# React 中的测试工具

就像现在编程中的所有事情一样，你可以选择测试。然而，React 文档中推荐的两个工具是 [Jest](https://jestjs.io/) 和 [React 测试库](https://testing-library.com/docs/react-testing-library/intro)。

其他流行的测试工具包括[酶](https://enzymejs.github.io/enzyme/)、[木偶师](https://developers.google.com/web/tools/puppeteer)和[柏树](https://www.cypress.io/)。Cypress 是专门面向端到端测试的，但是其他的可以单独使用或者结合使用来实现组件和端到端测试。

在这个系列中，我们将介绍 React 测试库、Jest 和 Cypress。

## 反应测试库

这个方便的库适合测试功能，它测试 DOM 节点而不是渲染 React 组件来完成这项工作。 [React 测试库文档](https://testing-library.com/docs/react-testing-library/intro)声明其“主要指导原则是”:

> 你的测试越像你的软件被使用的方式，它们就越能给你信心。

React 测试库是以前流行的酶库的替代品(这就是为什么我们不会在本系列中涉及它)。这里的主要思想是不仅模仿功能性，而且模仿用户体验——不是通过元素 ID 而是通过用户可以识别的任何东西来识别 DOM 节点，无论是输入的标签还是链接的显示文本。通过将测试的重点放在真正的 UX 而不是实现上，您的测试变得更加可靠，并且对未来的代码库更新更有弹性。

我想在这里花点时间说明这是一个*库*，而不是一个*框架*。如果你不熟悉这两者之间的区别，我建议你看看我之前的博客文章，其中有一个章节专门解释这一点。这意味着，你可以将 React 测试库和一个框架结合使用，比如 Jest。

## 玩笑

由脸书开发的 [Jest](https://jestjs.io/) 是一个 JavaScript 测试框架，可以与最流行的 JavaScript 框架一起工作，包括 React。用它自己的话说:

> Jest 是一个令人愉快的 JavaScript 测试框架，专注于简单性。

Jest 通过使用类似于`expect(var).toBe(val)`的语言使得测试 React 组件变得直观和可读。即使没有编程经验的人也知道我们期望 *var* 等于 *val* 。通过向 package.json 文件添加一个简单的脚本，您可以使用`yarn test`或`npm run test`运行这些测试。

这种语言可以集成到 try/catch、async/await、promises 和回调中来测试异步代码。它也可以用在模拟函数中。

此外，Jest 使用快照测试来测试组件是否按预期呈现。通过在您的测试环境中伪创建一个组件，并将其转化为 JSON 树，您可以将该树与应用程序的快照进行比较，以确保它能够正确呈现。

## 柏树

Cypress 是一个端到端的测试框架。Cypress 测试的不是单个组件的渲染，而是 web 应用程序的整个流程。与测试渲染组件相比，这种测试更全面，但在工程工作方面成本也更高。

Cypress 提供了一个完整的仪表板以及调试工具，但值得注意的是，它是一个免费的定价工具。在这里，我不会过多地谈论它，因为这是一个更大的生态系统，当我们可以深入研究时，它会更有意义。

这有望让您了解 React 测试的前景。接下来，我们将深入研究 React 测试库，包括概念和实际实现。

> **接下来:**[React 中的测试，第 2 部分:React 测试库](https://medium.com/javascript-in-plain-english/testing-in-react-part-2-react-testing-library-f32432b93c6c)