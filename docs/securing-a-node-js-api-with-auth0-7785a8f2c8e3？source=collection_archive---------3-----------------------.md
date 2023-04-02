# 使用 Auth0 保护 Node.js API

> 原文：<https://javascript.plainenglish.io/securing-a-node-js-api-with-auth0-7785a8f2c8e3?source=collection_archive---------3----------------------->

## 使用 Auth0 设置 Node.js API 的指南

![](img/92a42806b4143c096df20556814e1af6.png)

Made by the author in [Canva](https://www.canva.com/)

如果你读过我以前关于 Auth0 的文章，你就会知道我非常喜欢在新应用中使用它进行身份管理。在本文中，我将演示如何在 Auth0 admin 中创建一个 API，构建一个使用 Auth0 保护的 API，最后将介绍如何使用角色和权限管理用户访问。

要完成本文中的演示 API，您需要已经设置了一个 [Auth0](https://auth0.com/) 帐户，或者创建一个新帐户。如果你需要一些关于如何做到这一点的指导，一定要看看我以前的 Auth0 文章，我将在下面链接。

以前我写过一篇文章，介绍 Auth0，并为设置新的 React 应用程序和使用 Auth0 保护它提供指导。一定要看看下面这篇文章。

[](https://medium.com/javascript-in-plain-english/securing-react-applications-with-auth0-333180f8ea06) [## 使用 Auth0 保护 React 应用程序

### 使用 Auth0 引导新 React 应用程序的指南

medium.com](https://medium.com/javascript-in-plain-english/securing-react-applications-with-auth0-333180f8ea06) 

## 在 Auth0 中创建新的 API

在开始构建 Node.js API 之前，我们需要在 Auth0 admin 中设置一个新的 API。创建或使用现有帐户登录后，您需要导航到管理 UI 中的 API 部分。

![](img/d75be166396246a23410e151638e3ab7.png)

Screenshot by the author

要创建一个新的 API，你会看到一个对话框，需要输入一个`Name`和`Identifier`。Auth0 建议标识符为 URL。如果你知道你将把 API 部署到哪个域，你可以把它作为一个标识符。我将在演示中使用一个逻辑名称。

![](img/ca66d448d7acfbd1a3046e3c8b49616f.png)

Screenshot by the author

成功创建新 API 后，您将进入该 API 的设置视图。当我们稍后在这里设置我们的 API 时，您会想要记录这些设置。

![](img/036f36bfe3933558434732def6d41735.png)

Screenshot by the author

Auth0 将自动创建一个测试应用程序来测试您的新 API。您可以在下面的屏幕截图中看到，通过转到“机器对机器应用程序”选项卡，您可以看到这个测试应用程序。

在本文的后面，您将需要`client_id`和`client_secret`来生成访问令牌，以测试我们的 Node.js API。您可以通过点击新创建的测试应用程序的链接来找到这些设置。

![](img/ffe5f52a0d9f5e47b52b9ac1c2178f96.png)

Screenshot by the author

测试应用程序是一个机器对机器应用程序的例子，换句话说，它是一个使用承载令牌调用 API 的服务。这就是我们将在本文中用于演示的内容，但是请注意，您也可以使用在您的 Auth0 单页应用程序中生成的不记名令牌。

这就是本指南的初始管理部分。接下来，我们将设置 Node.js API。

## API

我们几乎准备好开始为我们的演示 API 编写代码了。但是在我们开始编写代码之前，我们需要创建一个新的 Node.js 项目，并通过 npm 安装一些依赖项。

创建一个新目录，并在该目录下的终端中输入以下命令。

```
npm init
npm install express express-jwt jwks-rsa express-jwt-authz
```

这一步是可选的，但是如果您希望 API 在您更改文件后自动重启，那么我建议将 *nodemon* 添加到依赖项中。

```
npm install --save-dev nodemon
```

我们的依赖项都已安装，现在我们可以在项目的根目录下创建`index.js`，我们将在那里创建服务器。下面是服务器文件的启动代码。

如您所见，对服务器没有什么影响。您只需要用您自己的 API 中的值替换`audience`、`issuer`和`jwksUri`。我们使用`jwt`中间件来验证授权头中发送的不记名令牌。请注意，因为我们已经在 express 应用的基础上设置了这个中间件，所以它将应用于我们创建的每个端点。

我们可以有选择地将它应用于一些端点，而不保护其他端点，但是如果我们像上面那样使用它，它将应用于我们创建的每条路由。

接下来，我们准备在终端中用下面的命令运行 API。

```
npx nodemon index.js
// or
node index.js
```

现在，随着 API 的运行，我们将使用 curl 为之前创建的测试应用程序生成一个访问令牌。使用以下命令生成令牌。必要时，确保用您自己的值替换示例值。

```
curl --request POST \
  --url [https://dev-gkawf0h0.us.auth0.com/oauth/token](https://dev-gkawf0h0.us.auth0.com/oauth/token) \
  --header 'content-type: application/json' \
  --data '{"client_id":"YOUR_CLIENT_ID","client_secret":"YOUR_CLIENT_SECRET","audience":"[https://auth0-nodejs-example](https://auth0-nodejs-example)","grant_type":"client_credentials"}'
```

您得到的结果将类似于以下内容:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImRaN2JYcUI3bkF4QTZ2LTZjQkZ2VSJ9.eyJpc3MiOiJodHRwczovL2Rldi1na2F3ZjBoMC51cy5hdXRoMC5jb20vIiwic3ViIjoiMWQwMVAzZExPS3VoSmxCU1IxQzBLdEx3ZW9yN3lmb3NAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vYXV0aDAtbm9kZWpzLWV4YW1wbGUiLCJpYXQiOjE2MDIzMDA4NjksImV4cCI6MTYwMjM4NzI2OSwiYXpwIjoiMWQwMVAzZExPS3VoSmxCU1IxQzBLdEx3ZW9yN3lmb3MiLCJndHkiOiJjbGllbnQtY3JlZGVudGlhbHMifQ.ZIVMFbtTVHaxpFNOBs-tYV_hL2gtLAuBQnlaO-b0ooDDF2Sbh8x4xiWGXlpqrtSez7v2wd-FGPTTGPzgvF_OaLHJRva9t-lHM4TfRk4CzrgxAS-I6xXlk5YPITL6BqnYEJk0aHZS_if5Hlp0s4UCiqjjNwPfew-WXAsLmgR3946LbnKY63omwcg2RX8FwD4MnzCHutiuCyRZNdXp4dyxSlAmdKtn3v49j7DGUt9ybj_k0nZ1AbEaqLtEpOInWP5C1IzWQ0aof-95Cv0VVKLuB3jabXI6BHIL0EUt-HFi8eCjR7Bv4apxcsSgyBmLwhqnY_Hko0n-FNp-rzis1NCp-A",
  "expires_in": 86400,
  "token_type": "Bearer"
}
```

您需要复制并粘贴发送的 access_token 的整个值。当我们调用 API 时，我们将把这个 JSON Web 令牌(JWT)添加到授权头中。

为了做一个快速实验来确保我们的端点是安全的，我们将尝试在没有端点的情况下调用 API。在终端中使用以下 curl 命令进行测试。

```
curl --request GET \
  --url [http://localhost:8000/secured](http://localhost:8000/secured)
```

结果应该如下所示:

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>UnauthorizedError: No authorization token was found<br> &nbsp; &nbsp;at middleware (/Users/matthewbrown/develop/auth0-node-example/node_modules/express-jwt/lib/index.js:79:21)<br> &nbsp; &nbsp;at Layer.handle [as handle_request] (/Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/layer.js:95:5)<br> &nbsp; &nbsp;at trim_prefix (/Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/index.js:317:13)<br> &nbsp; &nbsp;at /Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/index.js:284:7<br> &nbsp; &nbsp;at Function.process_params (/Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/index.js:335:12)<br> &nbsp; &nbsp;at next (/Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/index.js:275:10)<br> &nbsp; &nbsp;at expressInit (/Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/middleware/init.js:40:5)<br> &nbsp; &nbsp;at Layer.handle [as handle_request] (/Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/layer.js:95:5)<br> &nbsp; &nbsp;at trim_prefix (/Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/index.js:317:13)<br> &nbsp; &nbsp;at /Users/matthewbrown/develop/auth0-node-example/node_modules/express/lib/router/index.js:284:7</pre>
</body>
</html>
```

结果以一大块 HTML 的形式返回，但是您可以看到基本思想是，如果我们试图调用没有授权令牌的安全端点，就会显示这是`UnauthorizedError`。这很好，正是我们所期待的。

接下来，我们将再次尝试，但这次使用授权令牌。请注意，您将替换授权头中“载体”之后的令牌。

```
curl --request GET \
  --url [http://localhost:8000/secured](http://localhost:8000/secured) \
  --header 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImRaN2JYcUI3bkF4QTZ2LTZjQkZ2VSJ9.eyJpc3MiOiJodHRwczovL2Rldi1na2F3ZjBoMC51cy5hdXRoMC5jb20vIiwic3ViIjoiMWQwMVAzZExPS3VoSmxCU1IxQzBLdEx3ZW9yN3lmb3NAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vYXV0aDAtbm9kZWpzLWV4YW1wbGUiLCJpYXQiOjE2MDIzMDA4NjksImV4cCI6MTYwMjM4NzI2OSwiYXpwIjoiMWQwMVAzZExPS3VoSmxCU1IxQzBLdEx3ZW9yN3lmb3MiLCJndHkiOiJjbGllbnQtY3JlZGVudGlhbHMifQ.ZIVMFbtTVHaxpFNOBs-tYV_hL2gtLAuBQnlaO-b0ooDDF2Sbh8x4xiWGXlpqrtSez7v2wd-FGPTTGPzgvF_OaLHJRva9t-lHM4TfRk4CzrgxAS-I6xXlk5YPITL6BqnYEJk0aHZS_if5Hlp0s4UCiqjjNwPfew-WXAsLmgR3946LbnKY63omwcg2RX8FwD4MnzCHutiuCyRZNdXp4dyxSlAmdKtn3v49j7DGUt9ybj_k0nZ1AbEaqLtEpOInWP5C1IzWQ0aof-95Cv0VVKLuB3jabXI6BHIL0EUt-HFi8eCjR7Bv4apxcsSgyBmLwhqnY_Hko0n-FNp-rzis1NCp-A'
```

这次你应该会收到我们的成功信息:`{“success”:true}`

## 验证权限

现在我们已经完成了基本的工作，让我们继续授权或验证访问令牌，看看它是否包含对它试图在 API 上访问的内容的适当权限。

所以第一件事是我们需要跳回 Auth0 admin 来创建一些权限。遵循下面的截图，并在您的 API 中创建这些:

*   `create:users`
*   `read:users`
*   `delete:users`

![](img/15173cc6bd26828bb957c4ce330a8021.png)

既然我们已经为 API 定义了权限，我们将希望添加更多的端点来利用这些权限。将以下代码添加到现有的`index.js`文件中。

我们创建一个检查权限的函数，作为新端点的中间件函数。我们可以传递多个权限给 require，或者像上面所做的那样只传递一个权限。

如果我们用最新的更新运行 API，并将 URL 从之前的`curl`命令更新为指向`GET /users`，我们会收到以下消息:`Insufficient scope`

这正是我们所期望的，所以我们新的权限验证正在按预期工作。为了让它工作，我们可以简单地将这些权限添加到我们用来验证 API 的测试应用程序中。

![](img/8f497fc695fb52d3a99375eaf6c123c0.png)

Screenshot by the author

我们希望检查测试应用程序下的 APIs 选项卡中的一个或所有权限。一旦完成，我们将需要创建一个新的令牌，并确保它包含预期的权限。

有一个简单的方法来检查这一点。复制 JWT 并粘贴到 [JWT.io](https://jwt.io/) 中。这将对令牌进行解码，您可以很容易地将其作为 JSON 对象进行检查。

![](img/b10c10561507408d15f991a6e731b5db.png)

Screenshot by the author

如果我们检查 scope 属性中的 payload 部分，我们可以看到令牌包含我们刚刚添加的所有权限，因此如果我们使用新令牌再次调用 API，这次应该会成功。

*我希望这篇 Node.js 和 Auth0 入门指南对你有所帮助。Auth0 是一个强大的身份管理工具，我在生产应用程序中使用它有很好的体验，并且喜欢帮助其他人做同样的事情。感谢阅读！*