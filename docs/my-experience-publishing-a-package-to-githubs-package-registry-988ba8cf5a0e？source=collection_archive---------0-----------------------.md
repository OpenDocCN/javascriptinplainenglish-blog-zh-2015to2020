# 我将一个包发布到 Github 的包注册表的经历

> 原文：<https://javascript.plainenglish.io/my-experience-publishing-a-package-to-githubs-package-registry-988ba8cf5a0e?source=collection_archive---------0----------------------->

![](img/d761edea9cbc9d9dc8051ab0c1a01402.png)

Photo by [Samuel Zeller](https://unsplash.com/@samuelzeller?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在我最近进行的 React 本地组件库工作中，我遇到了一点阻碍——我需要在远程某个地方发布组件库，以便在通过 CI 管道构建的主应用程序中使用它。

我目前正在做的项目不是开源的，所以不幸的是，公共的 npm 包不是一个选项，私有的 npm 注册表将会非常昂贵。

我最初的解决方案是使用 [Verdaccio，这是一个自托管的 npm 注册表，有 Docker 映像可用。](https://verdaccio.org)

我在本地用 Verdaccio 做了一个快速测试，给我留下了非常深刻的印象，它是 npmjs.org 的替代品，安装和使用起来非常简单。

我可能会说，我将开始使用本地版本的 Verdaccio 来测试包的上传和下载工作，而不是使用`npm link`来本地测试包。

# Github 的包注册表来拯救？

在寻找自托管 npm 解决方案时，我偶然发现了 Github 包注册表。

现在 Github 似乎已经被微软接管，他们正在从存储代码的地方扩展到存储软件包的地方，我相信最终一切都与代码和项目有关。

具有讽刺意味的是，虽然这将是他们的最终目标，但在测试阶段，他们积极警告用户不要使用关键包的包注册表，我想如果服务下降或测试后他们不继续产品，他们会掩护他们。

通过与我的团队交谈，我发现不久前就已经有一个访问测试版的请求，所以当几天后 Github 发来一封电子邮件说我们已经注册了测试版时，看起来所有的部分都在一起了。

一旦你作为一个组织注册了测试版，你可以看到一个在组织的名称空间下发布的包的列表，在存储库级别，你可以看到使用该代码库发布的包。

Github 管理这些链接的方式首先是要求所有 npm 包的作用域与 Github 上的组织名称相同(例如，如果你的组织是`github.com/acmeinc`，那么你的包的作用域是`@acmeinc`)

其次，Github 使用来自您的`package.json`文件的存储库信息将发布的包链接到 Github 上的相关存储库。

为了发布，您还需要创建一个具有`read:packages`、`write:packages`和完整`repo`范围的身份验证令牌，然后使用此令牌登录您组织的 npm。

除了这些特定于平台的要求之外，使用 Github 包注册中心的 npm 端的设置类似于使用自托管的 npm 注册中心，在这里使用一个`.npmrc`文件并在`package.json`中添加一个`publishConfig`条目来告诉 npm 在哪里发布包。

# 薛定谔的贝塔入学

但是 Github 注册包的测试版有一个很大的问题，那就是当你查看 Github UI 时，你的组织看起来是在测试版中，而当你试图发布一个包时，你的组织看起来不是在测试版中。

我不确定这种情况的根本原因是什么，我只能假设注册表后端与 UI 不同步，因此需要额外的时间来准备接受包。

这个问题当然会破坏整个测试版的 UX，因为作为一个开发者，我对在 Github UI 上显示我的包不感兴趣，我想知道我可以运行`npm publish`，然后将包添加到我的依赖项中。

Github UI 真正的目的是让其他想要使用我的包的开发者了解它的存在，以及如何将它作为一个依赖项添加到他们的项目中。

对于大多数注册了 Github 软件包注册测试版的组织来说，我认为发布的新软件包数量相对较少，所以处于这种准注册状态是非常令人困惑的。

## 发布到 Github 包注册表的疑难解答

如果你试图用不正确的名称空间发布一个包(例如，你在 Github 上的组织是`ACMEinc`，但是你的包名称空间是`ACME`)，你可能会看到这个错误消息:

```
npm ERR! publish Failed PUT 402
npm ERR! code E402
npm ERR! You must sign up for private packages : @ACME/package-name
```

这个问题很容易解决，只需在 Github 上将包的名称空间更新为组织的名称。

对于 Github 不同于他们当前使用的名称空间的组织，我不确定这将如何与已建立的作用域包一起工作。

但是，如果您看到以下错误，那么您就处于无法发布的准注册状态，需要联系 Github 支持才能获得访问权限。

```
npm ERR! publish Failed PUT 400
npm ERR! code E400 
npm ERR! GitHub Package Registry is not enabled for user/org "ACMEinc".Unfortunately, we can't accept package uploads. : @ACMEinc/package-name
```

当我试图诊断这个问题时，我发现这些链接很有用:

*   无法发布的详细信息:[https://github.com/kubernetes/org/issues/812](https://github.com/kubernetes/org/issues/812)
*   在推特上发布回应，详细说明联系 Github 支持部门的必要性[https://twitter.com/jrsyo/status/1146636055153541122](https://twitter.com/jrsyo/status/1146636055153541122)

# 发布组件库

一旦我最终获得了发布到 Github 包注册表的权限，我就需要:

*   确保设置了正确的`main`属性，以便将库作为库而不是应用程序使用
*   确保最终的包中只包含了`src`和`assets`文件夹
*   确保库没有拉入消费库也需要的依赖项的副本，例如 React Native

因为我在组件库中使用 Expo 和 Storybook.js 来提供一个展示所有组件的运行应用程序，所以我需要将`main`条目作为`node_modules/expo/appEntry.js`，但是在打包组件库时，我需要将`main`条目作为`src/index.js`，这样我就可以导入组件。

我通过使用`json`包在`prepack`阶段覆盖`package.json`中的`main`属性为`src/index.js`，然后在`postpack`阶段将其恢复为`node_modules/expo/appEntry.js`来实现这一点。

Package.json with pre-pack and post-pack scripts

由于`prepack` / `postpack`挂钩在`npm pack`和`npm publish`之前和之后运行，这允许发布的包具有正确的`main`条目，同时确保 Expo 条目不会被删除。

为了删除用于存储单元测试、故事书设置和博览会信息的无关文件夹，我使用了`package.json`中的`files`属性来告诉 npm 只打包`src`和`assets`文件夹。

至于对等依赖，这只是将 React Native 及其相关库从 my `package.json`中的`dependencies`移动到`peerDependencies`，然后在安装组件库时确保安装对等依赖。

有了这些改变，我成功地发布了组件库，并开始在主应用程序中使用它。

# 使用组件库

为了使用我的组件库，我只需要添加`.npmrc`文件来告诉 npm 在 Github 包注册表中查找包，并将我的包包含在我的`package.json`中。

随着组件库的发布和添加到主应用程序中，我能够使用库中的组件，这反过来降低了主应用程序的复杂性，并确保显示信息和提供要显示的数据之间的关注点有明确的分离。

# 最后的想法

虽然设置起来有点麻烦，但 Github 的包注册表工作得非常好，我希望它支持许多不同的包管理器(类似于 Nexus 之类的东西),它将成长为一个繁荣的包生态系统，开发者不仅可以发布包，还可以利用 Github 提供的工具，如果他们做一些类似于他们的包自动化依赖审计工具的事情。

然而，我对开发周期中有多少将依赖于 Github 持谨慎态度，并且考虑到 Github 在过去被 DDoS 攻击的次数，这确实有可能导致一切停止。

与 git 不同，git 的设计本身就是分布式的，许多包注册表不是，所以除非组织使用现有的包注册表作为 Github 不可用的后备，否则他们会发现自己无法安装、打包和构建他们的应用程序和依赖项。

尽管如此，我还是很期待看到包注册中心是如何发展的，Github 是如何把它的项目、管道和 webhook 绑定到包注册中心的。