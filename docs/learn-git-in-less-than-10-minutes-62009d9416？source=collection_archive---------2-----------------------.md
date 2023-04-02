# 不到 10 分钟学会 Git

> 原文：<https://javascript.plainenglish.io/learn-git-in-less-than-10-minutes-62009d9416?source=collection_archive---------2----------------------->

你有没有想过回到以前的代码版本或者与他人分享？嗯，你可能正在寻找的东西被称为 Git，它是一个“分布式版本控制系统，用于在软件开发期间跟踪源代码的变化”，它还允许你与其他开发人员共享你的代码。今天我们将学习 Git 的基础知识，以及如何使用 Git 和[](https://github.com)*。学习如何使用 Git 和 GitHub 应该只需要几分钟，即使你是编码新手，这对任何开发人员来说都很棒！*

*![](img/8807ac920b48cafca12b9c22193f5e7f.png)*

# *从 GitHub 和 Git Bash 开始*

*为了开始使用 Git，你需要下载一个新的终端到你的电脑上，名为 Git Bash。Git Bash 将允许我们使用 Git，这样我们就可以开始将我们的文件上传到一个名为 [*GitHub*](https://github.com/) 的网站上。要安装 Git Bash，请单击下面的链接，并根据您的设备按照说明进行操作。*

*[](https://git-scm.com/downloads) [## 下载

### Git 带有内置的 GUI 工具(git-gui，gitk ),但是有几个第三方工具供用户寻找一个…

git-scm.com](https://git-scm.com/downloads) 

一旦你在电脑上安装了 Git Bash，你就可以开始使用 GitHub 了，这是我们放文件的地方。要开始，只需创建一个 GitHub 帐户，并填写您的必要信息。

[](https://github.com/) [## 一起打造更好的软件

### GitHub 汇集了世界上最大的开发人员社区来发现、共享和构建更好的软件。来自…

github.com](https://github.com/) ![](img/aced8a676f6e35c5f7a618a71ff5fd7b.png)

# 创建 GitHub 存储库

要开始创建您的第一个 GitHub 资源库，只需点击屏幕左侧 ***【资源库】*** 旁边的 ***【新建】*** 按钮，这是您上传所有文件的地方，以便您或其他开发人员稍后可以访问您的代码。给你的存储库起一个名字，如果你愿意的话，给它一个描述，把它设置为 public 或者 private，并且确保你选择了复选框 ***“用一个 README 初始化这个存储库”*** ，一旦你完成了这些，你就可以开始了！如果你想添加一个*现有项目*到你的库，而不是从零开始，这个视频会告诉你你需要知道的一切。

![](img/aced8a676f6e35c5f7a618a71ff5fd7b.png)

# Git 克隆

一旦你创建了你的库，点击按钮 ***克隆或下载*** 并复制提供的链接。

![](img/2e73a0adbb0d69fa045ec2a5157475ed.png)

复制完链接后，您可以打开 Git Bash 终端并输入以下命令:

```
$ git clone (your link here)
```

这就是您将如何设置刚刚创建的存储库并将其放在您的 PC 上。完成后，键入:

```
$ cd (the name of your repository)
```

当您完成这一步后，您就可以设置您想要的任何新项目了，只要您将所有文件放在创建的新文件夹中，该文件夹也与您的新存储库同名。

![](img/aced8a676f6e35c5f7a618a71ff5fd7b.png)

# Git 添加

一旦你做好了一切准备，并且已经在代码上工作了一会儿，你可能会到达你想要保存你的工作的点。这就是 git add 的用武之地。要保存您的工作，只需输入:

```
$ git add .
```

(一定要包括点)Git 会自动保存你所有的进度。为了将您的更改推送到您的存储库，我们将使用 Git Commit 和 Git Push。

![](img/aced8a676f6e35c5f7a618a71ff5fd7b.png)

# Git 提交和 Git 推送

在您完成 Git Add 并准备好将您的工作上传到您的存储库之后，您只需要再输入两个命令，它们是 Git Commit 和 Git Push。Git commit 将允许您编写一条小消息，描述您添加到代码中的新内容，Git Push 将允许您接受该提交并将其放入您的存储库中。要使用 Git Commit，只需在终端中键入以下内容:

```
$ git commit -m "your message here"
```

引号中的信息旨在向其他开发人员或您自己展示您对代码所做的更改，并且应该只是对您所做工作的简要描述。完成 git 提交后，只需输入:

```
$ git push
```

到您的终端，您的新提交和您的更改将被推送到您的 GitHub 库，您或其他人稍后可以在 GitHub 上访问它。

![](img/aced8a676f6e35c5f7a618a71ff5fd7b.png)

# 附加信息

Git clone、add、commit 和 push 是您在使用 Git 时需要了解的主要命令，但是，如果您想更深入地了解 git，您可以在下面的链接中找到。

[](https://www.codecademy.com/learn/learn-git) [## Git 教程:免费学习 Git 基础知识

### 利用我们的 Gilt 教程，学习如何将不同版本的代码项目保存和管理到 Git 分支中

www.codecademy.com](https://www.codecademy.com/learn/learn-git) 

来源:

[*https://github.com/*](https://github.com/)

*【https://en.wikipedia.org/wiki/Git】T5[T6](https://en.wikipedia.org/wiki/Git)*

*[*https://git-scm.com/downloads*](https://git-scm.com/downloads)**