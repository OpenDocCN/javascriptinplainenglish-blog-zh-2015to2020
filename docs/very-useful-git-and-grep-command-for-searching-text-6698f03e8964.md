# 一个非常有用的搜索文本的 Git 命令

> 原文：<https://javascript.plainenglish.io/very-useful-git-and-grep-command-for-searching-text-6698f03e8964?source=collection_archive---------5----------------------->

## 因为您最喜欢的 IDE 搜索不太好用

![](img/a592f61278b5c9cd20cb9afd03f81379.png)

Photo by [Josh Calabrese](https://unsplash.com/@joshcala?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将看到一个非常有用的命令来搜索 git 存储库中的特定文本。

很多时候，在处理代码时，您想要找出在存储库中的什么地方使用了某个特定的文本，或者用其他文本替换它，或者出于调试的目的。

如果您尝试在您最喜欢的 IDE(如 Visual Studio 代码或 Sublime Text)中使用全局搜索，您将在 package.json、package-lock.json 和其他不必要的文件中的数百或数千个文件中获得您正在搜索的文本。

相反，您可以使用 **git grep** 命令在文件中快速搜索。

所以让我们深入研究一下

假设，您想要找到使用了像**require(“express”)或 require(“express”)**这样的特定包的文件，只需在 VS 代码中使用 express 的全局搜索就会得到许多结果，相反，我们可以从您的存储库中的终端运行下面的命令。

Search For require(“express”) or require(‘express’) Text

上述命令将列出文件名、行号以及该文件中的匹配文本

这将产生如下所示的输出

![](img/98fbfb6027bb92cac3d71db99257b955.png)

Express Output

假设您想要查找在哪些文件中使用了来自 app object 的特定属性，如 http_mode。简而言之，如果你想找到。http_mode 您可以执行以下命令

Find Occurrence Of .http_mode

这将产生如下所示的输出

![](img/d21e77d6b17f87e8df255eed4635928b.png)

.http_mode occurrence result

如果你想只在一个特定的目录而不是所有的目录中找到文本 **NavLink** ，你可以执行下面的命令

Search for NavLink text In src/routes directory

这将产生如下所示的输出

![](img/f36f726ac8660116d5014706c9714fc2.png)

NavLink Output

本文到此为止。希望这对你有所帮助。

**别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周简讯，里面有惊人的技巧、窍门和文章。**