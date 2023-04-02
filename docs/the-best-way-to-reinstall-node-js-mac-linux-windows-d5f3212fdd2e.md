# 重新安装 Node.js 的最佳方法(Mac/Linux/Windows)

> 原文：<https://javascript.plainenglish.io/the-best-way-to-reinstall-node-js-mac-linux-windows-d5f3212fdd2e?source=collection_archive---------2----------------------->

![](img/f16e39d030579859e8b57e7abae743a9.png)

Photo by [Oliver Hale](https://unsplash.com/@4themorningshoot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如何安装 Node.js 最简单的方法之一就是去官方网站，下载安装程序并安装。后来，当开发人员需要切换到 Node.js 的另一个版本或更新它时，他可能会面临一种情况。

仍然可以从官方网站安装它，但是一个系统中已经存在多少节点呢？在已安装的版本之间切换的速度有多快？

也许这是一个很好的时机来卸载它们，并设置系统能够在几秒钟内切换节点，总是知道已安装版本的数量，并能够在一个简单的命令中删除任何版本。

# 如何从 Mac OS 卸载 Node.js

首先，我们需要卸载旧的节点版本以及与之相关的所有东西。如果你用自制软件安装了以前的版本，你是幸运的。自制方法是在 Mac 上安装或卸载节点的最简单的方法之一。

```
brew uninstall --force node
```

在终端中键入以下命令。brew 将卸载**所有**已安装的 Node.js 版本。

之后，最好运行`brew cleanup`，它将删除所有未使用的依赖项和文件夹。

```
brew cleanup
```

如果 Node.js 的安装方式不同，这不是问题。您需要手动删除它。有一堆文件夹，它们可以从 finder 或一个终端中一个接一个地删除。

**node . js 和 npm 定位或存储其资源的文件夹列表**:

*   `/usr/local/lib`中的`node`和/或`node_modules`
*   `/usr/local/include`中的`node`和/或`node_modules`
*   `/usr/local/bin`中的`node`、`node-debug`和`node-gyp`
*   `.npmrc`在您的主目录中(这是 npm 设置，**如果您打算立即重新安装节点，请不要删除它**)
*   `.npm`在您的主目录中
*   `.node-gyp`在您的主目录中
*   `.node_repl_history`在您的主目录中
*   `/usr/local/share/man/man1/`中的`node*`
*   `npm*`在`/usr/local/share/man/man1/`
*   `node.d`在`/usr/local/lib/dtrace/`中
*   `node`在`/opt/local/bin/`
*   `/opt/local/include/`中的`node`
*   `/opt/local/lib/`中的`node_modules`
*   `node`在`/usr/local/share/doc/`
*   `node.stp`在`/usr/local/share/systemtap/tapset/`

如果您不想手动搜索所有这些文件夹和文件，您可以在终端中运行一个命令:

```
sudo rm -rf /usr/local/{lib/node{,/.npm,_modules},bin,share/man}/{npm*,node*,man1/node*}
```

这个命令不涉及您的主目录，所以您可以稍后决定如何处理其余的文件。

现在，我们可以删除随 npm 一起安装的所有全局软件包:

```
rm -rf ~/.npm
```

执行这些命令后，Node.js 和 npm 将从系统中完全删除。

# 如何在 Mac 上安装 Node.js

清理之后，我们可以继续安装新的 Node.js，但是我们不会直接安装。因为如果我们安装某个版本，我们在未来仍会有多个节点和维护的问题。

为了解决这个问题，我们应该安装一个额外的[小工具:NVM](https://github.com/nvm-sh/nvm) 。

对于这个工具，我们只有一个要求——安装命令行工具。因此，如果您的系统中没有安装它们，您应该运行:

```
xcode-select --install
```

我们准备安装 NVM。最简单的处理方法是. sh 脚本。
下载并运行这个脚本我们可以用下一个命令:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

**0.37.2** 是本文撰写当天的最新版本。可以在 [NVM GitHub](https://github.com/nvm-sh/nvm/releases) 上查看版本。

当然也可以手动安装。您需要克隆一个存储库并将可执行文件添加到 PATH 中。在 [NVM 自述文件](https://github.com/nvm-sh/nvm#git-install)中描述了详细说明。如果您需要在 CI 管道中使用 NVM，这将非常有用。我建议将 NVM 添加到管道中使用的 Docker 映像中。我在上一篇文章中描述的关于提高 CI 性能的更多技巧:

[](https://medium.com/javascript-in-plain-english/improving-ci-performance-aka-how-to-save-your-money-31ff691360e4) [## 提高 CI 绩效(也称为如何省钱)

### 如何使用缓存和 Docker 映像提高 CI/CD 管道速度

medium.com](https://medium.com/javascript-in-plain-english/improving-ci-performance-aka-how-to-save-your-money-31ff691360e4) 

> 不要忘记重启终端窗口来刷新环境变量

我们差不多完成了。现在，我们可以轻松地安装 Node.js 的任何版本。例如，该命令将安装最新版本:

```
nvm install node
```

如果您希望安装 LTS 版本，但使用最新的 npm，您可以执行以下操作:

```
nvm install --lts --latest-npm
```

您可以提供任何版本，而不是使用像`—-lts`这样的标志:

```
nvm install 8.9.1 # or 10.10.0, 12, etc
```

要查看已安装版本的列表，您需要运行以下命令:

```
nvm list
```

安装后，您需要为您的系统选择默认版本:

```
nvm use --lts
```

**但是 windows 用户呢？**

对于 windows 可用的一个非常类似的工具:[Windows 的节点版本管理器(nvm)](https://github.com/coreybutler/nvm-windows)。这是一个不同的项目，但它实现了相同的东西。您仍然可以安装/卸载/列出和切换 Node.js 的任何版本

# 摘要

借助 NVM，您可以实现:

*   如何安装/卸载任何版本的 Node.js 的简单方法
*   在节点间切换的最佳工具
*   拆卸和安装一样简单

将来你会感受到它的好处，尤其是当你需要更新 Node.js 的时候。

**感谢阅读！**