# 为什么以及如何在您的项目和团队中使用稳定的节点和 npm 版本

> 原文：<https://javascript.plainenglish.io/why-and-how-to-use-stable-node-and-npm-versions-across-your-project-team-a4886f51a96c?source=collection_archive---------2----------------------->

![](img/e0e7c77ec42d720eccc60137f8975748.png)

Picture courtesy of [@veverkolog](https://unsplash.com/@veverkolog)

我坚信开发环境应该尽可能稳定，以消除环境之间和开发人员之间的差异。当不去管配置时，配置有一种自然的漂移趋势，所以最好是尽可能多地自动化和强制执行。

这就是为什么我认为基于节点的项目应该在任何地方都使用 NodeJS 和 npm 的稳定和已知版本:本地开发、执行测试、构建生产二进制文件以及在生产中运行应用程序。

对于项目依赖项，这很容易实施，因为您可以在 package.json 中列出您的依赖项，并将 package-lock.json 或 yarn.lock 文件添加到代码库。

但是对于 node 本身和 npm 的版本，还需要做更多的工作。

在本文中，我将解释可以采取哪些步骤来确保您在任何地方都使用相同/预期的版本。

# 节点和 npm 的项目范围版本

您可以做的第一件事是在项目(或 monorepo)的根目录下创建文件，以声明要使用的版本。

在我的 monorepo 中，我创造了:

*   。npm 版本，仅包含 6.14.4
*   。nvmrc，包含 12.15.0

的”。npm-version“文件名是任意的，因为据我所知，对于这种特殊的需求没有约定，但是”。nvmrc”文件是著名的节点版本管理工具 [nvm](https://github.com/nvm-sh/nvm) 期望的[名。](https://github.com/nvm-sh/nvm#nvmrc)

添加了这两个文件后，我们现在有了一个供节点和 npm 版本使用的真实信息来源。

但仅此一项并没有多大作用。

# 定义 npm 引擎

我们可以做的第二件事是利用 npm 的 package.json 文件中一个非常有用(但不太为人所知)的[特性，称为](https://docs.npmjs.com/files/package.json#engines)[引擎](https://docs.npmjs.com/files/package.json#engines)。

在 package.json 文件中，我们可以通过“引擎”字段指定不同“引擎”应该使用的版本。

这里有一个例子:

```
"engines": {
  "node": ">=12.15.0 <= 12.99.0",
  "npm": ">=6.14.4 <= 6.99.99"
}
```

这告诉 npm，它应该期望项目与等于或高于 12.15.0 的节点版本和等于或高于 6.14.4 的 npm 版本一起使用。

我们当然可以更严格，但是在这个例子中我们还是坚持这样。

请注意，如果指定了引擎字段，npm 将期望在引擎列表中至少找到“节点”。

提示:如果你正在使用 yarn，你也可以指定它的版本；纱线[也支持它](https://classic.yarnpkg.com/en/docs/package-json/#toc-engines)。

# 使用 npm 实施 npm 引擎

默认情况下，对于 npm，引擎列表只是建议性的，只会产生警告。纱线不那么宽松，如果不遵守发动机清单，纱线会爆炸。

好消息是，我们可以通过设置 [**引擎严格的**](https://docs.npmjs.com/misc/config#engine-strict) 配置标志，配置 npm 来执行引擎列表。

我们可以指示所有的开发人员调整他们的 npm 配置，或者把它放在开发人员指南中，但是更好的方法是向项目中添加一个. npmrc 文件，以便自动执行它。

下面是一个示例:

```
# Make sure that we use the expected node and npm versions
engine-strict=true
```

有了这个，npm 会让我们知道我们是否试图用一个意想不到的节点或 npm 版本来构建/安装。

# 使用环境文件

我们可以采取的另一个在整个构建生命周期中利用我们的版本的步骤是使用“**”。env"** 文件。 [Env 文件](https://docs.docker.com/compose/env-file/)只是环境变量的列表。

在我的项目中，每当需要构建、测试、执行或部署项目时，我都会生成一个。该文件是从各种配置文件中派生出来的，我现在还不会涉及。

重要的是当我生成。环境文件，我读了我的。NPM-版本和。以便能够正确定义 NPM _ 版本和节点 _ 版本变量。

在我的基本变量文件中，我有以下内容:

```
export NODE_VERSION=$(cat ./.nvmrc)
export NPM_VERSION=$(cat ./.npm-version)
```

我只是呼应了我的。env 文件使用:

```
echo NODE_VERSION=${NODE_VERSION} >> .env
echo NPM_VERSION=${NPM_VERSION} >> .env
```

一旦完成了，我的。env 文件包含我期望的内容:

```
...
NODE_VERSION=12.15.0
NPM_VERSION=6.14.4
...
```

那个。env 文件很有用，因为它可以在各种上下文中加载(例如，Docker build、Docker compose、Kubernetes configMaps/secrets、shell 脚本、build 脚本等)。

我将在下面展示一些例子。

# 利用 Dockerfile 中的 env 文件

正如我在上一篇文章中所展示的[，我们可以将参数传递给 Dockerfiles，这正是我为 CI Docker 形象所做的。](https://medium.com/@dSebastien/speeding-up-your-ci-cd-build-times-with-a-custom-docker-image-3bfaac4e0479)

由于我们想强制执行节点和 npm 版本，我们需要将它们作为参数传递给 Dockerfile，以便它在构建映像时使用正确的版本。

如果您想了解更多信息，请阅读[文章](https://medium.com/@dSebastien/speeding-up-your-ci-cd-build-times-with-a-custom-docker-image-3bfaac4e0479)。

负责创建 Docker 映像的脚本也是如此；这里有一个例子:

```
#!/usr/bin/env bash

# Reference: https://stackoverflow.com/questions/19331497/set-environment-variables-from-file-of-key-value-pairs
# Import env vars
set -o allexport
source ./.env
set +o allexport

echo "-----------------------------------"
echo "Create some Docker image           "
echo "-----------------------------------"

echo "Building the Docker image for whatever"

# Build arg values are passed automatically because they have the same name in the Dockerfile
# Reference: https://vsupalov.com/docker-build-pass-environment-variables/#using-host-environment-variable-values-to-set-args
docker ${DOCKER_EXTRA_OPTIONS} build \
       ${DOCKER_BUILD_EXTRA_OPTIONS} \
       --build-arg DOCKER_BASE_IMAGE \
       --build-arg NODE_VERSION \
       --build-arg NPM_VERSION \
       --build-arg NODE_OPTIONS \
       ...
       --target ${DOCKER_BUILD_TARGET} \
       --file Dockerfile \
       --tag ${DOCKER_IMAGE_NAME}:latest \
       --tag ${DOCKER_IMAGE_NAME}:${PROJECT_COMMIT_HASH} \
       --tag ${DOCKER_IMAGE_NAME}:${PROJECT_VERSION} \
       .
```

通过将正确的参数传递给 Docker 文件，我们可以确保使用正确的基节点 Docker 图像。

此外，一旦 Dockerfile 具有 npm 和节点版本的环境变量，它就可以使用它们来安装预期的版本；例如:

```
npm install --global npm@${NPM_VERSION}
```

由于这一点，我们可以肯定，正确的版本将被使用。

此外，如果您像我一样使用 Docker 映像进行开发，那么您也可以确信 Docker/Docker-compose/Kubernetes 开发环境装载了预期的版本。

由于这些版本只有一个真实的来源，所以很容易适应:改变文件，重新构建/发布，就这样。

# 结论

本文介绍了一些有用的技巧，可以减少项目团队中开发人员之间的差异，同时保持开发和生产环境的一致性。

这些都是小而容易的步骤，但是它们可以极大地帮助减少“它在我的机器上工作”综合症的发生。

今天到此为止！