# 软件设计:什么是验收标准，为什么你需要它？

> 原文：<https://javascript.plainenglish.io/software-design-how-to-write-good-acceptance-criteria-b4aed47b4ad7?source=collection_archive---------8----------------------->

![](img/9950ea1407231edca06f069267360eaa.png)

你在为一个网站开发一个功能。这是一个简单的登录表单。因为你是一个超级精明的软件超人，你决定在投入开发之前做一些基本的规划。至少，您需要确定将要构建的特性的一些方面。

这是你想出来的。你需要

*   用户名和密码输入框
*   提交按钮
*   某种方式让用户知道他们是否输入了错误的信息
*   一些恢复被遗忘的细节的方法
*   如果他们还没有账户，可以通过某种方式注册一个

即使是简单的功能也很难设计。你已经列出了五件你想追踪的事情。也许还不止于此。没有适当的计划就匆忙开发一个特性是疯狂的，但是你知道这一点，并且你已经写了上面的清单。

那么，够了吗？

如果你想写一些行为测试呢？怎么把上面的转化成用户步骤？这种“简短的标语”方法是否能告诉你用户实际上是如何做这些事情的？你考虑过用户成功登录后*会发生什么吗？这种方法会鼓励这种想法吗？*

我们知道*我们需要什么*，但是*这如何转化为一个设计良好的功能？*

你大概猜到答案:验收标准。

**什么是验收标准？**

验收标准是一组步骤的不必要的模糊名称，这些步骤描述了用户如何与特定的特性进行交互。它们以使用`Given, When,`和`Then`步骤的格式编写，并映射到用户动作。正因为如此，它们很容易转化为行为测试。以这种方式设计一个特性也是一种很好的方式，可以识别出使它工作所需要的其他东西。

您的登录表单的接受标准示例可能如下所示:

```
**Given** I have an account registered with <app>
**And** I am viewing the login form
**When** I enter correct login details
**Then** I should be logged in
**And** I should see the homepage
```

**给定**表示动作的某种先决条件。**当**指定动作时。**然后**定义动作的结果。我们还可以使用**和**通过添加额外的条件来补充任何阶段。这种做法是合乎逻辑的，明确的&简单的。这些步骤中的每一步都准确地解释了在该场景中预期会发生什么。

我们也可以很容易地为此编写一个行为测试，因为我们确切地知道所涉及的设置、行动和结果。我们有测试的先决条件:用户必须有一个帐户。我们有一个动作:用户点击登录按钮。我们得到的结果是:用户登录并查看主页。

这个 AC 也给了我们一些额外的信息。当我写的时候，我意识到我不知道用户成功登录后会发生什么。像这样格式化需求迫使我去思考它，从而推动了产品设计和用户体验。

最后，凭借它的格式，AC 鼓励你采用一种更符合逻辑的思维方式，并且由于使用了`Then`，确保你仔细考虑用户行为的结果。它让你关心用户会如何体验你的应用，而不仅仅是你知道你想做的那些很酷的东西。

**如何写好 AC？**

所以，你热衷于写这些东西，但是你怎么做才是正确的呢？上面的登录功能非常简单，但是更复杂的概念会导致混乱的 AC，所以记住以下几点很重要:

1.  从用户的角度来写

这是第一条规则。验收标准不是关于你，作为一个开发人员，如何与某些东西互动。这不是关于你如何*希望*某人与某事互动。你必须假装自己是这个星球上最没希望、最令人恼火的人:一个**用户**。因为他会摆弄你珍贵的登录表格。

2.简单

AC 应该很好理解。尝试将每一行映射到特定的用户操作或先决条件，例如输入正确的用户详细信息或已经注册到应用程序。试图包含多个内容的一长串 AC 会影响清晰度&从而抵消了上面提到的许多好处。

3.明语

这一条与第二点有关，并直接影响到它。用通俗易懂的语言写 AC。这种方法的一个主要好处是它可以被非技术人员理解。一个能够向任何人描述特性的工具*和*同时驱动实现/测试是非常宝贵的。复杂的语言会破坏这一点。

4.避免实施细节

AC 应该描述用户如何与一个特性交互；它不应该描述该功能看起来像什么，或者它是如何工作的。你实现某事的方式可能会比想法本身发展得更快。登录是常见的，但是提交按钮的颜色或者它使用的认证提供者就像放屁一样具体。

5.不要技术化

同样，这一点与上一点有点关联。不要提算法或机器学习或线粒体是细胞的发电站。不相关。

**AC 本身够用吗？**

不，这只是起点。它将推动您的设计，它将传达您的愿景&它将帮助您进行测试，但它不是最重要的。您仍然应该编写子任务来更好地定义您的功能的更多技术方面，创建模型并编写具体的示例。所有这些事情都是有效的和有价值的，并且应该被使用，但是写得好的验收标准是一个坚实的起点，并且当做得正确时，将总是产生更好质量的软件。

除非你是土坯，否则不管你做什么，它都是垃圾。

感谢您阅读这篇短文。我按照规定写作，但我也意识到，我曾开玩笑地点着我的“头发”，所以我绝不是什么天才。如果您在这里看到任何错误或不完善的地方，请随时发表评论&我会更新文章。我们都爱学习。

你可以在我的中型个人资料上找到更多我的东西&在 twitter 上关注我(如果你喜欢) [@lm_writing](https://twitter.com/lm_writing) 。