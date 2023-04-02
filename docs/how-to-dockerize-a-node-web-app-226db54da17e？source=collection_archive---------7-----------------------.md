# 如何对节点 Web 应用程序进行停靠

> 原文：<https://javascript.plainenglish.io/how-to-dockerize-a-node-web-app-226db54da17e?source=collection_archive---------7----------------------->

![](img/b705c37fb5fe7c56b825ef5ad322acda.png)

Photo by [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Docker 中运行我们的 Node web 应用程序为我们省去了很多麻烦。

每次部署 Docker 映像时，肯定会部署一个新的容器。

这样，我们就不必担心弄乱容器中的代码。

此外，Docker 映像是用一个映像构建的，因此我们可以重复构建它，而不需要手动做任何事情。

在本文中，我们将了解如何对一个简单的节点 web 应用程序进行 Dockerize。

# 创建 Node.js 应用程序

我们从创建节点应用程序开始。

首先，我们创建一个项目文件夹，并运行`npm init --yes`来创建`package.json`。

然后，我们可以将它们中的所有内容替换为:

```
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "a simple app",
  "author": "",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

然后，我们在同一个文件夹中创建一个`server.js`文件，并添加:

```
'use strict';

const express = require('express');

const PORT = 8080;
const HOST = '0.0.0.0';

const app = express();
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

它只有一条回应“hello world”的路由。

# 创建 Dockerfile 文件

接下来，我们在项目文件夹中创建一个`Dockerfile`。

然后我们加上:

```
FROM node:12
WORKDIR /usr/src/app
COPY package*.json ./RUN npm install
COPY . .EXPOSE 8080
CMD [ "node", "server.js" ]
```

我们获得节点 12 映像，然后创建一个工作目录来构建映像。

然后我们用`COPY package*.json ./`把`package.json`和`package-lock.json`复制到根目录

接下来，我们运行`npm install`来安装包。

然后我们把应用的源代码和`COPY . .`捆绑在一起。

然后我们用`EXPOSE 8080`打开 Docker 镜像的 8080 端口。

最后，我们运行`node server.js`用`CMD [ “node”, “server.js” ]`启动 app。

接下来，我们创建`.dockerignore`文件来阻止 Docker 将本地模块复制到 Docker 容器中。

我们对 NPM 日志做同样的事情。

因此我们在`.dockerignore`中有以下内容:

```
node_modules
npm-debug.log
```

# 建立形象

然后，我们可以使用以下内容构建图像:

```
docker build -t <your username>/my-app .
```

其中`<your username>`是您帐户的用户名。

然后我们应该在运行`docker images`时看到我们的图像。

# 运行图像

构建完成后，我们可以运行我们的映像:

```
docker run -p 8888:8080 -d <your username>/my-app
```

`-d`以分离模式运行容器，使其在后台运行。

`-p`将容器中的公共端口重定向到私有端口。

然后我们可以运行`docker ps`来获取容器 ID。

应用程序的输出可以通过以下方式获得:

```
docker logs <container id>
```

我们可以带着:

```
docker exec -it <container id> /bin/bash
```

然后，为了测试我们的应用程序，我们可以运行:

```
curl -i localhost:8888
```

然后，我们应该从应用程序中获得“hello world”响应。

![](img/1301ae6acd4b0fde5e7b04335b455428.png)

Photo by [Roberto Nickson](https://unsplash.com/@rpnickson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一个简单的 Docker 文件为一个节点 web 应用程序创建一个 Docker 映像。

然后，我们不需要做太多工作就可以让它与 Docker 一起运行。

## **简明英语 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**