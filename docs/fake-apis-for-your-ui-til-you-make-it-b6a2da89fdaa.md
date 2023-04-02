# 为你的用户界面伪造 API

> 原文：<https://javascript.plainenglish.io/fake-apis-for-your-ui-til-you-make-it-b6a2da89fdaa?source=collection_archive---------5----------------------->

## ('直到你成功！)

![](img/0d3b1df0bbe63131bc5706d077f3a02c.png)

很多时候我们只是想快速开始开发一个 UI 原型，没有太多的依赖。

但事实是，对于一个可用的现实世界的组件，我们经常需要获取一些数据，并让 UI 处理它，然后呈现一些东西。

然后我们要么:
——写一些快速的后端代码(用像 express，koa，happy 等后端框架。)
有几个 API，routes 返回一些哑数据或者数据库中的数据。
-从互联网上研究和利用一个假的 API 服务。

**对于选项 1**——编写我们自己的后端。我们要花很多时间用:
-哑数据，分页支持来创建后端 API。
-符合标准、合适的 API 接口。通常，这将导致从最初的想法分心，我们将有更少的时间来实现 UI(这可能是我们偶尔放弃宠物项目的原因)

**对于选项 2** —利用来自互联网的假冒 API 服务:
—需要互联网连接。
-依靠他们的稳定性。
-它们可能会有突破性的变化，你必须相应地调整你的代码。
-添加依赖关系—几年后它们还会存在吗？

**但是有一种更好的方法可以做到这一点……**

如果我们有一种简单的方法在本地启动我们的伪 API，并准备好一些虚拟数据供 UI 使用，会怎么样？

现在让我们探索一个名为 **API 的命令行工具！(api-now)** 。
只需在终端中键入 **$ npx api-now** ，它就会启动一个 api 服务器，在 HTTPS 的支持下提供 JSON、JS 文件或 faker 数据！

这为我们节省了大量修补后端的时间，所以我们可以专注于我们漂亮的 UI 原型，直到我们有时间投资后端设置。

**API-现在**有许多有用的特性，如:
-开箱即用的默认数据集:todos、用户、帖子、评论(使用 faker)。
- HTTPS 支持(带密钥、证书文件)。
-可以取一个. json 或者。js 文件。
-可以提供静态目录(例如/dist、/public 等。)—这可以替代 http-server 或 SimpleHTTPServer。
-API 支持分页(_page，_limit)。
- /echo 路由以 json 的形式返回响应参数。
-/文件路径服务于任何文件类型(包括图片)。
-/登录路由(POST)用一个伪 JWT 令牌响应(使用 jsonwebtoken)。
- /todos route 返回待办事项列表(遵循 TodoMVC 规范)。
- /image/random 提供一个目录中的随机图像文件。
-/头像/随机服务一个随机头像形象。
-/自然/随机服务一个随机的自然图像。

要尝试它，请准备好您的节点(谁不准备呢？)并运行这个命令行 **$ npx api-now** 。就是这样！您现在可以尝试(从另一个终端):

```
 $ curl [http://localhost:3003/todos](http://localhost:3003/todos)
$ curl [http://localhost:3003/users?_page=1&_limit=5](http://localhost:3003/users?_page=1&_limit=5) (others: /posts /comments)Other Useful Routes:
$ curl [http://localhost:3003/echo?any=value](http://localhost:3003/echo?any=value)
$ curl [http://localhost:3003/file?path=YourFilePath](http://localhost:3003/file?path=YourFilePath)
$ curl [http://localhost:3003/image/random?path=YourDirPath](http://localhost:3003/image/random?path=YourDirPath)
$ curl [http://localhost:3003/avatar/random](http://localhost:3003/avatar/random)
$ curl [http://localhost:3003/nature/random](http://localhost:3003/nature/random)
$ curl -X POST [http://localhost:3003/login](http://localhost:3003/login) -H 'Content-Type: application/json' -d '{"username": "test"}'
```

下面是一个使用 api-now 作为 UI 样板的示例项目:
[https://github.com/ngduc/parcelui](https://github.com/ngduc/parcelui)

现在你有更多的时间来享受修补你的 UI 项目的乐趣了！:)

**链接:**
[https://github.com/ngduc/api-now](https://github.com/ngduc/api-now)