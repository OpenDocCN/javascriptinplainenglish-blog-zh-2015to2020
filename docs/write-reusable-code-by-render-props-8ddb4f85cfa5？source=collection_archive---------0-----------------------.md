# 使用{render.props}在 React 中编写可重用的代码

> 原文：<https://javascript.plainenglish.io/write-reusable-code-by-render-props-8ddb4f85cfa5?source=collection_archive---------0----------------------->

在[的上一篇文章](https://medium.com/@hellonehha/reactjs-reusable-components-by-props-children-df6d77d69a98)中，我们看到了如何通过使用*儿童道具*来重用 React 中的组件。在这篇文章中，我们将学习在 React 中创建可重用组件的另一种重用模式—***render . props****。*

![](img/fc2f02813f16b02634116ed9c28ac04e.png)

# 什么是 render.props

*render.props* 是一种编写可重用组件的方式。根据 React 文档——"*render . props 是一种* *在 React 组件之间共享代码的方式，使用一个值为函数的 prop。*“不断有人将 *render.props* 与 *children.props* 混淆比较。然而，两者都有自己的用例，但 render.props 比 *children.props* 更强大，我们将在一段时间内看到。

# **问题陈述**

假设，我们想要创建一个 3 列布局，布局中的每个列都可以通过传递道具来独立处理，并且我们可以访问它们。

简而言之，我们希望实现两件事:

1.  三列布局，其中所有三列组件都应该有一个独立的包装
2.  每个 col-component 应该能够从父包装器访问 props

![](img/9641f34058f806d804e161c01f277179.png)

# 解决方案:

让我们先试着通过*儿童道具*来解决这个问题，然后我们将移动到*渲染道具*

*步骤 1:为包装器创建四个组件——left . js、Right.js、Mid.js 和 Content.js。*

![](img/507e167c63f238fa689f9601404c1dd0.png)

*步骤 2:将下面的代码放入 Left.js、Mid.js 和 Right.js 中，不要忘记更改第 3、#4 和#7 行的组件名称*

![](img/0a64a2d899f50b1368da4497eb0b9c42.png)

*第三步:让我们组装到 index.js 中的应用程序*

![](img/ed3c40dabfd75d2879523264c345ba5a.png)

*第四步:通过 children.props 我们将渲染 Content.js 中的组件*

![](img/c3447197a9c67686ffb90c351a9845d5.png)

耶！！我们得到了内容中带有 *children.props* 的所有三个组件。现在，继续尝试 Content.js 中的以下代码来分别访问每个组件。

![](img/cfd79a7d173425883c3b8ab2a606b3a6.png)

你会看到这是行不通的。这是*儿童道具*范围结束和*渲染道具*开始的地方。所以，现在我们将重构代码来实现 *render.props*

代码链接:[https://codesandbox.io/s/y848622v9](https://codesandbox.io/s/y848622v9)

> 我们将使用相同的代码，只在需要的地方重构文件。

*第一步:按照下面的截图重构 Index.js。*

![](img/0f57e235b1c7a1fc7b3b22065c4e1cc0.png)

*第二步:在 Content.js 中，用 section 或 div 标签包装每个组件。(根据下面的截图)。*

![](img/27a3fb01edb5de580541a51e8f5d890f.png)

哇。！所以，现在我们能够独立地得到每个组件，并且它有自己的包装。满足了第一个要求，我们也看到了如何在缺少*儿童道具*的地方使用*渲染道具*。

第二个要求是将*道具*传递给组件。为此，我们将转到 *Content.js* 并将*道具*添加到渲染组件(*第 6 和第 7 行)。(参见下图截图)*

![](img/bdec965460154c965239aaab4db6b2e3.png)

> 如果你注意到上面的截图，我们正在传递圆括号中的道具，代表一个函数。是的，我们称之为道具。这里的功能是传递道具，但是等待我们的 Content.js 持有简单的 Component，然后它将如何处理它。现在，我们将最后一次重新访问 Content.js。

![](img/adab0be8cc1c0c9c7470f284c7d42572.png)

在 15 号线上方，我们正在传递一个函数作为道具，以访问从 6 号线传递的道具并返回组件。

![](img/bef52335a1a2e1ca5028bf859e255d01.png)

因此，现在您可以访问 *Left.js* 中的标题作为*道具*。有了这个，我们也能够成功地满足第二个要求。

代码链接:[https://codesandbox.io/s/9y6zpj4jjy](https://codesandbox.io/s/9y6zpj4jjy)

> Summary，render.props 让我们将组件作为道具传递，您也可以将组件作为函数传递，返回组件并从包装器组件访问道具。当您想要独立访问 render.props 中传递的每个组件时，这是很重要的，这在 children.props 中是不可能的。

如果你喜欢它，请分享并为它鼓掌。你可以在推特上关注我——twitter.com/hellonehha

## 进一步阅读

[](https://bit.cloud/blog/how-to-reuse-react-components-across-your-projects-l4pz83f4) [## 如何在项目中重用反应组件

### 最后，您完成了在应用程序中为表单创建一个奇妙的输入字段的任务。你对…很满意

bit.cloud](https://bit.cloud/blog/how-to-reuse-react-components-across-your-projects-l4pz83f4) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)***，以及****[***不和***](https://discord.gg/GtDtUAvyhW) *对成长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) ***。*****