# 如何应对热与冷

> 原文：<https://javascript.plainenglish.io/how-to-rxjs-hot-vs-cold-3b430c4b7690?source=collection_archive---------4----------------------->

小心前面的热东西！Rx 中有哪些冷热可观察？

这是我关于 RxJS 系列的第二部分，你可以通过下面的链接查看我的其他文章:

*   [如何 RxJS —基础知识](https://medium.com/javascript-in-plain-english/how-to-rxjs-the-basics-d5a5905497e0)
*   **如何 RxJS——热 vs 冷**
*   [如何 RxJS —创作基础](https://medium.com/javascript-in-plain-english/how-to-rxjs-creational-basics-70318c5eca38)

在本系列文章的前一部分，我写了 RxJS 的基础知识，以及 Observables 是如何工作的，它们基于什么概念。在这一部分，我想谈谈 RxJS 的一个基本概念，然而这是这项技术中最常被误解的部分。

![](img/cda5c1540473e7f65e99215369cebbf3.png)

Photo by [ferdinand feng](https://unsplash.com/@ferdinand_feng?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ice-lava?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 可观察的人是懒惰的——难道不是吗？

正如我在上一篇文章中所说的，Observables 是懒惰的，所以他们只会在有人感兴趣的时候做一些事情。如果我们继续下去，那就意味着，你不能错过任何一个可观测物体发出的东西，因为只有你订阅了它们，它们才会发出某种东西。然而，并非在所有情况下都是如此。看看下面的例子:

Missing out a next notification

如你所见，我们将错过关于这个主题的第一个下一个事件。那么，这整个可观察事物的懒惰是怎么回事呢？

# 制片人

![](img/500b24e69c83ec06e53814d4c41cbab0.png)

Photo by [Christopher Burns](https://unsplash.com/@christopher__burns?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/factory?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

正如我们已经知道的，可观测数据是一个异步数据流。我们可以通过监听这个数据流来获取数据，但是有些东西需要*产生*这些项目。这个东西被称为可观察的**生产者**。例如，在“of”创造性操作符的情况下，它是给它的参数，或者在“from”操作符的情况下，它可以是我们传递给它的承诺。

更简单地说，可观测量是函数，它们的工作是将观察者与生产者联系起来。如果我们继续下去，我们也可以意识到，因为一个可观察的只是一个函数，有一个工作，他们不一定要设置生产者。他们唯一的任务是每当生产者制造一些新的数据时通知观察者。

我们可以根据产生者把可观察的事物分成两类。一个可观察对象可以是热的*或冷的*或*。*

*![](img/3e943a5595fd4586cdd5d5d4277d7a58.png)*

*Photo by [v2osk](https://unsplash.com/@v2osk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ice-cold?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

> *如果它的生产者是在订阅期间创建或激活的，我们称之为可观察的感冒。*

*让我们通过用构造函数创建一个可观察对象来看看一个基本的例子。*

*Creating a Cold Observable*

*上面的代码将每秒钟一个接一个地打印一系列数字。这里重要的部分是我们给 Observable 的构造函数的函数。在 Rx 的类型定义中，这个参数被称为“订户”。每当有人订阅一个可观察对象时，这个方法就会被调用，这个方法负责将生产者与观察者联系起来。在上面的例子中，我们在这个函数中创建了 interval，所以我们在订阅期间创建并激活了我们的可观察对象的生产者。因为在我们订阅这个可观察对象之前，这个间隔是不存在的，所以我们可以确定我们没有丢失任何由可观察对象发出的数据。简单地说，我们正在懒洋洋地创建我们的生产者。*

> *如果一个可观察的 Hot 的生产者是在它的订阅之外被创建或激活的，我们称之为可观察的 Hot。*

*现在，让我们来看一个热可观测的例子。*

*Creating a Hot Observable*

*在这里，生产者是 WebSocket，但是这里的问题是，在订阅期间，我们没有创建这个 WebSocket 生产者。相反，我们使用的是生产者的引用，所以生产者已经存在，甚至可能已经发出消息。在这个 WebSocket 例子中，这样更好，因为您不希望每次有人订阅您的 Observable 时都创建一个新的 WebSocket 连接。在这种情况下，当您订阅可观察对象时，您可能已经错过了一些“下一个”通知。*

*然而，上面例子的一个缺陷是我们没有任何关闭 WebSocket 连接的逻辑。在现实世界的应用程序中，您将为 WebSocket 连接创建一个冷的可观察对象，并将这个冷的可观察对象变成热的，并让其他人订阅这个热的可观察对象。RxJS 提供了几个操作符来使一个冷的可观察的热的，但是我们将在本系列的后面讨论它们。*

# *摘要*

*总而言之，我们刚刚讨论的主题是 RxJS 最重要的基石之一，但也是新手程序员最容易误解的。在我或我的一个程序员同事遇到的大多数情况下，缺乏对这个主题的理解是至关重要的。*

# *注意事项和问题*

*我的一个程序员朋友在我的第一篇文章中指出，读起来有点长。所以，在这里我试着把事情做得简短一点，以便更容易理解。如果可以的话，请留下一些反馈，这样更好吗？或者我应该回到更长，但更完整的话题？*

*此外，请随时留下关于我的故事的任何建议或评论。我也乐于接受新的事物来改进或学习。*

## *JavaScript 用简单的英语写的一个注释:*

*我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，请用你的 Medium 用户名发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。*