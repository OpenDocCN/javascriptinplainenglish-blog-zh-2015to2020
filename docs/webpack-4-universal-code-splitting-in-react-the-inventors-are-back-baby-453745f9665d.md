# web pack 4:React 中的通用代码分割

> 原文：<https://javascript.plainenglish.io/webpack-4-universal-code-splitting-in-react-the-inventors-are-back-baby-453745f9665d?source=collection_archive---------1----------------------->

[extract-css-chunks-web pack-plugin](https://github.com/faceyspacey/extract-css-chunks-webpack-plugin)—迄今为止最好的 CSS 分块插件(Webpack 4，独立，SSR 友好，内置 HMR)！

我相信我们都曾遇到过不可避免的问题，“我想在服务器端呈现我的 React 应用，但我也真的想进行代码拆分。”欢迎来到地狱。

我自己，像 React 社区中的许多其他人一样，面临着同样的困境。

我们中的许多人可能一直在等待 React 17 来拯救世界……据开发它的人说，这还有很长的路要走。流泪了，面对现实了😂。

# 我选了什么？

有很多选择，为了确定最佳选择，我列出了一个基本要求的列表。

*   粒度控制& **灵活性**
*   **可扩展**并且与**容器化**架构配合良好
*   **健壮**用于**平台**开发，日常**简单**用于特性开发。
*   能够从**低级 API** 访问详细输出

很难找到，尤其是当你在处理这种新的**微前端架构**热潮的时候。我需要一些架构师和初级开发人员一样热爱的东西。

**我选择了 FaceySpacey 的环球家族。宇宙家庭给了我两个世界最好的东西。**

尽管该解决方案在初始实施阶段更复杂，但从长远来看，它提供了更大的灵活性:

*   你可以在导入里面使用**动态表达式**
*   创建**用户土地 HOCs**
*   **提升机静力学**
*   用**阿波罗**进行双重渲染
*   渲染时利用**回调**
*   **分体式异径管**
*   诸如此类…

**Universal 支持许多其他代码分割包需要变通方法才能实现的特性。**

> Universal 是健壮的，功能丰富的，而其他解决方案是最小的。

[https://web pack . js . org/API/module-methods/# require-resolve weak](https://webpack.js.org/api/module-methods/#require-resolveweak)

# Universal 和 web pack 4——两个版本的故事

在为开发人员建立了一个非常棒的 POC 节点基础设施，并提供了令人惊叹的代码拆分和非常简单的代码拆分方式之后，可以肯定地说，我对自己的小实验非常满意。修补过程中出现的问题，使 Universal 为大规模、复杂的系统做好准备。

但是后来 Webpack 4 发布了。考虑到我们是一个代码分离系统，Webpack 4 几乎重写了所有分块的东西…当我试图更新时，Universal 不工作。**游戏结束了？**

> 通用系列现在支持激进的代码分割！

> 截至 2018 年 6 月 4 日，我们发布了[**babel-plugin-universal-import**](https://github.com/faceyspacey/babel-plugin-universal-import)**的重大更新，现在**支持** **Webpack 4****
> 
> **截至 2018 年 6 月 5 日，我们发布了[**extract-CSS-chunks-web pack-plugin**](https://github.com/faceyspacey/extract-css-chunks-webpack-plugin)**的重大更新，现在**支持 Webpack 4******
> 
> ****截至 2018 年 6 月 12 日，我们发布了**[**web pack-flush-chunks**](https://github.com/faceyspacey/webpack-flush-chunks)的重大更新，使**开发者能够将 Universa** l 与更复杂/ **激进的代码分裂**战术一起使用。******

******那么，我们是如何解决这个问题的呢？******

****差不多，[extract-css-chunks-web pack-plugin](https://github.com/faceyspacey/extract-css-chunks-webpack-plugin)是主要问题…我们需要一个 Webpack 4 解决方案来处理 CSS 分块。****

****我们都知道 Webpack 在未来的某个时候会有更好的 CSS 处理能力。我们不知道什么时候，但似乎没有人说它会很快。所以我们不能指望蒸汽工具****

> ****看看宇宙家庭(从这里开始):[https://github.com/faceyspacey/react-universal-component](https://github.com/faceyspacey/react-universal-component)****

****我肯定你在想，**“就用 mini-css-extract 吧。”是啊。我试过了，它做了代码分割 CSS 的工作，但是没有**没有**热模块重载( **HMR** )…哎哟******

****“但是有一个样式加载器，只是把它用于开发构建！”**
嗯嗯，有那个。但是来吧**

*   **没有源地图**
*   **无法在 chrome devtools 中编辑样式**
*   **向 DOM 中注入字符串**
*   **当追加更多内联代码时，样式覆盖样式。**
*   **声誉好的公司不喜欢这样**

**![](img/11da3e59138dce528a9ead83470df180.png)**

**The use of style-loader**

**在 Fiverr，或者任何一家大公司，我都有一些常识性的目标要实现。**

**在 CSS 的范围内，下面是我想要的:**

*   **I **提高 DX** 和**降低风险**。**
*   **尽量避免本地和生产之间的不一致。**
*   ****避免客户端难以调试的** CSS、额外内存和**变慢**。**
*   **不要干涉 CSS 的层叠效果(保持样式的可继承性)**
*   **改善长期缓存**
*   **加速构建**

**你需要**开发版本**作为**运行，尽可能接近**生产版本。你需要一个良好的端到端系统，经得起高流量的考验。**依赖一个**不太接近生产**的版本**会给你带来**风险**你可能只有在部署了**10 万美元的错误后才会发现。****

**如果 DX(开发者体验)不在它需要的地方，那么一个伟大的系统还有什么意义？我四处寻找如何让迷你 css-extract 热重装。非常坦率地说，我很失望，不是因为 Sokra 的伟大工作，而是因为它只是更多的工作，仍然笨拙。我只是希望它能起作用。说真的，这年头什么不热装了😂**

## **解决方案**

**本质上，我们采用了 mini-css，对其进行了一点修改，并添加了自动 HMR 注入功能。**

## **但是为什么呢？为什么不把它公关回 Webpack 呢？**

**一个合理的问题。我分两部分回答。**

**为什么要用 mini-css-extract 修改呢？**

*   **API 是熟悉的，更少的摩擦&顺便更换。**
*   **写得很好，快速，简单。**
*   **Webpack 的作者直接参与其中。**

**为什么不是 PR Webpack？**

*   **我会的。但这对我们现在的议程来说并不理想。**
*   **我想控制环球公司的架构。**
*   **迷你 css 似乎优先级较低，这是一个权宜之计，直到 Webpack 原生处理 css，他们计划在未来的某个时间点。**
*   **迷你 css 可能会随着时间的推移而改变，我们的用例与议程可能不匹配。**

**FaceySpacey 回来了，事实是他从未真正离开过。他忙着做更令人兴奋的事情。Universal 太棒了，现在有了一个忠诚的团队，我们可以更快地创新，更好地照顾我们忠实的家属，并实现我们改进 React 工作流程的最终目标。**

**其中很大一部分是 [**鲁迪**](https://github.com/faceyspacey/redux-first-router/tree/rudy-respond) (之前的 redux-first-router)**

**鲁迪的任务清单上没剩下多少了！然而，鲁迪只是回应框架*的三个关键部分之一，我们希望它能真正改变 React 的发展。***

***我们只是一个程序员分布的团队。我们对所有这些工作的回报很简单。我们都是程序员，我们只是想制造一些工具，让我们的生活更轻松，反过来，我们也想让你们的生活更轻松。React 很棒，但现在是它的“Rails”时刻了。下一件大事的时间到了。***

***在**推特** [上关注我](https://twitter.com/ScriptedAlchemy)***