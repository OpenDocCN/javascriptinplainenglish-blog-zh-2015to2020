# Git 基础知识:重命名本地和远程分支

> 原文：<https://javascript.plainenglish.io/git-basics-renaming-a-local-and-remote-branch-2badf749ea3d?source=collection_archive---------7----------------------->

![](img/530f8dfface2f9583a56e66a35e9db2d.png)

Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Git 是大多数开发人员，尤其是 web 开发人员的必备工具。但有时我们会犯愚蠢的错误，因为我们是人。当您错误地命名了一个分支并将它移动到远程服务器/存储库时。然后，在其他开发人员/团队成员有机会批评你没有正确适应命名约定之前，遵循下面提到的步骤

> **1。重命名您的本地分支。**

如果您在要重命名的分支上:

> git branch -m<new-name></new-name>

如果您在不同的分支上:

> 吉特科-m<old-name><new-name></new-name></old-name>

> **2。删除<旧名称>远程分支，推送<新名称>本地分支。**

> git 推送原点:<old-name><new-name></new-name></old-name>

> **3。重置<新名称>本地分支的上游分支。**

如果您仍在不同的分支上，则切换到新的分支，然后:

> git 推送原点-u<new-name></new-name>