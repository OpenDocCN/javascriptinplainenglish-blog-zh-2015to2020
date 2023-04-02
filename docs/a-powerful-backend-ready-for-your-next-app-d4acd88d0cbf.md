# 一个强大的后端，为您的下一个应用程序做好准备🚀

> 原文：<https://javascript.plainenglish.io/a-powerful-backend-ready-for-your-next-app-d4acd88d0cbf?source=collection_archive---------4----------------------->

![](img/6e964051935d9c23f5c08d936c6d6129.png)

我主要是一名前端开发人员。

## 每当我开始一个新项目时，我总是陷入这样的困境:

*   哪个后端？
*   哪个数据库？
*   哪个托管提供商？
*   对于云服务提供商，我这次必须经历哪些复杂性？
*   如果我以后需要转到另一个医疗服务提供者，该怎么办？
*   我应该无服务器吗？
*   我需要身份验证吗？我应该为此使用第三方服务吗？
*   如何办理 HTTPS 证书的签发和续保？
*   配置项/光盘设置如何？
*   如何获得一个方便的本地开发环境来匹配生产部署？

Firebase 和 AWS Amplify 等服务一直很有吸引力。然而，由于按次付费模式，我对数据库设计有明显的限制(有很多去规范化)感到不舒服。我知道，我知道，这就是 NoSQL 的工作方式，但我仍然不开心。此外，我不想因为编码错误而在 72 小时内[支付 3 万美元。然后](https://hackernoon.com/how-we-spent-30k-usd-in-firebase-in-less-than-72-hours-307490bd24d)[就不能做一个基本的全文搜索](https://firebase.google.com/docs/firestore/solutions/search)！

无服务器在小规模上可能非常经济，但在大规模上可能相当昂贵。我想要一个可预测的预算。

此外，云后端的本地开发的容易性也值得关注。对于所有这些问题，有许多好的解决方案或变通方法，但是对于不同项目的需求，没有一个觉得那么容易和灵活。

所有这些，我们甚至还没有开始谈论我最关心的东西，我的应用！

前端开发已经变得足够复杂，以至于也会被所有这些问题分散注意力。

最近，我遇到了[解析服务器](https://github.com/parse-community/parse-server)。这是一个由脸书(parse.com)收购的项目，后来当他们决定停止提供云服务时，该项目是开源的。这是一个如此好的项目，几乎吸引了我所有的注意力。

因此，我创建了一个 monorepo starter 项目，该项目由[Docker composite](https://docs.docker.com/compose/)管理，该项目运行了功能丰富的即用型设置，可在本地开发中使用，并且易于部署。

您可以在 GitHub 上找到它:

[](https://github.com/hatemhosny/parse-starter) [## hatemhosny/parse-starter

### 这是一个初学者模板，带有一个功能丰富的即用型后端，可用于本地开发和…

github.com](https://github.com/hatemhosny/parse-starter) 

# 功能摘要

[**解析服务器**](https://github.com/parse-community/parse-server) :具有以下功能的后端即服务(baa):

*   用于流行平台的 SDK
*   [REST API](https://docs.parseplatform.org/rest/guide/)
*   [Graphql API](https://docs.parseplatform.org/graphql/guide/)
*   实时应用的 LiveQuery
*   安全特性包括认证、[用户、](https://docs.parseplatform.org/js/guide/#users)、[角色、](https://docs.parseplatform.org/js/guide/#roles)、[访问控制列表(ACL)](https://docs.parseplatform.org/rest/guide/#object-level-access-control) 和[类级权限(CLP)](https://docs.parseplatform.org/rest/guide/#class-level-permissions)
*   [第三方认证](https://docs.parseplatform.org/parse-server/guide/#oauth-and-3rd-party-authentication)
*   [推送通知](https://docs.parseplatform.org/parse-server/guide/#push-notifications)
*   用于[文件存储](https://docs.parseplatform.org/parse-server/guide/#configuring-file-adapters)和[缓存](https://docs.parseplatform.org/parse-server/guide/#configuring-cache-adapters)的适配器
*   [分析学](https://docs.parseplatform.org/rest/guide/#analytics)
*   [云代码](https://docs.parseplatform.org/rest/guide/#cloud-code)用于定制服务器端逻辑
*   [网钩](https://docs.parseplatform.org/rest/guide/#hooks)
*   运行在 [Express](https://expressjs.com/) 之上，允许使用 Express 中间件
*   综合[文档](https://docs.parseplatform.org/)
*   大型[社区](https://github.com/parse-community)

[MongoDB数据库。](https://www.mongodb.com/)

[**解析仪表板**](https://github.com/parse-community/parse-dashboard) (可选):管理解析服务器的强大仪表板。

**API-First 无头 CMS** (可选):使用[凿子-cms](https://chiselcms.com/) 。

一个样例实时**前端 app** 。

**使用 [Caddy 服务器](https://caddyserver.com/)对前端和后端进行自动 HTTPS** 。

可重复设置使用由单个 [**Docker 组成的**](https://docs.docker.com/compose) 文件管理的 [**Docker**](https://www.docker.com/) 容器。

**本地开发工作流程**，前端和后端热重装。

**易部署**。

**CI/CD** ( [持续集成和部署](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)):使用 [github 动作](https://docs.github.com/en/free-pro-team@latest/actions)。

可选的**部署到多种环境**(例如开发、试运行和生产)。

高度**可配置**。

整个堆栈都是开源的，没有供应商限制或按请求付费限制。

# 入门指南

运行 shell 命令:

```
docker-compose up
```

默认情况下，将提供以下服务:

*   解析服务器后端:[https://localhost:1337/API](https://localhost:1337/api)
*   解析 graph QL API:[https://localhost:1337/graph QL](https://localhost:1337/graphql)
*   解析仪表板:[https://localhost:1337/dashboard](https://localhost:1337/dashboard)
*   前端本地开发服务器(带 HMR):[https://localhost:1234](https://localhost:1234)

在[生产构建](https://github.com/hatemhosny/parse-starter#production-build)之后:

*   前端 app: [https://localhost](https://localhost)

当 [CMS 被启用](https://github.com/hatemhosny/parse-starter#headless-cms)时:

*   凿 CMS:[https://localhost:1337](https://localhost:1337)

现在，您可以编辑/替换`frontend`目录中的应用程序，并开始利用功能丰富的后端构建自己的应用程序。

# 部署

Docker 和 Docker Compose 大大简化了部署。docker 映像中已经考虑了所有的设置和依赖关系。

因此，原则上，部署所需的步骤是:

*   定义部署环境的变量。
*   构建 docker 映像并验证它们。
*   在主机服务器上运行容器。

虽然这可以手动完成，但是使用包含的使用 GitHub 动作的自动化 CI/CD 设置可以大大简化。

# 快速启动

假设您可以使用 SSH 连接到您安装了 Docker 和 Docker Compose 的服务器(参见[服务器设置](https://github.com/hatemhosny/parse-starter#server-setup))，并且您有一个 GitHub 个人访问令牌(参见[容器注册表](https://github.com/hatemhosny/parse-starter#container-registry)，添加以下 [Github 秘密](https://github.com/hatemhosny/parse-starter#github-secrets):

*   你的个人 github 访问令牌
*   PROD_SSH_HOST:您的服务器 IP 地址
*   PROD_SSH_KEY:您的服务器 SSH 私有密钥
*   PROD_ENV_VARS:使用您的值编辑以下示例

```
HOST_NAME=mywebsite.com
  APP_ID=myappid
  MASTER_KEY=mymasterkey
  PARSE_DASHBOARD_USER_ID=user
  PARSE_DASHBOARD_USER_PASSWORD=pass
```

注意:远程部署需要环境变量`HOST_NAME`。

现在，将代码推送到主分支应该会触发构建并部署到您的服务器。请注意，您可以在 GitHub repo 中的“Actions”选项卡上跟踪 CI/CD 工作流的进度并阅读其日志。

# 就是这样！

您已经在本地启动了强大的后端，并在几分钟内完成了部署。

你可以在 github repo:[https://github.com/hatemhosny/parse-starter](https://github.com/hatemhosny/parse-starter)中找到文档和配置细节

这显然不是解决世界所有问题的灵丹妙药，但它让我的生活变得更轻松，我希望它也能让你的生活变得更轻松。

请让我知道您的建议/意见/评论，我将非常感谢您的贡献。

快乐编码，去构建一些伟大的应用程序吧！

*最初发布于* [*dev.to*](https://dev.to/hatemhosny/a-powerful-backend-ready-for-your-next-app-55a)