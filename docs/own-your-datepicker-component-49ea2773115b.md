# 拥有您的 Datepicker 组件

> 原文：<https://javascript.plainenglish.io/own-your-datepicker-component-49ea2773115b?source=collection_archive---------3----------------------->

## 这并不像我想象的那么令人畏惧

![](img/7314b8abd9441d24132dcbc80eada3e1.png)

Photo by [Dima Pechurin](https://unsplash.com/@pechka?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我想要一个满足我的要求的日期选择器库，如下所示👇

# 1.**它是一个组件**

理想情况下，它应该是一个“Web 组件”，要么用“ [lit-html](https://lit-html.polymer-project.org/) 构建，要么用[低级 API](https://developer.mozilla.org/en-US/docs/Web/Web_Components) 构建。这两条路我都走过，但是我遇到了各种各样的问题，主要是试图让 CommonJS 包在我的影子世界中工作，导致不可持续的攻击，所以我取消了它们。

在我看来，我最终回到了最不邪恶的版本——Preact，在我看来，它仍然是最接近金属的，没有所有疯狂的语法和规则——这将完全疏远那些从纯 HTML/CSS/Javascript 开始的初学者——同时仍然为后代强制执行非常少的一致性和健全性。

因此，我寻找的日期选择器将是一个 **Preact 组件**。

# **2。极简主义**

> *极简主义是一种工具，可以让你摆脱生活的过剩，专注于重要的事情——这样你就可以找到幸福、满足和自由。*
> 
> [*【来源】*](https://www.theminimalists.com/minimalism/)

在我看来，典型的约会对象有以下特点:

## 为您提供以通用标准(ISO8601)选择的日期

这是一个普遍的要求和标准。我们可以安全地把它烤成核心。

## 以不同于 ISO8601 的格式显示日期

显示格式可能与文化相关。用户应该对它的决定有完全直观的控制。

## 整体外观和感觉

外观和感觉也是文化相关的——让用户完全定制你的日期选择器，而不用继承你的 CSS 包袱。

原则上，这将引导您使用一个轻量级的日期选择器。然而在实践中，我意识到，由于各种既得利益者(用户和实现者)看到自己的需求和偏见被推动，引入了更多的复杂性，因此导致了脆弱和臃肿的系统，所以明显流行的日期选择器库永远不会满足要求。

所以我需要去别处看看，因为极简的轻量级对我来说是必须的。

# 3.设计风格的白板

你的组件的用户不应该为了覆盖你的日期选择器的默认 CSS 而揪着自己的头发[让自己变胖](http://seriouspony.com/blog/2013/7/24/your-app-makes-me-fat)，或者更糟，不得不学习新的框架、技术或者你的一堆奇怪的 API。

我曾经亲身经历过工作/继承一个已经变得令人厌恶的 Boostraped 风格的应用程序时身体上的痛苦。所以我不希望这种事再发生在任何人身上。保持简单和无聊。

给用户一个选项，让他们在空白板上用纯 CSS 来设计你的日期选择器。

# 4.最小的认知开销

你写的每一行代码都在消耗你(和其他人)有限的无价的认知能力。像日期选择器这样基本的库不应该用神秘的代码实现，这些代码只有在最初的实现者头脑中才会出现。

我希望能够简单地使用和混合一个日期选择器，从它的实现细节开始。它不应该被 Typescript 语法噪音所污染，也不应该是高度抽象的。还是那句话，保持简单无趣。放松点，你不是在建造一个自我学习的人工通用智能。看在上帝的份上，这只是个约会对象。

很明显，没有一个日期选择器库是我喜欢的。所以像其他人一样，我继续构建了一个…但这有点言过其实，因为我实际上是在这篇为 React 构建了一个的文章的基础上构建了[，尽管我对它做了很多修改并将其作为一个包发布。](https://blog.logrocket.com/react-datepicker-217b4aa840da/)

这就是体现了上述所有理念的日期选择器:

[](https://github.com/kilgarenone/datepicker) [## kilgarenone/datepicker

### 为您提供最基本的日期选择器功能，同时完全自由地处理选定的…

github.com](https://github.com/kilgarenone/datepicker) 

这是演示:

 [## 日期选择器

kheohyeewei.com](http://kheohyeewei.com/datepicker/) 

希望有人觉得有用🙏

好的，就这样。以后再说。

![](img/f19baa3d22a0587a1833299425fe21a6.png)

Untitled by Kheoh Yee Wei