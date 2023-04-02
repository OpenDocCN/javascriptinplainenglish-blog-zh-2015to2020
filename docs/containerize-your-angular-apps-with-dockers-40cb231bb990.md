# 用 Dockers 容器化你的 Angular 应用

> 原文：<https://javascript.plainenglish.io/containerize-your-angular-apps-with-dockers-40cb231bb990?source=collection_archive---------10----------------------->

## *了解如何为 Angular* ⛴构建轻量级和可移植的软件容器

![](img/874bc38fd7025d81181d13707fdfe1cb.png)

在这篇文章中，我们来看看这个示例项目，并一步一步地了解如何将 Angular 应用程序与作为服务器的 node js 封装在一起。

*   ***什么是码头工人？***
*   ***容器化你的棱角 App***
*   ***在 Docker 上运行应用***
*   ***多级 Docker 构建***
*   ***结论***

# 什么是码头工人？⚓🚤 ⛴

> Docker 是一个工具，旨在通过使用容器来简化应用程序的创建、部署和运行。容器允许开发人员将应用程序与它需要的所有部分打包在一起，比如库和其他依赖项，并作为一个包进行部署。

简而言之，Docker 使我们的应用程序完全可移植，因此它们可以在任何环境中运行。有了 dockers 的帮助，我们再也不用担心庞大的包或库依赖了。即插即用。Docker 是用 [Go](https://golang.org/) 编写的，它利用了 Linux 内核的几个特性来提供它的功能。

# 容器化角度应用

闲聊够了！让我们开始创建 docker 容器。在本文中，我们将使用一个我很久以前构建的简单的角度应用程序。如果您愿意，也可以使用您的应用程序，并按照正确的顺序执行这些步骤。

首先，我们将从我的存储库中克隆项目:

```
[https://github.com/ahmedkhan1/ng-content-projection.git](https://github.com/ahmedkhan1/ng-content-projection.git)
```

现在您已经在本地目录中获得了项目，确保您已经安装了所有必需的依赖项。在这种情况下，它的角和节点 js。

如果安装了所有的依赖项，请打开终端并在项目的根目录中键入以下命令。

```
ng build --prod
```

该命令将创建一个名为 **dist** 的新文件夹，其中将包含项目中所有已编译的文件。

下一步是创建一个 **docker 文件**。

## Docker 文件

**dockerfile** 是一个文本文档，包含用户可以用来从 **Docker Hub** 获取 Docker 图像的所有命令/步骤。

## Docker 图像

Docker 图像只是包含创建 Docker 容器所需的所有信息的模板。我们可以创建自己的 docker 图片，也可以使用其他人创建并发布在 Docker Hub 上的图片，Docker Hub 是 Docker 图片的公共注册中心。

在项目的根目录下创建名为`Dockerfile`的 docker 文件，然后输入以下几行:

```
**FROM** nginx:1.17.1-alpine
**COPY** /dist/ng-content /usr/share/nginx/html
```

这些命令执行以下操作:

*   中的 ***将指示 docker 前往 Docker Hub 并获取特定的 Nginx docker 映像***
*   ***Copy*** 后面跟一个路径会简单的把那个图像复制粘贴到指定的路径。

回到终端，键入以下命令

```
docker image ls
```

该命令将显示当前本地系统中所有可用的图像。目前，它不会显示任何内容。

接下来，键入以下命令来构建容器。

```
docker build -t docker-app-image
```

这里:

*   **-t** 标志用于指示 docker 使用标签
*   在 **-t** 标志旁边，我们将键入容器的名称。

输入上面的命令后，检查本地可用的 docker 图像。

```
docker image ls**REPOSITORY**        **TAG**            **IMAGE ID **     
docker-app-image  latest         a160a7494a19      
nginx             1.17.1-alpine  ea1193fd3dde
```

如您所见，我们已经成功获取了 docker 映像。

这里:

*   存储库名称是图像名称
*   标签只是用来区分它
*   图像 id 是每个 docker 图像拥有的 ID。它可以用来删除这个图像

现在要运行映像，只需运行以下命令:

```
docker run --name docker-app-container -d -p 8080:80 docker-app-image
```

这里:

*   `run`用于引用容器的开始。
*   `- -name`用于为容器命名，在本例中为 **docker-app-container**
*   `-d`标志将保持容器在后台运行。
*   `-p 8080:80`用于你映射容器端口到你的本地，80 是 Nginx 服务器。
*   最后，在最后，您将提供可供使用的图像的名称。

现在让我们检查可用的容器。

```
docker container ls**CONTAINER ID**  **IMAGE  **           **STATUS         NAMES**
2523d9f77cf6  docker-app-image  Up 10 minutes  docker-app-container
```

`docker ps -a`显示运行和停止的容器。

`docker stats --all`显示所有容器的列表，默认显示刚运行的。

我们已经成功地创建了 docker 的容器，并让它运行起来。这不是最有效的方法，但是我们可以通过简单地将`build - -prod`命令传递到 docker 构建中来提高效率。这将使过程更加自动化和干净。

为了实现这一点，我们将不得不使用所谓的 [**多级 Docker 构建**](https://docs.docker.com/develop/develop-images/multistage-build/) **。**

# 多级码头建造

通过多阶段构建，您现在可以在 docker 文件中使用多个`FROM`语句。每条`FROM`指令可以使用不同的基础，并且它们中的每一条都开始了构建的新阶段。因此，基本上我们将 docker 文件分成几个阶段，通过这种方法，我们可以将一个阶段的结果用于另一个阶段。

## 第一步:

在这个阶段，我们将把我们的 Angular 应用程序代码编译成一个生产就绪的代码。

## 第二步:

接下来，我们将在 docker 映像中运行它。

为了编译 Angular 源代码，我们将使用包含 [Node.js](https://hub.docker.com/_/node/) 的 Docker image。

```
**FROM** node:12.7-alpine **AS** build
**WORKDIR** /usr/src/app
**COPY** package.json ./
**RUN** npm install
**COPY** . .
**RUN** npm run build
```

这里:

*   `FROM`正在从 Docker Hub 中获取*节点*的 Docker 映像，并且也是在多阶段 Docker 构建中，我们现在可以使用关键字`AS`来命名每个阶段，在本例中该阶段被命名为`build`。现在，我们可以在下一阶段参考它。
*   `WORKDIR`设置默认工作目录。
*   `COPY`正在从本地项目根目录复制`package.json`文件，其中包含我们的应用程序需要的所有依赖项。
*   `RUN`安装库。
*   `COPY`用源代码复制所有剩余的文件。
*   `RUN`将运行构建命令来编译我们的应用程序。

现在让我们创建一个名为`.dockerignore`的文件。在这里，我们可以定义希望 Docker 忽略的文件和文件夹。

```
dist
node_modules
```

现在，我们最终的 docker 文件将如下所示:

```
*### STAGE No 1: Build ###*
**FROM** node:12.7-alpine **AS** build
**WORKDIR** /usr/src/app
**COPY** package.json ./
**RUN** npm install
**COPY** . .
**RUN** npm run build*### STAGE No 2: Run ###*
**FROM** nginx:1.17.1-alpine
**COPY** --from=build /usr/src/app/dist/ng-content /usr/share/nginx/html
```

在上面的代码中，您可以看到在阶段 2 中，我们引用了在步骤 1 中使用`As`分配的名称 build 的阶段 1。这将告诉 Docker 从阶段`build`复制所有文件。

现在让我们创建新的图像:

```
docker build -t docker-app-multistage-image
```

最后，在不同的端口上运行应用程序。

```
docker run --name docker-app-multistage-container -d -p 8888:80 docker-app-multistage-image
```

现在进入浏览器，输入 [http://localhost:8888/](http://localhost:8080/) 你会看到它正在运行！

# 结论🎉

> 我希望这篇文章对您有所帮助，如果您有， ***请关注我的*** [***中型***](https://medium.com/@mrahmedkhan019) ***和***[***Twitter***](https://twitter.com/50shadeofkhan)***以获取更多关于软件开发文章的通知，不要忘记点击*** 。终于！感谢阅读，快乐学习！

[](https://medium.com/javascript-in-plain-english/angular-regime-series-tree-shaking-technique-3dc07f5e85a1) [## 树木倾斜地摇晃着

### 用温柔的方式，你可以撼动世界！

medium.com](https://medium.com/javascript-in-plain-english/angular-regime-series-tree-shaking-technique-3dc07f5e85a1) [](https://medium.com/javascript-in-plain-english/a-deep-dive-into-web-components-3f7d6713da9e) [## 对 Web 组件的深入研究

### 在本文中，我们将深入研究 Web 组件，并了解为什么前端开发人员迫切需要…

medium.com](https://medium.com/javascript-in-plain-english/a-deep-dive-into-web-components-3f7d6713da9e) [](https://medium.com/@mrahmedkhan019/angular-regime-series-a-guide-to-change-detection-strategy-2a8a4da46c5c) [## 角度范围系列:变化检测策略指南

### 开发可持续变化的应用程序！

medium.com](https://medium.com/@mrahmedkhan019/angular-regime-series-a-guide-to-change-detection-strategy-2a8a4da46c5c) 

# **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击 点击 [**查看我们，并确保订阅该频道😎**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)