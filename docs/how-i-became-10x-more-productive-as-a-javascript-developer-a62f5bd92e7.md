# 作为一名 JavaScript 开发人员，我的工作效率如何提高了 10 倍

> 原文：<https://javascript.plainenglish.io/how-i-became-10x-more-productive-as-a-javascript-developer-a62f5bd92e7?source=collection_archive---------2----------------------->

## 或者质量和自动化的重要性

![](img/0acdd1480ea569d0160323ec87aeb8da.png)

Photo by [krisna iv](https://unsplash.com/@finesite) on [Unsplash](https://unsplash.com/s/photos/productivity)

从我作为一名工程学生开始 React 到现在，发生了很多变化，尤其是我的生产力。同样的结果，我花在编码、交付和修复 bug 上的时间大大减少了。

对于与代码相关的工作中一项任务真正花费了多少时间，有一个普遍的误解。当一个特性处于生产环境中并且没有错误(通常更多)时，它就完成了，而不是当它在开发人员的本地环境中看起来已经完成了。

这就是你如何提高你的生产力，当然是花在编写代码上的时间，但更多的是通过提高部署速度和减少花在错误纠正上的时间。

# 编写代码

是的，通过练习，你会更好地理解和使用你的库、框架和环境。这意味着，更好更快地选择技术和编写代码，但你会很快碰到一个天花板。这时你可能会对一些新的实践感兴趣。

## 读取代码

有趣的是，你写代码的效率与阅读代码密切相关。我不能说我有多喜欢《干净的代码》中的这句话*:[敏捷软件工艺手册](https://amzn.to/2IXCwMz):**的确，阅读和写作的时间比远远超过 10:1。”。***

*那么如何花更少的时间写作呢？从花更少的时间阅读开始。在我看来(根据经验)，有些做法应该是**强制性的:***

*   *类型化 JavaScript:乍看之下，类型化代码会给你更多关于你实际在做什么的信息。*
*   *Linter:许多糟糕的实践会使你的代码可读性更差，任何配置良好的 linter 都会提高你的生产率*
*   *代码格式化程序:和 linter 一样有用，可以防止你写下以后会后悔的代码。*

*此外，有了经验，你可能会进入一个良性循环:写好的代码使它更可读，让你写更好的代码，等等。*

## *在您的 ide 中*

*花在学习如何使用 IDE 上的时间是值得的。一个配置良好的 IDE 会让你的生活更轻松，如果你在启动你最喜欢的 IDE 后不能立即开始工作，可能是有问题。*

*任何人都应该掌握的基本功能是:快捷方式，处理不同的文件和项目，GIT，搜索(和替换)，调试和插件。*

*我们之前讨论过类型，linter 和 formatter。几乎总是有一个插件来利用这些功能。*

*更高级的特性可以是[代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)。很多插件可以为你添加一些，但是我 100%推荐你自己添加你需要的插件。*

## *清晰的结构*

*花时间去寻找一个文件是不可行的，这是我作为一个 React 开发者所经历的困难之一:如何组织你的应用，文件和目录。*

*一个好的开始是尊重[关注点分离](https://en.wikipedia.org/wiki/Separation_of_concerns) (SoC)，也就是将组件与请求、翻译等分开。*

> *关于 React，有很多关于它的讨论，但是[没有完美的解决方案](https://fullstackfeed.com/the-100-correct-way-to-structure-a-react-app-or-why-theres-no-such-thing/) ⁴，以及 [Redux](https://redux.js.org/faq/code-structure) ⁵.*
> 
> *关于文件，我使用的另一个好的实践是避免在同一个目录中有超过 7 个文件和目录。*

## *模板*

*有时，我们会谈到样板文件。当你开始一个新项目时，你是否想一遍又一遍地做同样的事情？*

*然后，也许寻找一个严肃的起步项目或创建自己的项目可以节省你很多时间。*

## *重复发明*

*同样，我们可以获得大量的开源软件包。你曾经需要过洛达什⁶或者⁷吗？也许[深度合并](https://www.npmjs.com/package/deepmerge) ⁸？*

*我们，开发者，喜欢写代码。有时也许太多了，对有用的软件包的良好了解是有价值的。*

*⁹.不要多此一举*

# *错误纠正*

*我们的目标不仅是快速纠正不必要的行为，而且是在第一时间阻止它们。*

## *测试*

*测试驱动开发(TDD)无疑有助于防止不必要的行为。*

*[](https://medium.com/javascript-scene/tdd-changed-my-life-5af0ce099f80) [## TDD 改变了我的生活

### 现在是早上 7:15，客户支持忙得不可开交。我们刚刚上了《早安美国》的专题节目，还有一大堆第一…

medium.com](https://medium.com/javascript-scene/tdd-changed-my-life-5af0ce099f80) 

TDD 本身是一个完整而复杂的主题。当我们在这里讨论生产力时，我会说:

你的生产力会 ***下降*** ，你没听错吧。当你第一次发现测试驱动开发的奇迹时，你会发疯，花更多的时间编写测试，而不是修复 bug。

但是，*好消息，那只是开始。一旦你对单元/集成测试和 TDD 有了一定程度的掌握，你花在编写测试和编码上的时间可能会比以前写代码少。*

*我甚至还没有谈到一个应用程序在生产中的风险。这有多值钱？不仅提高了生产率，而且避免了生产中的错误以及任何倒退。*

> *重要的不仅仅是测试，而是一个好的测试政治。在 TDD 和单元/集成测试之后，您会发现功能、e2e、契约和更多的测试实践。您将了解覆盖率和变异测试。*
> 
> *你能测试一切吗？你测试什么？这些问题需要一个答案。*

## *监视*

*   *如果有不好的事情发生，你意识到了吗？*
*   *如果你意识到了，你知道为什么会发生吗？*

*或者你花几个小时寻找一个拼写错误的属性？这时监控就派上用场了。*

> *每次你写“console.log”的时候，有人误删除了一个生产数据库。*

*外面有很多监控工具。其中，[哨兵](https://sentry.io/welcome/)， [datadog](https://www.datadoghq.com/) ， [dynatrace](https://www.dynatrace.com/) 或 [splunk](https://www.splunk.com/) ⁴.根据我的经验，sentry.io 已经足够了，但是你可能还需要其他的东西，这取决于你的需求。*

# *流程和部署*

*我会考虑，在今天这个时代，你在用 GIT。*

*首先，定义良好的过程/结构是必须的。您应该能够轻松地作为一个团队工作，并且没有任何麻烦地进行部署。我使用 GitFlow 已经有几年了，我从未离开过它。*

*[](https://medium.com/@muneebsajjad/git-flow-explained-quick-and-simple-7a753313572f) [## Git 流程解释:快速简单

### 嗨，我希望你们都做得很好，你们可能想知道为什么另一篇 gitflow 文章已经有很多文章了…

medium.com](https://medium.com/@muneebsajjad/git-flow-explained-quick-and-simple-7a753313572f) 

前面，我们讨论了使用 linter、格式化程序和测试。这些将帮助您验证您的应用程序可以使用两种工具部署:GIT 挂钩和管道。

## GIT 挂钩

[](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) [## Git - Git 挂钩

### 像许多其他版本控制系统一样，Git 有办法在某些重要动作发生时触发定制脚本…

git-scm.com](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) 

我使用 git 钩子是为了运行快速便宜的脚本。这不应该困扰开发人员(并使他们想绕过它)。

在提交或推之前至少跑得更快更漂亮是一个不浪费时间的好方法。

## 代码审查

[](https://www.atlassian.com/agile/software-development/code-reviews) [## 为什么代码审查很重要(并且实际上节省了时间！)

### 敏捷团队是自组织的，技能集跨越整个团队。这部分是通过代码实现的…

www.atlassian.com](https://www.atlassian.com/agile/software-development/code-reviews) 

根据项目的不同，代码审查被设置为 GitFlow 的一部分。我们并不是真的通过做代码审查来赢得时间，我们避免了更多的损失！

这些评论给出的反馈是有价值的，因为新的视角可以快速帮助识别新的问题。它们也是传递知识的好地方。

## 管道和质量

[](https://docs.gitlab.com/ee/ci/pipelines/) [## CI/CD 管道

### GitLab 8.8 中引入。管道是持续集成、交付和部署的顶级组件…

docs.gitlab.com](https://docs.gitlab.com/ee/ci/pipelines/) 

管道也是一个复杂的问题。主要的有 [GitLab CI/CD](https://docs.gitlab.com/ee/ci/) ⁷、 [CircleCI](https://circleci.com/) ⁸和 [Travis CI](https://travis-ci.org/) ⁹.

我用 GitLab 很久了，绝对喜欢！这条管道是我战略的重要组成部分:

*   作为 GitFlow 一部分，为每个任务创建分支。单元/集成测试是用 eslint 和更漂亮的软件运行的
*   一旦在我的开发分支上合并，这些测试将在新版本上重新运行，带有 e2e 测试。
*   一旦在发布时合并，根据项目的不同，这些测试将在生产环境的副本上运行。
*   然后，它们在母版上合并并标记。

## 自动发布

谁负责从 master 创建标签？你如何决定它是否值得创建，你给这个标签取什么名字？

标签可以伴随着一个[释放](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/about-releases) ⁰.同理，谁负责创建它和它的内容？

嗯——有一些工具可以帮你处理，比如[语义发布](https://github.com/semantic-release/semantic-release)和[标准版](https://github.com/conventional-changelog/standard-version#readme)。我最喜欢的是语义发布，它自动化了你的整个发布过程。

> **语义发布**自动化整个包发布工作流，包括:确定下一个版本号、生成发布说明和发布包。

## 部署

不仅测试在我的管道上运行，而且一旦成功，我的应用程序会以不同的方式部署:

*   在开发时，应用程序部署在廉价的开发环境中。
*   发布时，该应用程序部署在接近生产的环境中。
*   最后，标记完成后，应用程序将被部署到生产环境中。

# 结论

这个过程允许我和我的团队开发/部署高质量的应用程序，同时避免错误和时间损失。

最终，所有这些实践都是相互关联的。增加的可读性不仅减少了编写时间，还减少了 bug(这意味着更少的部署)。

这些实践不仅会帮助你提高效率，还会让你变得更有价值，因为它们有助于避免昂贵的不必要的行为。

> 感谢阅读。

[](https://morintd.medium.com/about-me-teddy-morin-9fb1d65fe24e) [## 关于我——泰迪·莫林

### 嗨，我是泰迪，一个反应的爱人🚀。

morintd.medium.com](https://morintd.medium.com/about-me-teddy-morin-9fb1d65fe24e) 

*   **【1】干净的代码:敏捷软件工艺手册:**[https://amzn.to/2IXCwMz](https://amzn.to/2IXCwMz)
*   **【2】Visual Studio 代码中的片段:**[https://Code . Visual Studio . com/docs/editor/userdefinedspenets](https://code.visualstudio.com/docs/editor/userdefinedsnippets)
*   【https://en.wikipedia.org/wiki/Separation_of_concerns】【三】分离顾虑:
*   **【4】构造 React app 的 100%正确方法(或者为什么没有这样的东西):**[https://fullstackfeed . com/The-100-correct-way-to-structure-a-React-app-or-why-there-no-this-thing/](https://fullstackfeed.com/the-100-correct-way-to-structure-a-react-app-or-why-theres-no-such-thing/)
*   **【5】代码结构| Redux:**[https://redux.js.org/faq/code-structure](https://redux.js.org/faq/code-structure)
*   **【6】洛达什:**[https://www.npmjs.com/package/lodash](https://www.npmjs.com/package/lodash)
*   **【7】react-hook-form:【https://react-hook-form.com/】T22**
*   **【8】深度合并:**[https://www.npmjs.com/package/deepmerge](https://www.npmjs.com/package/deepmerge)
*   **【9】多此一举:**[https://en.wikipedia.org/wiki/Reinventing_the_wheel](https://en.wikipedia.org/wiki/Reinventing_the_wheel)
*   **【10】TDD 改变了我的人生:**[https://medium . com/JavaScript-scene/TDD-Changed-My-Life-5a f0ce 099 f 80](https://medium.com/javascript-scene/tdd-changed-my-life-5af0ce099f80)
*   **【11】应用监控和错误跟踪软件|哨兵:**[https://sentry.io/welcome/](https://sentry.io/welcome/)
*   **【12】云监控即服务| Datadog:**【https://www.datadoghq.com/ 
*   **【13】云监控的领军人物| Dynatrace:**[https://www.dynatrace.com/](https://www.dynatrace.com/)
*   **【14】为云构建的数据对一切的平台| Splunk:**[https://www.splunk.com/](https://www.splunk.com/)
*   **【15】Git—Git 挂钩:**【https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks】T2
*   **[16]为什么代码审查很重要(而且实际上节省时间！):**[https://www . atlassian . com/agile/software-development/code-reviews](https://www.atlassian.com/agile/software-development/code-reviews)
*   **【17】CI/CD 管道| git lab:**[https://docs.gitlab.com/ee/ci/pipelines/](https://docs.gitlab.com/ee/ci/pipelines/)
*   **【18】持续集成和交付—circle ci:**[https://circleci.com/](https://circleci.com/)
*   **【19】Travis CI—满怀信心地测试和部署您的代码:**[https://travis-ci.org/](https://travis-ci.org/)
*   **【20】关于发布—GitHub:**[https://docs . GitHub . com/en/free-pro-team @ latest/GitHub/administrating-a-repository/About-releases](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/about-releases)
*   **【21】语义释放:**[https://github.com/semantic-release/semantic-release](https://github.com/semantic-release/semantic-release)
*   **【22】标准版:**[https://github.com/conventional-changelog/standard-version](https://github.com/conventional-changelog/standard-version#readme)**