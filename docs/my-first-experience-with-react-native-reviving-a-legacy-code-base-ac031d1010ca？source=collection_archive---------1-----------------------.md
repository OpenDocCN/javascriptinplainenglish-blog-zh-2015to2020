# 用 React Native 恢复遗留代码库

> 原文：<https://javascript.plainenglish.io/my-first-experience-with-react-native-reviving-a-legacy-code-base-ac031d1010ca?source=collection_archive---------1----------------------->

![](img/33d65b3a6a6632bf5c6574605422f513.png)

Photo by [Richard Catabay](https://unsplash.com/@illest_shinobi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在过去的几个月里，我一直在帮助一个 Pokemon Go 社区组织开发他们基于 React Native 的移动应用程序，该应用程序已经有一年没有在上面进行积极的开发了，所以代码库需要一点 TLC。

我有机会开发这个应用程序，因为我有 React 的经验，我的一个朋友建议他们和我谈谈，但这是我第一次使用 React Native，我总是乐于学习新的东西。

当我继承遗留代码时，特别是当它是一个 JavaScript 项目时，我必须做的第一件事就是让项目构建并运行。

# 反应本地工具

![](img/201624a106a055973bcf129f8198b204.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React Native 有一个 CLI，分别使用`react-native run-ios`和`react-native run-android`这样的命令可以相对容易地做到这一点，但是我遇到了一个问题，因为自上次创建应用程序以来，iOS 和 Xcode 一直在更新，我需要更新依赖项才能与新版本一起工作。

对 React 原生框架的一次简单更新导致了许多需要更新的依赖项，但最终一切似乎都井然有序，我可以开始编写代码了。

当我使用 React Native 而不是一些旧的“本地应用的 web 技术”技术时，最大的(令人愉快的)惊喜之一是 Metro bundle server，它允许您只需刷新即可看到应用的变化，而不必重新编译应用——这真的加快了开发速度。

此外，React Native 的调试工具与 web 开发人员习惯使用的工具相同，即 Chrome developer tools 和 Chrome developer tools 扩展的独立版本`react-devtools`。

这些工具允许您查看 React 组件结构以及组件的 JavaScript，这使得弄清楚发生了什么变得轻而易举。

此外，React Native 创建 Xcode 和 Gradle 项目，因此您可以使用 Xcode 和 Android Studio 来编译和运行应用程序，并调试本机级别的功能。

不幸的是，我无法与 Expo 合作，我知道 Expo 使事情变得容易得多，但该项目被“驱逐”,这意味着 Expo 不再管理应用程序的构建周期，因为已经添加了自定义的原生功能。

# 学习代码库

在我让应用程序运行之后，下一步是更好地理解代码库。

之前的开发团队在将应用程序组织成不同的模块(动作、减速器、组件、屏幕等)方面做得很好，但组件并没有像它们应该的那样模块化，也没有测试和有限的文档。

我认为最好的办法是浏览整个代码库，并在每一处添加文档。

这将允许我弄清楚每件事都在做什么，并留下重构笔记供我以后继续学习。

## 生成文档

![](img/ddd590c9b8b88e3229088662980f675f.png)

Photo by [Ed Robertson](https://unsplash.com/@eddrobertson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

虽然添加文档字符串相对容易，但是将这些文档字符串转换成可用的文档站点稍微困难一些。

我最初从`jsdoc`开始，因为我过去一直在使用它，但是代码库中使用的模块结构似乎混淆了`jsdoc`，我不得不使用`better-docs`来手动定义模块结构。

然后我用`esdoc2`和它提供的插件进行了实验。这工作得很好，因为它选择了模块结构，并包含了很好的工具，如文档覆盖报告，并允许单元测试用例与功能相关联。

我最终决定使用`jsdoc`,因为我遇到了`esdoc2`的一个问题，它从我无法正确配置的类中省略了许多公共方法——这可不是你想从文档生成器中看到的！

## 添加测试

![](img/7f4a8b0fbab4d91db225510de22ad02c.png)

Photo by [Nicolas Thomas](https://unsplash.com/@nicolasthomas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

由于代码库没有测试，我开始添加一些。

编写测试是迄今为止理解一个函数或类正在做什么的最好方法，之前的开发团队在他们的待办事项中有许多标签来添加测试，这允许我学习这些标签的代码和标记。

该应用程序使用 redux，所以我开始进行单元测试，测试内容包括 action 和 reducers，因为当与 redux 的愿景内联实现时，这些都是没有副作用的纯函数，这使得它们非常容易测试。

一旦我为应用程序中的状态管理编写了测试，我就可以开始为改变状态的函数编写测试，然后为显示状态数据的组件编写测试。

由于 React 本机组件只是组件，因此可以采用与 React 组件相同的方法来测试它们，例如快照测试和浅层渲染。

## 项目管理

![](img/19d1a4cfb827cce30925ed930f04e6cb.png)

Photo by [Kyle Glenn](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为一名开发人员，我发现继承项目最容易被忽视的一个方面是项目管理&代码库的产品开发方面。

作为一名开发人员，当面对一个庞大的代码库时，很容易全神贯注于代码，以至于我们无法思考最初为什么以及如何编写代码。

在继承代码库的早期，我和一个人开了一次会，这个人让我负责理解哪些未完成的拉请求(有八个)可以被删除，我们能够将回购上的特性分支数量减少到只有两个。

这使事情变得更容易，因为移动的部分更少，更重要的是，我可以看到开发团队在工作中有什么变化，当他们关闭工具时，因为有可能最终这些变化会使我正在做的一些重构变得多余。

我们还进行了一次待办事项梳理会议，这样我就可以了解哪些功能是重要的，就好像他们计划很快删除或重写一些东西，这样我就可以让自己不要太专注于应用程序的这个领域。

这些会议和测试实际上是最有用的，因为我对以前的开发团队留下的事情有了一个全面的了解，并且我知道应该在哪里集中精力。

# 从反应移动到反应本地

React Native 的目标之一是将 web 组件技术引入原生开发领域，使构建原生应用变得快速而简单，我敢说它绝对达到了这个目标。

除了最终只是在模拟器或设备上协调应用程序外壳的编译和运行的附加工具之外，构建应用程序的工作流程没有太大的不同。

当您使用组件时，可以(也应该)使用相同的设计模式，例如纯组件和高阶组件，并且可以使用工具，例如 [Storybook.js，使团队之间的交流和共享理解更加容易](https://storybook.js.org/)。

事实上，我的下一步是将许多复杂的组件分解成更小的纯组件，这些组件将形成一个导入主应用程序的组件库，这样代码库更清晰，而且这些纯组件可以在其他地方重用。

我发现这种模式在大型 React 应用程序中非常有效，因为一个团队可以专注于应用程序数据结构的组成，而另一个团队可以专注于数据和用户旅程的显示。

Redux 正在应用程序中使用，所以我很可能会很快对它进行重构，以包括 Redux Saga，因为应用程序中有大量的副作用(例如，将状态写入本地存储)可以使用 Sagas 抽象出来，我发现 Redux Saga 使这些副作用非常容易测试。

# 后续步骤

这可能会是我未来几个月甚至几年博客中的一个不断发展的话题，因为我还有很多关于 React Native 和 also React 的内容要学。

到目前为止，接下来的步骤是重构代码库，以允许创建纯组件库，添加 Redux Saga，并设置一个持续集成服务器来运行测试，作为开发工作流的一部分。

敬请关注 React Native 的更多见解！