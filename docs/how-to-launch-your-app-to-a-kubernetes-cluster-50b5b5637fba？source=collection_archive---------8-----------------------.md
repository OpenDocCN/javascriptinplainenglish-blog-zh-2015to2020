# 通过 8 个步骤启动 Kubernetes 集群的应用程序

> 原文：<https://javascript.plainenglish.io/how-to-launch-your-app-to-a-kubernetes-cluster-50b5b5637fba?source=collection_archive---------8----------------------->

![](img/6334973b761bfcf32b3d713a4a18d7d6.png)

Photo by [Callum Galloway](https://unsplash.com/@callumgalloway?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

要求

*   谷歌云 SDK:[https://cloud.google.com/sdk/docs/](https://cloud.google.com/sdk/docs/)
*   https://kubernetes.io/docs/tasks/tools/install-kubectl/
*   https://docs.docker.com/install/[码头工人](https://docs.docker.com/install/)
*   https://nodejs.org/en/

回购链接:[https://github.com/raymundoxmartinez/StoreService](https://github.com/raymundoxmartinez/StoreService)

# **第一步:**创建一个 App

对于这一部分，我们将使用 express 创建一个基本的应用程序来生成一个简单的 web 服务器，而不使用视图引擎。

![](img/84f88c6278cccb4498348a760be5b370.png)

您应该得到以下文件结构:

![](img/9ae513fe26bdabee6d3ca722e952836d.png)

让我们对应用程序做一些更改。首先，我们将更改服务器正在监听的端口。

导航到 *bin/www*

改变这一点:

![](img/7b59584d9d4e2ac1fc5a59c3e6277e55.png)

对此:

![](img/3e3b8b96000aed71ecccc5a5e17b8a3a.png)

我们所做的只是将端口号从 3000 更改为 8080。

接下来，我们将编辑 */users* 路由的响应

导航至 *routes/users.js*

改变这一点:

![](img/afce8874eee46a268150e0922f5ec07c.png)

对此:

![](img/e0a15eb729ba196db8c0742de8108080.png)

现在运行您的服务器

![](img/a08e22711e66bd683683acd127cb7deb.png)

并在 Postman 上测试您的服务。

![](img/f1deb498eac31999459c80778f122221.png)

我们现在有一个工作服务！

# **步骤 2:创建 Docker 图像**

首先，我们将向根目录添加一个 Dockerfile 文件。

![](img/def402c4898868383f321f113c983b8d.png)

您的 docker 文件应该如下所示:

![](img/b8377bc1be59adff91ba6a71665e0d30.png)

注意， *CMD ["节点"，"。/bin/www"]* 是运行应用程序的命令。如果您有不同的命令(例如 *node server.js ),请更改该命令。*

下次运行:

![](img/828c6c70e39aa58b30c8e5eb6ab8a787.png)

在根目录中构建并标记 docker 映像。

您应该会得到这样的结果:

![](img/8aeba3373b8ee0ade0a2b7852dfe7523.png)

通过运行以下命令查看您的图像

![](img/b1961cbf215b02ed9e06dc07851dc021.png)

您应该能够看到构建图像的列表。

![](img/bf9470f726195a99a83392ad72b705be.png)

现在让我们运行容器！！

![](img/bcacbdb7d6ec32485bd8dcd4336acaec.png)

这里，我们将服务器 8080 的端口映射到容器端口 49160。

让我们在 Postman 上测试我们的容器:

![](img/742f7b99a331a56f4536eedace1f773d.png)

太好了！

现在我们正在与 docker 容器中的服务进行通信！

# **步骤 3:** 将图像推送到注册表

让我们在 Google 云平台中创建一个新项目。

登录 gcloud:

![](img/be0f5f7b150ee613a40e0cda003b1582.png)

现在授权您的 gcloud 帐户连接 docker:

![](img/6e713ea9569fcc8174f73b11c5ffa66b.png)

您应该看到这个:

![](img/e05cf0613066f878550429e5d69c004c.png)

现在，让我们标记映像并将其推送到 gcloud 存储库:

![](img/8324fa562ec116ca5b5ac6551ffd677a.png)![](img/16102945119e3fa953abcc56ea58bd93.png)

检查容器注册表以确保您的映像已成功推送。

![](img/d12988948750eb26db4ca1cb5c91a21a.png)

噪音。

# 步骤 4: 创建一个集群

转到 gcloud 平台，创建一个新集群。

![](img/1c9360af9f5f09755e36cfdd9ad59a2c.png)

将集群重命名为 *store-cluster* ，并将位置类型更改为 r*regional*。

![](img/75f90d7b1e7f1fa97b0ee2c33a70c616.png)![](img/e9f3376ee054110eb55da046c514e359.png)

现在确保您**启用了 istio 设置！！**

我们将使用此功能与集群通信。

# **步骤 5:配置设置，将 Kubernetes 与您的新集群连接起来**

连接到您在步骤 3 中创建的项目:

![](img/0de0ca91f2ba5c6b183f27046b26a9b5.png)

现在，连接到步骤 4 中创建的集群:

![](img/59caa4ca572994667e131e91e81e1e2d.png)

检查 kubernetes 是否正在与您的 gcloud 集群通信:

![](img/08c1776a5f8667b6731478c82b1f4248.png)

您应该看到 gke _ YOUR _ PROJECT _ ID _ REGION _ CLUSTER _ NAME

快到了！！！

# 步骤 6:创建一个定义服务的 Yaml 文件

在根目录中创建名为 store-service.yaml 的新文件

该文件包含 3 个部分:

*   服务
*   服务帐户
*   部署

**服务:**

![](img/33c8c5cd994071fdee366edd1760b866.png)

请确保在该部分的末尾包含“--”。

**服务账户:**

![](img/ffbb3a75067057f3ba3657b659da1da1.png)

**部署:**

![](img/da445cbcfa2e15d34dbea042179e4979.png)

将“URL_TO_DOCKER_IMAGE”替换为步骤 3 中推送到注册表的图像的 URL。

您可以在容器注册表部分找到 url

![](img/3f207cce4e812b3cfea18915a45895cd.png)![](img/ba860662786cfcd74d81f605a33acfe3.png)

点击*储存*储存库。

![](img/7b1d6f3803836aaf7d90678a086db8f5.png)

现在复制“完整映像名称”并将其添加到上面的部署部分。

让我们部署我们的容器吧！

![](img/1df78e10655a7c2bfe0c36f5623906a9.png)

检查服务是否已成功部署:

![](img/4448e2ce188b3a133a1ef67d408bc49b.png)

您应该看到:

![](img/e42bb1ad89725ce68028112baebaf76e.png)

检查已部署容器的状态:

![](img/a8478119df905c8553e06c9a2f99094d.png)

您应该看到:

![](img/e557050b2335114a45a323c521ce4362.png)

太好了，现在让我们打开我们服务的门户吧！！

# 步骤 7:创建网关 Yaml 文件

向根目录添加一个名为 store-gateway.yaml 的新文件。

该文件有两个部分:

*   门
*   虚拟服务

**网关:**

![](img/cd441ab2a1f73c9f217ca5708e041711.png)

**虚拟服务:**

![](img/57776e13dcedfdd45bee353b2fce07da.png)

部署网关:

![](img/cabbba5d65355ba47577e6daf60e3e0f.png)

检查它是否部署成功。

![](img/d0db65887726da12a560e2e1a7b317cf.png)

您应该看到:

![](img/ea36a01bee55bde66dc689259967d838.png)

现在是测试的时候了！:D

# 步骤 8:测试您的服务

让我们获取上面创建的网关的 IP 地址。

运行:

![](img/e39befc45cde481682cc2fce47cb5979.png)

您应该看到:

![](img/1018080486fed657030a6c1c758c5e25.png)

复制外部 IP。

现在，请致电 Postman 测试您的服务:

[***http://EXTERNAL-IP:80/users***](http://EXTERNAL-IP:80/users)

![](img/b9dcb6ca70764c556f38e5bbfb05b1d9.png)

令人满意！

现在你可以自由探索 docker、gcloud 和 kubernetes 的极限了。

祝你旅途愉快！！