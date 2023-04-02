# 提高 CI 绩效(也称为如何省钱)

> 原文：<https://javascript.plainenglish.io/improving-ci-performance-aka-how-to-save-your-money-31ff691360e4?source=collection_archive---------5----------------------->

## 如何使用缓存和 Docker 映像提高 CI/CD 管道速度

![](img/01d407e239f9077aa6bdcf1341c8735f.png)

Reducing the pipeline duration will save your money

如今，在一个项目中使用 CI 是必须的。在大多数情况下，我们必须为依赖项安装、项目构建、测试、部署所需的时间付费。如果你觉得这是你的情况，我会分享我的经验，如何显著提高 Angular app(不仅是)CI 的性能，并及时降低计费成本。

# 管道改进

## 依赖项安装

几乎每个管道都是从依赖项安装开始的。为了处理它，通常，我们使用简单的命令 **npm install** 或 **yarn** 。在我们的情况下，当我们将它作为 CI 的一部分运行时，我们检查包锁。如果它与 package.json 有相同的版本，它将安装锁文件中的所有内容。但是如果 package.json 有更新的版本，它将重新生成包锁文件。

## npm ci 而不是 npm 安装

这个命令:`npm ci`类似于`npm install`，除了它更适合在自动化环境中使用，比如持续集成。通过跳过某些面向用户的功能，它可以比常规的`npm install`快得多。此外，它有一个超级酷的特性，如果 package.json 在没有 package-lock 的情况下更新，它会报错。所以你可以确定，在不同的构建之间，你总是会有相同版本的依赖关系。普通的 npm 安装在这种情况下可以安装一个更新的版本并生成新的包——lock . JSON

## 带缓存的 npm ci

Npm 缓存位于`~/.npm`中，但是在大多数配置项中，您只能缓存工作目录中的内容。

为了避免这种情况，我们可以使用 **npm set cache 将缓存目录更改为项目中的本地文件夹。npm** 。NPM 缓存现在在`./.npm`中，所以现在它在 CI 作业之间缓存。

例如，我们使用 Gitlab CI，其中我们需要在。gitlab-ci.yml。

```
cache:key: ${CI_COMMIT_REF_SLUG}paths:- node_modules/- .npm/
```

之后，我们可以使用下一个命令安装依赖项:

```
npm ci — cache .npm — prefer-offline
```

现在，我们的 CI 使用缓存，并与我们的下一次管道运行共享它。

## 角度测试速度可以快 3 倍(直到角度错误存在，该问题在 2020 年 12 月 14 日有效)

几周前，我发现了一篇关于角度测试的[文章](https://bit.ly/3lTxfD9)。总的来说，这个问题是关于在每次测试后清理损坏的共享样式。框架在每次测试后添加一个带有共享样式的样式标签。在测试结束时，您可以在测试页面上拥有数千个共享样式的样式标签。

作者提供了一个临时的解决办法，如何解决这个问题，直到正式修复。

您只需在项目中添加几行代码:

在 CLI 项目中，打开 test.ts 并添加以下行:

```
import { ɵDomSharedStylesHost } from ‘@angular/platform-browser’;afterEach(() => { getTestBed().inject(ɵDomSharedStylesHost).ngOnDestroy();});
```

令人惊讶的是，它确实有效！分享评论对你有帮助吗？

# Docker 改进

根据 [2020 Jetbrains 开发者调查](https://www.jetbrains.com/lp/devecosystem-2020/)，44%的开发者现在使用 Docker 容器的某种形式的 CI 和 CD。所以我想分享一些常见的最佳实践。它们中的每一个都将提高任何 NodeJS 应用程序的性能，不管你使用 Angular、React、Vue 还是 vanilla js。

## 使用较小的 Docker 基本图像

四构建 Angular app 或简单 SSR 使用较小的 Docker 镜像就足够了，比如 Slim 和 Alpine Linux 修改。这将减少建设的持续时间，推动和拉动你的 CI 形象。

```
*# base node image* FROM node:12-slim
```

## 使用缓存来重建图像和重新运行管道

有了配置良好的缓存策略，您几乎可以立即重建您的映像。规则很简单:

1.  在复制源文件之前安装模块
2.  不要使用标签`LABEL build_number=”123"`

```
*# base node image*FROM node:12.20.0-slimWORKDIR /usr/src/appCOPY package*.json ./RUN npm ci
```

## 使用明确的映像版本，而不是最新版本。

当您使用最新/稳定的引用作为基础映像时，您不能确切地确定哪个版本将是最新/稳定的。因此，最好使用基础映像的精确版本。

```
FROM node:12.20.0-alpine3.10 as builder
```

## 主持您自己的 CI/CD runner

自己的 runner，首先能带来更好的网速。GCP/AWS/Digital Ocean 的网络带宽比内置的运行器好得多，所以你可以节省安装依赖项的时间。
其次，如果你需要，你可以使用 CPU/RAM 更好的机器，它会更快，但不确定它能节省你的钱😉。

## **不要在每次运行时安装所有依赖项**

如果使用带有预安装依赖项的 Docker，可以节省大量时间。至少你可以在 Docker 镜像中预装 CLI 之类的全局依赖。但是如果你在一个项目中工作，依赖项很少收到更新。这可能是一个好的选择，预安装所有的依赖项，并将您的源代码复制到正确的文件夹中。之后，您可以运行任何命令，而无需安装依赖项。

就是这样。有了这些简单的建议，你可以大大节省时间和金钱。

**感谢阅读！**如果你对构建 SEO 友好的 Angular 应用感兴趣，阅读[我以前的文章](https://medium.com/javascript-in-plain-english/how-to-make-a-fast-angular-seo-friendly-app-a6d769bfd8d2)可能会有用。