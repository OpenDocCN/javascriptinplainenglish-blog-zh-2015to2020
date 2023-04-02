# 将 SDK 代码从核心 React 代码中分离出来

> 原文：<https://javascript.plainenglish.io/decouple-external-services-from-your-core-ui-code-dd490f91ae49?source=collection_archive---------10----------------------->

## 并根据开放/封闭原则减少未来 PRs 的大小

![](img/e142f792d3e1cae8fc2aebb874e25eb0.png)

KNOW YOUR ENEMY (image by [Klara Kulikova](https://unsplash.com/photos/CatcixzdUcg), meme from [Programming Memes](https://programming-memes.com/spaghetti-code-know-your-enemy/))

假设我使用外部服务提供商来处理我的电子商务应用程序的支付，我需要嵌入一些外部 SDK 代码来将支付服务集成到我的应用程序中。

在这个过于简单的例子中，假设支付服务负责根据客户的设备、地区等检查给定的支付方式(例如 Apple Pay 和 Google Pay)是否可用。而我的“核心”UI 组件`PaymentOptions`负责将可用的支付方式呈现为选项。最后，我希望在未来增加新的支付方式的灵活性(对于📈💰原因)。

我可以这样写。

然而，UI 代码与来自支付服务的外部代码紧密耦合，即**我必须修改** `**PaymentOptions**` **组件，以便添加新的支付方式或进行 SDK 更新。**

我也许可以将 SDK 代码分解到一个单独的钩子中。

**我还必须修改** `**PaymentOptions**` **和任何其他共享** `**usePaymentMethods**` **钩子的组件，如果我想添加的话，例如** `**isPaypalAvailable**` **。**

为了最小化未来 PRs 的大小，我一直在思考开放/封闭原则，即 SOLID 中的“O”(查看这个优秀的[解释者](https://medium.com/better-programming/revisiting-solid-927e6a5202d3)):*一个软件工件应该对扩展开放，但对修改关闭。*

用我自己的话来说:*我应该以这样一种方式设计这个功能，如果我将来要添加新的支付方式(开放扩展)，我不必接触我编写的任何原始代码(关闭修改)。*

以下是我对这一原则的看法。让我们将支付服务分离到它自己的模块中:一个简单的对象，其中每个键代表一种支付方法。每个支付方法键都指向一个具有`isAvailable`属性(使用 SDK 代码的函数)和`component`属性(支付选项的 UI 组件)的对象。

将`paymentServiceModule`导入到`PaymentOptions`组件中。

`PaymentOptions`现在与 SDK 实现细节脱钩，不知道具体的支付方式。

当我想用一种新的支付方式(如 PayPal)来扩展这一功能时，**只需将新的键/值对插入到** `**paymentServiceModule**` **中，而无需修改** `**PaymentOptions**` **组件或原始的支付方式。**

如果我要改变支付服务提供商(例如💸原因)只要支付方式的鸭子类型保持不变。

我是否正确地应用了开放/封闭原则？我很想了解其他遵循这一原则的 React 或 JavaScript 模式。

# 奖金

在`paymentServiceModule`，使用`[React.lazy](https://reactjs.org/docs/code-splitting.html#reactlazy)` API 延迟加载每个支付选项。

在`PaymentOptions`中，将每个支付选项包装在`Suspense`中，根据可用性延迟加载组件。

# **资源**

*   [在深入领域驱动设计或干净架构之前，改进代码的第一步](https://medium.com/javascript-in-plain-english/a-first-step-to-improve-your-code-before-diving-into-domain-driven-design-or-the-clean-architecture-90da4a80d863)
*   [重访固体](https://medium.com/better-programming/revisiting-solid-927e6a5202d3)作者[马修·卢卡斯](https://medium.com/u/12cc371abade?source=post_page-----dd490f91ae49--------------------------------)

# 阅读更多

[](https://medium.com/javascript-in-plain-english/how-to-decouple-data-from-ui-in-react-d6b1516f4f0b) [## 如何在 React 中将数据与 UI 解耦

### 第 2 部分:对钩子、渲染道具和特设模式的进一步探索

medium.com](https://medium.com/javascript-in-plain-english/how-to-decouple-data-from-ui-in-react-d6b1516f4f0b) [](https://medium.com/javascript-in-plain-english/decouple-data-from-ui-with-react-hooks-6f7fe968c3e3) [## 用 React 钩子将数据从 UI 解耦

### 以及我如何用 JavaScript 函数“编程到一个接口”

medium.com](https://medium.com/javascript-in-plain-english/decouple-data-from-ui-with-react-hooks-6f7fe968c3e3) 

📫*我们连线上*[*LinkedIn*](https://www.linkedin.com/in/suhanwijaya/)*或者* [*邮箱*](mailto:suhanw@gmail.com) *！*