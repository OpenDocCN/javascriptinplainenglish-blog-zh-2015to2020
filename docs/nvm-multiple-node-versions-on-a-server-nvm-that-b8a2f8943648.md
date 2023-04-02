# NVM:一个服务器上有多个节点版本？为此使用 NVM！

> 原文：<https://javascript.plainenglish.io/nvm-multiple-node-versions-on-a-server-nvm-that-b8a2f8943648?source=collection_archive---------4----------------------->

![](img/67f929a053f64384c0b76bade2abcbce.png)

Photo by [Oladimeji Ajegbile](https://www.pexels.com/@diimejii?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/man-working-using-a-laptop-2696299/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

## 头痛

如果你曾经不得不在一台服务器上维护一个应用程序，你可能会花上几个小时，甚至几天，就像我们的朋友一样。尽管这可能很困难，但现实是一台服务器将用于托管多个应用程序，而不仅仅是一个。如果你使用 *Node.js* 来驱动你的应用程序，就像使用 *Angular* 、 *React* 、 *Ionic* 等制作的应用程序一样。，这会造成相当大的困境。考虑部署依赖于 Node.js 特定版本的应用程序，比如 v9.x.x。只要部署到该服务器的所有应用程序都依赖于相同版本的 Node.js，一切都很好。但是，如果你以这种方式解决问题，你将会失败。出于各种原因，开发人员可能希望开发依赖于 Node.js 更新版本的新应用程序，例如:

1.  Node.js 的新更新提供了有价值的增强，并可能修复了旧问题。
2.  像 Angular 和 React 这样的新版本框架需要新版本的 Node.js 才能工作。

如何解决这样的问题？您是否投入时间和资源来更新现有程序，以便与新的 Node.js 版本配合工作(肯定效率不高)？是否为 Node.js 的每个版本创建新的服务器(肯定不可行)？是不是只在 Node.js 的一个版本上开发(肯定不现实…弃用是个东西)？好头疼！

## 痛苦的黑仔

对我们来说幸运的是，Node.js 社区很棒，所以 nvm(没关系)担心所有这些。 [*NVM*](https://github.com/nvm-sh/nvm) *(节点版本管理器)*解决了我们所有的问题，而且安装起来相当容易。 **NVM 允许您在同一台服务器上安装和使用 Node.js 的不同版本。**您可以根据需求在 Node.js 版本之间切换，以构建和启动不同的应用程序。

**说明:**

1.  在托管我们所有问题的服务器上，运行以下 *cURL* 命令下载并安装 NVM:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```

2.为了安全起见，请关闭当前的终端窗口并打开一个新窗口。

**快速启动 NVM 命令**

安装 Node.js 的最新版本

```
 nvm install node #installs the latest version of Node.js 
```

安装特定版本的 Node.js

```
nvm install 6.14.4
```

列出所有已安装的 Node.js 版本

```
nvm list
```

切换或使用 Node.js 的特定安装版本

```
nvm use 6.14.4
```

设置打开新终端时使用的 Node.js 的默认版本

```
nvm alias default 6.14.4
```

我认为这就是医生给你开出的所有让你开始使用 NVM 并防止进一步头痛的处方。部署愉快！

**参考文献:**

[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)[https://stackoverflow . com/questions/47190861/如何使用 nvm 设置默认节点版本](https://stackoverflow.com/questions/47190861/how-can-the-default-node-version-be-set-using-nvm)