# 服用类固醇的节点版本管理器(NVM)

> 原文：<https://javascript.plainenglish.io/nvm-on-steroids-auto-switch-nodejs-version-in-your-terminal-without-typing-the-nvm-use-command-3a49c4b5c6e0?source=collection_archive---------6----------------------->

## 自动切换 Node.js 版本，无需输入“nvm use”命令

![](img/d61125abb642705af6d58e3d49b29da0.png)

Photo by [Mohamed Nohassi](https://unsplash.com/@coopery?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

每次打开项目根目录中的终端时，都必须手动切换节点版本，这很快就会变得很烦人。**如果能根据项目的需求自动切换节点版本岂不是很棒？**这个简单的技巧会让你的开发体验更好:

# macOS/Linux

对于本教程，我们假设您已经在 linux/macOS 系统上安装了[节点版本管理器(nvm)](https://github.com/nvm-sh/nvm) 。

我们可以简单地编写一个 shell 脚本，在每次打开目录中的终端或更改目录时运行。该脚本在目录中查找一个`.nvmrc`文件，并尝试使用该版本。如果它没有找到那个版本，脚本将返回到默认版本。

## 步骤 1 —创建一个`. nvmrc '文件

在 npm 项目的根目录下，在`package.json`文件旁边创建一个`.nvmrc`文件。

或者，如果你像我一样懒，可以用下面的命令轻松完成:

```
$ nvm current > .nvmrc
```

## 步骤 2 —将脚本添加到您的 shell 的配置文件中

我们将添加一个 shell 脚本，它本质上修饰了更改目录命令并扫描目录中的`.nvmrc`文件。如果它找到了`.nvmrc`文件，它通过运行`nvm use`使用指定的版本。如果没有，脚本将退回到默认的节点版本。如果你想了解更多关于 shell 脚本的知识，请查阅这本书。

**痛击**

将这个脚本添加到您的`.bash_profile`或`.bashrc`文件中，然后通过运行以下命令来获取您的 bash 环境:

```
$ source ~/.bash_profile
```

或者

```
$ source ~/.bashrc
```

好了🎉 🎉 🎉 🎉

现在打开您的终端并运行:

```
$ node -v
```

**Zsh**

Zsh 需要一个稍微不同的脚本。

将此添加到您的。zshrc 文件，并通过运行以下命令获取您的 zsh 环境:

```
$ source ~/.zshrc
```

这里我们使用 [zsh 钩子函数](http://zsh.sourceforge.net/Doc/Release/Functions.html)来修饰`cd`命令，自动检测`.nvmrc`文件并使用指定的节点版本。如果脚本找不到一个`.nvmrc`文件，脚本将返回到默认的节点版本。

好了🎉 🎉 🎉 🎉

现在打开您的终端并运行:

```
$ node -v
```

# Windows 操作系统

通过使用用于 Linux 的 (WSL)的 [windows 子系统并在 WSL Linux 发行版上安装 nvm，您可以在 Windows 上使用相同的解决方案。如果你不/不能使用 WSL，请关注我，了解一个潜在的](https://docs.microsoft.com/en-us/windows/wsl/install-win10)[nvm for windows](https://github.com/coreybutler/nvm-windows)+PowerShell/cmd 解决方案。如果你捷足先登，请在下面的评论中告诉我:)

# 摘要

作为软件工程师，我们的使命是提高效率。我们学习了如何利用[外壳脚本](https://amzn.to/36QN5ug)来自动检测和切换我们的 nvm 节点版本。这在处理依赖于不同节点版本的多节点项目时特别有用。现在，我们可以坐下来在节点项目之间切换，而不用担心每次在 vscode :D 中打开新的集成终端时手动切换我们的节点版本

感谢你的阅读，我希望这能帮助你，就像它帮助了我一样。

我很想在下面的评论中知道这是否有用！

你可以在[https://github.com/nivrith](https://github.com/nivrith)跟着我

快乐工程！