# 用案例笑话更好地组织你的测试

> 原文：<https://javascript.plainenglish.io/organize-your-tests-better-with-jest-in-case-19ea1ce86905?source=collection_archive---------6----------------------->

![](img/bb3869d0055683ddba7a9a073dc47089.png)

Photo by [Azharul Islam](https://unsplash.com/@azhar93?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这篇文章讨论了我如何使用[案例笑话库](https://github.com/atlassian/jest-in-case)来编写更有组织的测试。然而，使用这个库有一个先决条件，你可能猜对了。

如果你使用 Jest 来编写测试，那么这篇文章可能会对你有所帮助。

## 问题背景

我不知道你是怎么想的，但是很多时候，当我写一些非常普通的函数时，我会有很多测试用例，这些测试用例基本上是用不同的值反复进行的相同的测试—杰米·凯尔

上面的陈述让我产生了很大的共鸣，尤其是当我开始面对这样的情况，我编写的单元测试代码是我最初代码变更的三倍或四倍。更糟糕的是，大多数单元测试代码都是重复的。

事不宜迟，让我们看看下面的案例研究。

# 个案研究

下面的函数是一个简单的重定向函数，我们将

1.  检查交易状态
2.  根据交易状态将用户重定向到不同的状态页面

现在让我们试着为上面的函数写一个单元测试。上面的典型 Jest 单元测试如下所示。

我们在上面写了 4 个测试用例来涵盖以下场景:

*   当交易状态为“待定”时，重定向至“存款待定 Url”
*   当交易状态为“已批准”时，重定向至“存款成功 Url”
*   重定向至“交易状态为“失败”时存放失败的 Url”诊断树
*   重定向至“交易状态为“失败”时存放失败的 Url”诊断树

从上面的测试用例中，我们意识到所有的测试用例都有彼此非常接近和相似的代码。唯一的区别是

上面的测试用例有什么问题？

*   **难以维持。**举一个简单的例子，如果我要将`**findTransaction**` 函数重命名为`**findTansactions**` ，我大概要用 transaction service . find transaction&来查找所有的行，更新每个测试用例。
*   **无聊&不思进取**。当你不得不编写如此多的测试用例场景并重复相同的代码库时，你会感到厌烦，不太可能去编写它们，最终会变得没有生产力。

# 使用案例笑话编写测试用例

现在让我们使用库`jest-in-case`为上面测试的类似场景编写测试用例。

利用`jest-in-case`，我只需编写一次测试用例逻辑，并通过选项提供不同的输入和预期结果。测试用例最终减少了 2 倍的代码，更加清晰易读。更重要的是，它使开发人员更容易维护测试用例。

# 结论

从这篇文章中，我们知道`jest-in-case`是一个很好的库，它使我们能够:

*   编写更简洁的测试用例。这也提高了编写测试用例的效率。
*   当您的测试用例基于不同的输入有多个场景时，这是非常有用的。

# 参考

*   笑话 [Github](https://github.com/atlassian/jest-in-case)
*   詹姆斯·凯尔[网站](https://jamie.build/jest-in-case.html)

> *原文章发表在我的* [*博客*](https://tekloon.dev/how-to-write-well-organized-unit-test) *。*