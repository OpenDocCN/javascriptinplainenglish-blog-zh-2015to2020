# 从羽毛和反应开始—第 2 部分

> 原文：<https://javascript.plainenglish.io/get-started-with-feathers-react-part-2-d071e29cb6a9?source=collection_archive---------3----------------------->

![](img/3668cf7c30fc5efb4a2175c3caf88a77.png)

Illustration by [Undraw](https://undraw.co/)

在我的 [**上一篇文章**](https://ingeniousambivert.medium.com/get-started-with-feathers-react-part-1-6b2b35a398c3) 中，我简要解释了为什么选择**反应**和**羽毛**来创建一个网络应用。现在，在本文中，我们将设置一个基本的 **Web** - **App** ，带有配置了 *REST API* 和*认证*的 **React 客户端**和 **Feathers** **服务器**。

# 所以让我们开始吧。

首先，我们将创建/设置 **React 应用程序/客户端**，然后是 **Feathers 应用程序/服务器。**

所以基本上有很多[的方式](https://reactjs.org/docs/create-a-new-react-app.html)来设置一个 **React App** ，但是我们会用最常见最直接的方式**——**使用[***创建——React——App***](https://create-react-app.dev/docs/getting-started/)。

> 先决条件:你需要有 [***节点***](https://nodejs.org/) 和[***NPM***](https://www.npmjs.com/package/qs)*(或者你也可以用 [***纱线***](https://yarnpkg.com/)*代替 ***npm*****

**那么我们用[***npx***](https://nodejs.dev/learn/the-npx-nodejs-package-runner)*(node . js 包 runner)在终端中运行 **create-react-app** 来生成我们的**客户端** *。****

```
***// cd to the directorynpx create-react-app client***
```

***![](img/0fcbae4cbe615545e532dfcf5f3e1cf1.png)***

***这将需要几分钟的时间来完成。***

***同时让我们生成**服务器，**类似于 **React** 有不止一种[方式](https://docs.feathersjs.com/guides/basics/starting.html#our-first-app)来创建 **Feathers App** 但是我们还是会使用最常见的方式——[***Feathers-CLI***](https://docs.feathersjs.com/guides/basics/generator.html)*。*在终端中打开另一个标签，输入以下命令。***

> ***先决条件:你需要在你的系统中全局安装****feathers-CLI***。****

```
***// cd to the directoryfeathers generate app***
```

***![](img/27ec796d36576fff244e17e58a411c1c.png)***

*****feathers-cli** 将开始一系列提示，根据您的需要配置服务器。重要的有:***

1.  ***语言偏好(**JavaScript**/**TypeScript**)—*JavaScript****
2.  ***包管理器的选择(**NPM**/**yarn**)—*NPM****
3.  ***API 类型( **RESTful** 和/或**实时** ) — *REST****
4.  ***测试库的选择(**Jest**/**Mocha+assert**)—*Jest****
5.  ***选择我们是否需要认证(创建认证*服务* ) — *是****
6.  ***林挺图书馆的选择(**ESLint**/**standards js**)——*ESLint****
7.  ***认证策略的选择(**本地**/**OAuth**)——*本地****
8.  ***用于认证的*服务*的种类— *用户*(可以根据使用情况而变化)***
9.  ***选择一个 [***数据库适配器***](https://docs.feathersjs.com/api/databases/adapters.html) 用于认证*服务—mongose****
10.  ***根据选择的*适配器指定*数据库连接*—MongoDB://localhost:27017/server*(也可以选择使用远程连接)***

***就是这样。我们已经成功地为我们的 Web-App 创建了一个**客户端** ( *反应*)和一个**服务器** ( *羽毛*)(当它们组合在一起时，就构成了 Web-App btw)。***

***现在，继续在不同的终端中运行**客户端**和**服务器**。***

```
***// cd to the respective directoriesnpm start (or yarn start)***
```

***您应该会看到类似这样的内容。***

***![](img/51cee716d5479fd8f1d81316cc41d86c.png)***

***Images created by [Carbon](https://carbon.now.sh/)***

***现在，两个项目(**客户机** & **服务器**)都已经启动并运行。让我们浏览项目目录，看看*生成器/CLI*创建了什么，并浏览其中的代码。***

## ***项目目录— ( src/)***

*   *****客户端*****

```
***├── src├── App.css├── App.js├── App.test.js├── index.css├── index.js├── logo.svg├── reportWebVitals.js└── setupTests.js***
```

***这里的 ***index.js*** 是 app 的根，它保存了主要的 **App** 组件，这就把我们带到了 ***App.js*** 。app 所有不同的*组件*都配置到 ***App.js.*** 中***

***app 的内部路由也在 ***App.js*** 中配置。我不会深入研究路由是如何工作的，但是路由是在一个 **React** 应用程序的不同组件视图之间的导航。如果你想了解更多，请看这里的。***

***然后将 ***App.js*** 作为依赖项插入到 ***index.js*** 中，呈现所有*组件*并生成一个 [*单页面应用。*](https://en.wikipedia.org/wiki/Single-page_application)***

****CSS* 文件由应用程序的样式组成。***

> ***现在可以忽略 *reportWebVitals.js* 和 *setupTests.js* 。***

*   *****服务器*****

```
***├── app.hooks.js├── app.js├── authentication.js├── channels.js├── index.js├── logger.js├── middleware│   └── index.js├── models│   └── users.model.js├── mongoose.js├── services├── index.js└── users├── users.class.js├── users.hooks.js└── users.service.js***
```

***在这里，类似于*的客户端* ***index.js*** 是服务器的根。它包含执行应用程序和启动服务器的代码。 ***app.js*** 保存了三个关键实例——***羽毛*** ， ***快递*** (RESTful 部分)，以及 ***猫鼬*** (数据建模为 [*猫鼬*](https://www.mongodb.com/) ) 包括所有配置) ***app.hooks.js*** 包含了 ***before、*** 和 ***错误钩子、*** 这些钩子运行在每个 ***HTTP 调用 ***app*** 的*** 上也就是说这些 ***钩子*** 将运行在每个上***

***接下来，***authentic ation . js***实现*本地*&*JWT*[*策略*](https://docs.feathersjs.com/api/authentication/strategy.html) 。它注册这些*策略*，并在我们的 **Feathers 应用**中配置 *expressOauth* 。服务注册在***'/认证'*** 路径上。***

***继续， ***channels.js*** 具有事件通道，这些事件通道由决定将[实时事件](https://docs.feathersjs.com/api/events.html)发送给哪些连接的客户端，以及发送的数据应该是什么样子。它在一个**服务器**上使用，带有*实时*传输([*socket . io*](https://docs.feathersjs.com/api/socketio.html)*或 [*Primus*](https://docs.feathersjs.com/api/primus.html) )设置。但是因为我们没有，我们可以选择忽略它。****

****接下来是 ***logger.js*** ，其中是用 [winston](https://www.npmjs.com/package/winston) 创建的一个简单的可插拔函数，用来记录应用程序的事件和动作。以及中的**中间件/** 用于注册 **Feathers App 中的任意一个 [express 中间件](https://expressjs.com/en/guide/using-middleware.html)。******

*****服务*的所有*数据模型(*用于 *MongoDB)* 存储在 **models/中。**它们是自动生成的(您可以稍后修改)。***mongose . js***由客户端为*的 MongoDB* 数据库驱动—***mongose***组成。****

****继续介绍**服务—**T42 羽毛**的**心脏。 ***服务*** 包含了我们**羽毛** **App** 的所有业务逻辑。 ***class.js*** 是我们写业务逻辑的地方。 ***service.js*** 配置服务路径，实例化类，创建模型。 ***hooks.js*** 与 ***app.hooks.js*** 包含 ***之前、*** 和 ***错误挂钩*** 为特定的 ***服务*** 。而 ***index.js*** 是所有服务注册到 **Feathers App** 的地方。****

****这是项目目录和其中代码的概述。****

****现在，我们可以进入下一步了。将 ***客户端*** 连接到 ***服务器。*******

****但在此之前，让我们测试一下我们的 ***认证****API*real quick*。*我通常使用[*Postman*](https://www.postman.com/)*，*来测试我的 API*，但是你可以使用任何你喜欢的 *REST 客户端**。******

***因此，启动 *REST 客户端*，用凭证对象(电子邮件和密码)向***"/用户"*** 发出一个 *POST* 请求。***

```
******url :*** *http:localhost:3030/users*// body type - JSON**body :**{"email":"monarch@maisuriya.com", // your email"password":"maisuriya" // your password}***
```

***点击发送后，您应该会收到类似以下内容的响应***

```
***{"_id": "5f98effe9958b369f665215d","email": "monarch@maisuriya.com","createdAt": "2020-10-28T04:13:50.397Z","updatedAt": "2020-10-28T04:13:50.397Z","__v": 0}***
```

***瞧啊。这意味着我们已经成功地用我们的 *API* 在我们的系统中创建了一个用户。并且用户具有上述属性。***

***现在让我们尝试对创建的用户进行身份验证。***

***在 *REST 客户端*中打开另一个标签页，向*发出 *POST* 请求。但是这一次我们需要给我们的凭证对象添加一个额外的属性—策略**，在我们的例子中是本地**。********

```
******url :*** *http:localhost:3030/authentication*// body type - JSON**body :**{"strategy":"local", // the auth strategy we chose earlier"email":"monarch@maisuriya.com", // email used to create the user"password":"maisuriya" // password used to create the user}***
```

***点击 send，您应该会收到一个类似如下的响应对象。***

```
***{"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6ImFjY2VzcyJ9.eyJpYXQiOjE2MDM4NTg5NTEsImV4cCI6MTYwMzk0NTM1MSwiYXVkIjoiaHR0cHM6Ly95b3VyZG9tYWluLmNvbSIsImlzcyI6ImZlYXRoZXJzIiwic3ViIjoiNWY5OGVmZmU5OTU4YjM2OWY2NjUyMTVkIiwianRpIjoiZDUyNzAzZjYtYTZhZS00NDgyLTg2MDYtODdlYTNmMzBjNzBlIn0.OMTAJO_cb9erx4SqHIfFUplQCy5a2CIS2-OfrATNqw4","authentication": {"strategy": "local"},"user": {"_id": "5f98effe9958b369f665215d","email": "monarch@maisuriya.com","createdAt": "2020-10-28T04:13:50.397Z","updatedAt": "2020-10-28T04:13:50.397Z","__v": 0}}***
```

***它有访问令牌(JWT)、用户详细信息和认证策略。***

*****现在你已经有了一个全功能的*认证*基础 *REST API。******

***你看到设置这个有多简单了吗？没有手动设置 Passport 和 JWT 以及本地策略或会话和 cookies，或者用任何 ORM 或 ODM 配置数据库的麻烦。羽毛为我们做了这一切。这不是很神奇吗？***

## ***现在下一步。将我们的 React 客户端连接到我们的 Feathers 服务器。***

***要做到这一点 [***羽毛方式***](https://docs.feathersjs.com/api/client.html#feathersjs-client) (这在我看来是相当容易的方式)我们将需要在中安装几个依赖*。所以打开 **React App** 目录中的终端，用 *yarn* (或者 *npm* )安装下面的依赖项。****

```
***yarn add @feathersjs/authentication-client @feathersjs/client @feathersjs/feathers @feathersjs/rest-client***
```

***因为我们已经添加了所需的依赖项。让我们来编写实际的代码，将 ***客户端*** 连接到 ***服务器*** 。***

***在 **src** 中创建一个名为**‘client’**的新目录。并在这个目录中创建一个 **index.js** 。并键入以下代码。***

***![](img/0aeb8dc8335f0ac2823c347d65512fe4.png)***

***Image created by [Carbon](https://carbon.now.sh/)***

> ***我使用 [***axios***](https://github.com/axios/axios) 作为 *HTTP 客户端。但是你可以使用任何你想要的其他客户端，只要把它作为一个依赖项添加进来。点击了解更多信息[。](https://docs.feathersjs.com/api/client/rest.html#feathersjs-rest-client)****

***这段代码是做什么的呢？***

***这相当简单。我们从**客户端**导入**羽毛**实例，从**认证客户端**导入**认证**实例，从 **rest 客户端**导入 **rest** 实例。***

***首先，我们实例化 **feathers()** ，然后实例化 **rest() —** 传递 *API_URL* (在我们的例子中— *http:localhost:3030* )作为参数。***

***注意——我使用 [**环境**](https://create-react-app.dev/docs/adding-custom-environment-variables/) 变量来存储默认信息，比如在**中的 API_URL。**客户端/** 的**根**处的环境*****

***其次，我们在 *feathersClient* 中配置 *restClient* 以及我们选择的 *HTTP Client* 。然后我们用 *feathers-authentication 配置 *feathersClient* 。****

**第三，我们在*中设置了 **auth()** 羽毛状* 与的一些典型选项。**

```
**// stores the jwt in the browser's local storage
storage: window.localStorage,// storage key for the jwt
storageKey: "feathers-react-jwt"**
```

**就是这样，我们已经成功地将我们的 **React 应用**连接到我们的 **Feathers 服务器**。现在，我们可以使用这个 *feathersClient* 让 *API 调用*到我们的***服务器*** *。***

## **现在是本文的最后一步。**

**让我们创建一些简单的 *React 组件*来组成一个 S ***ignUp*** 和一个 S ***ignIn*** 页面，这将允许我们与**服务器**进行通信，以便通过一个简单的用户界面*来创建和验证用户。***

**现在我不想从头开始创建这些组件。所以我使用 Chris Bakley 的 [React Forms Starter 模板](https://github.com/chrisblakely01/basic-react-forms)，它有几个风格得体的 React 表单。**

**我还在我们的 app 中使用了[***react-router-DOM***](https://www.npmjs.com/package/react-router-dom)*作为内部路由的依赖。***

***所以在将模板中的代码添加到我们的 ***客户端***之后，最终的 **App** 看起来是这样的。***

***![](img/22aa916d5b31cacaf749c6b93a5b3f48.png)***

***这里发生了什么？***

***这相当简单。我们有一个带有两个 CTA 按钮的登录页面— **注册**和**登录。**点击任意一个按钮，应用程序会将我们导航到不同的视图。***

*   *****注册表单**:这是一个简单的*反应组件*，有两个输入和一个提交按钮。所以让我们来看看代码。***

***![](img/6db65380dcbdf0ddc5cb037358346d76.png)***

***Images created by [Carbon](https://carbon.now.sh/)***

***这里到底发生了什么？让我们一步一步来。***

***首先，我们导入必要的依赖项，如*使用状态*挂钩和*羽毛客户端*。***

> ***如果你不熟悉 *React Hooks，*我建议你在继续前进之前先看看这里的。***

***所以正如我在我的 [**上一篇文章**](https://ingeniousambivert.medium.com/get-started-with-feathers-react-part-1-6b2b35a398c3) 中提到的， [***状态***](https://reactjs.org/docs/state-and-lifecycle.html) 对象是你存储属于组件的属性值的地方。由于我们使用 **React 的功能组件**，我们将需要使用*钩子*来执行与基于**类的组件**相同的操作。***

> ***为什么我选择了**功能组件**而不是**基于类的**？它完全是另外一个故事。如果你想了解它，你可以阅读这篇文章。***

***我们正在制作一个简单的**注册组件**。现在，因为我们将使用来自用户的凭证(电子邮件、密码)来创建用户，所以在将它们发送到**服务器**之前，我们需要将它们存储在组件中的临时**本地** **状态**中，这就是*使用状态*挂钩的作用。它允许我们创建一个组件的**局部** **状态**。因此，我正在创建一个具有电子邮件和密码属性的**状态**对象(称之为*凭证*)。我还创建了另一个**状态**对象- *结果*来存储我们从服务器得到的响应。***

***让我们跳到组件的 JSX 部分。我们将与它同时理解方法(*handleemailputchange，handlePasswordInputChange，handleSubmit* )。***

***因此，我们创建了一个基本表单，有两个输入(电子邮件和密码)和一个提交按钮。看起来相当简单，但是这里我们需要关注的是输入的两个属性——*value*和 *onChange* 。这里的 *value* 属性保存输入的值，我们从 *credentials* 对象中获取该值，这将我们带到下一个属性 *onChange* 。***

****onChange* 属性根据输入调用方法*handleemailputchange*或*handlePasswordInputChange*，并将输入的值更改为凭证对象的值。现在我们来看看这两种方法背后的大致思路。***

***这些方法接受一个参数`event`，这是一个 *javascript 事件。****

```
***// passing the credentials object
setCredentials((credentials) => ({// spreading the credentials object
...credentials,// changing the input (email/password) value
input: event.target.value,}));***
```

***那么，让我们转到 submit 按钮，单击该按钮将提交表单。现在让我们回到创建表单的最开始，表单有一个属性 *onSubmit* ，它在表单提交时调用一个方法。所以我们来看看提交表单时被调用的方法——*handle submit。****

***同样，这个方法接受一个参数`event`，这是一个 *javascript 事件。****

```
***// e = event// prevent the default behaviour of the form on submit
**e.preventDefault();**// A simple Try-Catch Block**try {**/* since this is an async method we are awaiting and then storing the response from the server in result */
**const result = await feathersClient
.service('users').create(credentials);**/* The main reason why we used the feathersClient - it allows us to call the API in this simple manner : 
*clientInstance.service('service name/route').HTTPMethod({params})* *//* using the set method to store the result from server in the result object we created earlier */
**setResult(result);****} catch (error) {** /*
Throwing the error in a catch statement defeats its purpose. I did it for testing purposes only. 
*/ **throw Error(error);
}*****
```

***在 Try-Catch 中， *feathersClient* 请求服务器在 *users* 服务中创建一个新用户。如果成功，服务器将返回一个响应对象，该对象将存储在*结果*对象中。我使用了 [*条件呈现*](https://reactjs.org/docs/conditional-rendering.html) 的概念，并且仅当*结果*对象不为空或者用户已经注册时，才呈现*链接*到的*登录表单*。看，那很容易！***

*   *****签到表**:和*签到表*一样，是一个简单的*反应组件*，有两个输入和一个提交按钮。但是让我们看一下代码。***

***![](img/8b5eaed39bed57d99ecdb6c7f18b1758.png)***

***Images created by [Carbon](https://carbon.now.sh/)***

***因此，如您所见，*签到表*与*签到表*非常相似。只有一些变化。让我们检查一下这些变化。***

***首先， *handleSubmit* 方法中的逻辑被改变。但它仍然采用相同的参数。***

```
***// e = event// prevent the default behaviour of the form on submit
**e.preventDefault();**// A simple Try-Catch Block**try {**/* Feathers Client provides an authenticate() strategy for authentication. Using that you can just call that method with the appropriate params and Feathers will authenticate the user, and return a JWT with other details. But you can also make a request with the default syntax, like the one we discussed above in the SignUp Form */
**await feathersClient.authenticate({strategy: 'local', ...credentials});**/* Now as I said, calling the authenticate() returns an object with JWT, so why are we calling the service again? Because I want the result object to work as a [computed property](https://daveceddia.com/computed-properties-in-react/), for our conditional render later, so I will let Feathers authenticate first and then request the response later */
**const result = await feathersClient.get('authentication');**// Store the result
**result ? setResult(result) : setResult(null);****} catch (error) {** /*
Throwing the error in a catch statement defeats its purpose. I did it for testing purposes only. 
*/
**throw Error(error);
}*****
```

***其次，表单末尾的条件呈现发生了变化，不再是一个*链接，*变成了一个*登出*按钮，并带有一个 *handleSignOut w* 的回调，该回调请求登出。***

```
*****feathersClient.logout()**
/* This method removes the access token from storage on the client. It also calls the remove method of the authentication service */***
```

***就这样，我们结束了。***

## ***我们已经用[的基本设置和配置](https://ingeniousambivert.medium.com/get-started-with-feathers-react-part-1-6b2b35a398c3) [创建了一个功能完整的 web 应用程序。](https://ingeniousambivert.medium.com/get-started-with-feathers-react-part-1-6b2b35a398c3)真了不起！！！***

***这就是 ***【羽毛&反应*** 的 **Part 2** 。感谢阅读:)***

*****Part 3** :自定义 **React App** 与 [***Ant 设计***](https://ant.design/) UI 库，实现 [***状态管理***](https://medium.com/better-programming/react-state-management-in-2020-719d10c816bf)*与*[***Redux-Toolkit***](https://redux-toolkit.js.org/)，并添加其他*必备安全特性*添加 [***认证管理***](https://github.com/feathersjs-ecosystem/feathers-authentication-management) 到**羽毛服务器。*******

***即将推出。敬请关注。***