# 这个简单的 git hack 可以节省您查找 bug 的时间

> 原文：<https://javascript.plainenglish.io/this-easy-git-hack-can-save-you-hours-when-looking-for-a-bug-cccfdbe0733f?source=collection_archive---------9----------------------->

![](img/67dfc74e35c586c0179f3562fa32ea12.png)

Photo by [Neringa Hünnefeld](https://unsplash.com/@ringane?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 在一次面试中，我被问到这个问题:

*“如果你注意到了一个错误，但是你不知道在你一百万行代码的应用程序中到底是什么导致了这个错误，你将如何找到它？”*

我花了几秒钟思考，然后说:我会尝试找到我所知道的没有错误的最新提交(姑且称之为提交 A)，然后转到我所知道的肯定有错误的提交(提交 B)之间的提交，看看它是否有错误。如果是，那么它就成为新的提交 B，否则它就成为新的提交 a。然后清洗并重复，直到我找到引入 bug 的提交。

面试官(我最终和他一起工作，并从他身上学到了更多)点点头。

## 他对回答很满意，然后说道:

”*是的，你刚才描述的东西叫做二分搜索法。有一个 git 特性可以做到这一点。我想知道你是否已经知道了。”*

我摇摇头，说“不”。

面试官然后告诉我，这个命令叫`git bisect`。

今天，我需要做的就是:找到一个天知道何时、如何引入的 bug 提交。我是这样解决的:

1.  第一步是找到一个无错误的提交，并将其复制到 SHA。
2.  完成后，您将 git 放在一个 bug 提交上(或者最近一次提交，因为它已经有了 bug ),然后在命令行上键入`git bisect start`。
3.  然后你输入`git bisect bad`
4.  然后`git bisect good (good commit SHA)`使用我们在步骤 1 中获得的 SHA。
5.  然后 Git 会带你到中间的提交，你检查 bug 是否存在。如果是，你输入`git bisect bad`，否则输入`git bisect good`，重复这个步骤，直到 git 告诉你`some SHA is the first bad commit`，瞧！你已经抓到坏人了。您只需要检查提交的内容，看看哪里发生了变化，错误是从哪里产生的。

关于这个非常有用的 git 提交的更多文档，您可以查看:[https://git-scm.com/docs/git-bisect](https://git-scm.com/docs/git-bisect)

编码快乐！