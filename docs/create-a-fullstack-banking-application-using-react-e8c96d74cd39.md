# 使用 React 创建全栈银行应用程序

> 原文：<https://javascript.plainenglish.io/create-a-fullstack-banking-application-using-react-e8c96d74cd39?source=collection_archive---------0----------------------->

## 第 1 部分:使用 PostgreSQL、Express、React 和 Node.js 堆栈创建应用程序

![](img/2dd01b3be9fe7813b1077d757ed3bac2.png)

Photo by [Burst](https://unsplash.com/@burst?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们将使用 PostgreSQL、Express、React 和 Node.js 创建一个银行应用程序。

通过构建这个应用程序，您将了解到

1.如何从 Node.js
2 使用 PostgreSQL 数据库？如何使用 JWT 认证来保护你的 API？如何保持登录会话，直到用户手动注销
4。如何从 Node.js 中执行 PostgreSQL 中的 CRUD 操作
5。如何动态生成 pdf 以获取所选日期范围内的交易列表
6。如何自动下载任何类型的文件，包括 pdf 文件。
还有更多。

这是多部分系列的第一部分。让我们从第一部分开始。

## 我们开始吧

使用`create-react-app`创建一个新项目

```
create-react-app fullstack-banking-app
```

项目创建完成后，删除`src`文件夹中的所有文件，并在`src`文件夹中创建`index.js`文件。同时在`src`文件夹中创建`actions`、`components,`、`css`、`reducers`、`router`、`store`和`utils`文件夹。

安装必要的依赖项

```
yarn add axios@0.19.2 bootstrap@4.4.1 history@4.10.1 jwt-decode@2.2.0 lodash@4.17.15 moment@2.25.3 node-sass@4.14.1 react-bootstrap@1.0.1 react-redux@7.2.0 react-router-dom@5.1.2 redux@4.0.5 redux-thunk@2.3.0
```

使用以下代码在`components`文件夹中创建一个新文件`Login.js`

在`utils`文件夹下新建一个文件`constants.js`，内容如下

```
export const SIGN_IN = 'SIGN_IN';
export const SIGN_OUT = 'SIGN_OUT';
```

在`reducers`文件夹下新建一个文件`auth.js`，内容如下

在`store`文件夹下新建一个文件`store.js`，内容如下

这里，我们已经创建了一个 redux 存储，目前只有一个身份验证缩减器。我们还为 redux dev 工具添加了配置，以查看调度的实时操作。要了解如何配置它，请单击此处的

在`router`文件夹下新建一个文件`AppRouter.js`，内容如下

在`css`文件夹下新建一个文件`main.scss`，内容如下

在`src`文件夹中创建一个新文件`index.js`，内容如下

现在，您可以通过执行`yarn start`命令来运行应用程序，并验证是否显示了`Login`组件。

在`utils`文件夹下创建一个新文件`common.js`，内容如下

现在，用以下内容更新`Login.js`组件

在`components`文件夹中创建一个新文件`Register.js`，内容如下

在`Login`和`Register`组件中，我们只是显示了一个表单，并添加了验证来检查是否输入了所有数据。

现在，如果您检查应用程序，您会看到显示登录和注册表单，并且当我们不输入任何数据时，还会显示一条错误消息

![](img/e5309772921787a357f7c0c23d3d3cc7.png)![](img/584ea8330839ea7fb0465ef9de3a6c18.png)

Login and Register Page

现在，让我们使用 Nodejs 创建一个后端来处理登录功能。

在`src`文件夹旁边创建一个新的`server`文件夹。因此您的项目现在将包含四个文件夹，即`node_modules`、`public`、`server`和`src`。

从终端的`server`文件夹中执行以下命令

```
yarn init -y
```

这将在`server`文件夹中创建一个新的`package.json`文件

现在在`server`文件夹中创建`db`、`middleware`、`routes`、`utils`和`views`文件夹。同样，在`server`文件夹中创建`index.js`

现在，从`server`文件夹中安装 npm 包

```
yarn add axios@0.19.2 bcryptjs@2.4.3 cors@2.8.5 dotenv@8.2.0 express@4.17.1 jsonwebtoken@8.5.1 moment@2.25.3 pg@8.0.3 nodemon@2.0.4
```

在`server/db`文件夹中，新建一个文件`connect.js`，内容如下

在 connect.js 文件中提供您的`postgresql`数据库连接细节。

在`server`文件夹下创建一个新文件`scripts.sql`，内容如下

PostgreSQL Database scripts

连接到 PostgreSQL 数据库，复制并粘贴来自`scripts.sql`的命令，以创建数据库和表。

在`utils`文件夹中，新建一个文件`common.js`，内容如下

在`server/middleware`文件夹下新建一个文件`auth.js`，添加以下内容:

在`routes`文件夹中，添加`auth.js`，代码如下

在`server`文件夹中新建一个`.env`文件，内容如下

```
secret=ThisIsMySecretKey
```

现在，让我们创建一个 express 服务器。在`server/index.js`文件中添加以下代码。

在`package.json`中添加一个新的`start`脚本

```
"scripts": {
 "start": "nodemon index.js"
}
```

让我们来理解来自`routes/auth.js`的代码

为了查询 PostgreSQL 数据库，我们使用了一个连接池来处理多个查询并提供更快的响应。

我们在`db/connect.js`中创建了池对象，我们在`auth.js`中将其导入为

```
const { pool } = require('../db/connect');
```

接下来，在`auth.js`内的`/signup`路径中，我们首先检查到达 API 的数据是否包含使用`common.js`中定义的`isInvalidField`函数注册所需的字段

从客户端发送的数据将在`req.body`对象中可用，我们将它作为 JSON 获取，因为我们在`index.js`中添加了`app.use(express.json())`

所以我们当前和未来的所有路由都将在`req.body`对象中以 JSON 的形式接收数据。

看看下面来自`utils/common.js`的代码

```
const isInvalidField = (receivedFields, validFieldsToUpdate) => {
  return receivedFields.some(
    (field) => validFieldsToUpdate.indexOf(field) === -1
  );
};
```

这将确保在插入时，我们不会试图添加一些导致错误的无效字段。因此，如果有一些无效的字段需要插入，我们会返回一个错误

```
const isInvalidFieldProvided = isInvalidField(
  receivedFields,
  validFieldsToUpdate
);if (isInvalidFieldProvided) {
  return res.status(400).send({
    signup_error: 'Invalid field.'
  });
}
```

然后，我们查询数据库，检查数据库中是否不存在具有相同电子邮件的用户。

```
const result = await pool.query(
  'select count(*) as count from bank_user where email=$1',
  [email]
);
```

这里，我们使用异步 await 语法，而不是 promises 语法。因为我们已经为`/signup` route 函数添加了`async`关键字，所以我们使用`await`关键字进行查询。

结果变量将包含一些额外的数据。我们感兴趣的数据总是来自`result`的`rows`属性。因为只有一个计数，所以我们将其访问为

```
const count = result.rows[0].count;
```

如果计数大于零，这意味着已经有另一个用户使用相同的电子邮件，所以我们发回错误

```
const count = result.rows[0].count;
if (count > 0) {
  return res.status(400).send({
    signup_error: 'User with this email address already exists.'
  });
}
```

注意，我们在这里添加了一个 return 关键字，所以下面几行代码将不会被执行。由于没有 return 关键字，即使响应被发送到客户端，接下来的代码行将被执行，从而导致错误。

现在，如果计数为零，那么我们可以在表中插入用户详细信息。

在将用户详细信息添加到数据库之前，我们首先需要加密密码。我们使用`bcryptjs`库创建安全密码。要了解它是如何工作的，请在这里查看我之前的文章

这里，我们使用下面的代码来生成密码。

```
const hashedPassword = await bcrypt.hash(password, 8);
```

接下来，使用`pg` npm 包，允许我们在数据库中执行查询时编写查询。

```
await pool.query(
  'insert into bank_user(first_name, last_name, email, password) values($1,$2,$3,$4)',
  [first_name, last_name, email, hashedPassword]
);
```

这里，我们通过分配`$count`变量来提供动态值

因此 1 美元将被`first_name`取代，2 美元将被`last_name`取代，以此类推。使用$而不是直接使用值将防止 SQL 注入攻击，所以总是建议使用$进行攻击。

还要注意，因为我们已经在 try 块中添加了整个代码，所以如果在执行查询时发生错误，try 块中的下一个代码将被跳过，catch 块将被执行，并且我们将错误发送回客户端。

```
res.status(400).send({
  signup_error: 'Error while signing up..Try again later.'
});
```

如果一切顺利，那么我们调用`send`响应方法来完成请求，并发回`201`状态代码，表示某个东西被创建了。我们可以发送`200`状态代码，但是`201`在这里更合适。

现在，从`server`文件夹运行`yarn start`命令，启动`express`服务器。

我们现在可以使用邮差测试`/signup` API 了。

![](img/2eb9e01914fe30422f8af06eb1bcbc46.png)

Successful signup

![](img/97415839e6ecefd880a1c2873ed9da83.png)

Email already exists error

![](img/463bac10fed714e2ab6191818efa9cab.png)

Invalid field error

现在，让我们了解一下`/signin`路线。

在这里，我们首先从`common.js`调用`validateUser`功能，检查是否有用户提供了电子邮件地址。

```
const result = await pool.query(
  'select userid,  email, password from bank_user where email = $1',
  [email]
);
```

如果存在这样的用户，则我们将比较用户提供的密码和存储在数据库中的密码，以检查是否匹配。

```
const isMatch = await bcrypt.compare(password, user.password);
if (isMatch) {
  delete user.password;
  return user;
} else {
  throw new Error();
}
```

如果密码匹配，则我们会从响应中删除密码，然后将响应发送回客户端，这样响应将不会包含存储在数据库中的加密密码。

如果用户不存在于数据库中，那么我们使用抛出新的错误()抛出一个错误；

一旦我们抛出`validateUser`函数的错误，来自`/signin`路由的 catch block 将被执行，我们将错误发送回客户端。

```
res.status(400).send({
  signin_error: 'Email/password does not match.'
});
```

如果我们从`validateUser`函数中获取用户，那么我们调用`generateAuthToken`通过传递用户对象来生成 jwt 令牌。

```
const generateAuthToken = async (user) => {
  const { userid, email } = user;
  const secret = process.env.secret;
  const token = await jwt.sign({ userid, email }, secret);
  return token;
};
```

为了生成令牌，我们使用了`jsonwebtoken` npm 库。它提供了一个`sign`方法，该方法接受我们提供的数据来生成令牌作为第一个参数，然后将密钥作为第二个参数，在生成和验证令牌时使用这个参数。

一旦由`generateAuthToken`函数生成令牌，我们就将该令牌和用户 id 一起插入令牌表中。因此，每当用户登录时，我们都会生成一个新令牌并添加到令牌表中。这将确保我们可以从多个设备登录到应用程序，因此令牌仅对该特定设备有效，并且一旦用户从应用程序注销，我们将从令牌表中删除该令牌。

```
const result = await pool.query(
  'insert into tokens(access_token, userid) values($1,$2) returning *',
  [token, user.userid]
);
```

一旦令牌成功添加到`tokens`表中，我们将把用户详细信息和令牌一起发送回客户端

```
user.token = result.rows[0].access_token;
res.send(user);
```

我们现在可以使用 Postman 测试`/signin` API。

![](img/30551d4f47981a1c7d807a5fa78475bf.png)

SignIn Error

![](img/1054eec904938299608f3dc1fadcd128.png)

SignIn successful

> 现在，我们将在 React 应用程序中集成这些 API。

在里面，`src/utils/constants.js`添加另一个常数

```
export const BASE_API_URL = 'http://localhost:5000';
```

在`actions`文件夹下新建一个文件`auth.js`，添加以下内容

这里，在`registerNewUser`函数中，我们调用我们的服务器 API[http://localhost:5000/sign up](http://localhost:5000/signup)，将用户数据作为第二个参数传递给它，并发送回对象`{success: true }`作为 API 成功的结果，发送回`{success: false}`作为失败的结果。

从带有分派参数的函数返回函数的语法由`redux-thunk`库提供。点击这里查看我以前的文章[，了解如何使用它。](https://medium.com/javascript-in-plain-english/how-to-integrate-redux-thunk-into-your-react-application-cd92b87c1629)

打开`components/register.js`并在`this.setState({ isSubmitted: true });`语句后的`registerUser`函数内添加以下代码

```
this.props
 .dispatch(registerNewUser({ first_name, last_name, email, password }))
 .then((response) => {
  if (response.success) {
    this.setState({
      successMsg: 'User registered successfully.',
      errorMsg: ''
    });
  }
});
```

另外，在文件顶部添加一个`registerNewUser`的导入。

```
import { registerNewUser } from '../actions/auth';
```

现在，通过运行`yarn start`命令启动 React 应用程序，并尝试注册一个新用户。输入所有细节后，当你点击注册按钮时，后端 API 将被调用，但现在你会在控制台中看到一个错误，即`CORS(Cross-origin resource sharing)`错误。

![](img/4f4d182169d08dd664eb1d251ad07496.png)

CORS Error

出现错误是因为我们正在从运行在端口 3000 上的应用程序向运行在端口 5000 上的服务器发出 API 调用，因此浏览器阻止了我们对服务器的请求。这是出于安全原因和跨域策略。

为了解决这个问题，我们需要在我们的 express 应用程序中添加额外的中间件，这样它就可以接受来自任何应用程序的请求。

不要担心，在本文的后面，我们将看到如何在我们的应用程序中消除对 cors 的需求。

为此，打开`server/index.js`，为 cors 添加一个导入，并添加它的用途

```
const express = require('express');
const cors = require('cors');
const authRoute = require('./routes/auth');
require('dotenv').config();const app = express();
const PORT = process.env.PORT || 5000;app.use(express.json());
app.use(cors());
app.use(authRoute);app.listen(PORT, () => {
  console.log(`server started on port ${PORT}`);
});
```

现在，重新启动你的服务器，如果它没有运行，从`server`文件夹执行`yarn start`，现在，尝试再次注册用户

![](img/3159a2900ea1c81dd2a70575453e1a48.png)

Registration successful

用户也被添加到数据库中

![](img/f8644e400bad9a966855269b5967b173.png)

Database Users

> 恭喜你，你已经成功整合了注册用户的后端和前端。哇！

现在，我们将把登录 API 集成到 React 应用程序中。

打开`src/components/Login.js`和`handleLogin`函数，在 else 块中添加对`initiateLogin`动作的调用。此外，在顶部添加一个导入

重新启动 React 应用程序(如果尚未运行),并尝试使用已经注册的用户凭据登录。

![](img/2853e8f378733c76c37445c8fe9a2d0b.png)

Login success

如您所见，一旦您单击登录按钮，当登录成功时，数据将被添加到 redux 存储中。

如果您输入错误的凭证，您将在控制台中看到错误

![](img/a99752aa6c4bfdd13218554e836e439b.png)

Login failed

如果您打开网络面板并检查登录请求-响应，您将看到来自后端的实际响应错误

![](img/f6260220ab86973532bdb8fdc245f128.png)

Server error

现在，我们将在 UI 上显示登录和注册页面的后端服务器错误。

打开`src/utils/constants.js`并添加两个新常量

```
export const GET_ERRORS = 'GET_ERRORS';
export const RESET_ERRORS = 'RESET_ERRORS';
```

在`src/reducers`文件夹下新建一个文件`errors.js`，添加以下内容

在`src/actions`文件夹中创建一个新文件`errors.js`,内容如下

现在，打开`actions/auth.js`和`initiateLogin`和`registerNewUser`函数的内部 catch 块，添加以下代码

```
error.response && dispatch(getErrors(error.response.data));
```

这里，从服务器发送的响应将在 error.response.data 对象中可用，我们在 reducer 中添加了该对象

此外，在文件顶部为 getErrors 操作函数添加一个导入

现在，为了获得缩减器中的错误，将缩减器添加到`store/store.js`文件中

```
combineReducers({ 
 auth: authReducer, 
 errors: errorsReducer 
})
```

也为它添加一个导入，在顶部

现在，每当我们调用`getErrors`函数从`initiateLogin`函数调度`GET_ERRORS`动作时，它将调用 reducer，错误将被添加到 redux 存储中

现在，尝试使用无效凭证登录，您将看到 redux 存储中添加的错误。

![](img/9895bcb9a2529a9b78be46a90ad172ca.png)

server error in the redux store

现在，我们将在 UI 上显示来自 redux 存储的错误。

打开`components/Login.js`，在文件底部添加`mapStateToProps`函数，并将其传递给`connect`函数。

```
const mapStateToProps = (state) => ({
  errors: state.errors
});export default connect(mapStateToProps)(Login);
```

添加`componentDidUpdate`和`componentWillUnmount`生命周期方法，并在文件顶部添加`lodash`和`resetErrors`的导入

所以现在，每当 redux 存储中添加了一个错误时，我们将在`props.errors`中获得该错误，因为我们的 mapStateToProps 函数，并且我们通过实现`componentDidUpdate`方法来获取更新后的正确值。

在`componentDidUpdate`方法中，我们首先使用 lodash `isEqual`方法检查先前的属性是否不等于当前的属性，然后才在`errorMsg`状态中设置错误。这个条件是避免无限循环错误所必需的。要理解 lodash 方法的重要性，请点击这里查看我以前的文章

现在，如果您输入无效的登录凭证，您将能够在 UI 上看到后端错误

![](img/5682741acfd85fb0f0e73de83e49f4b0.png)

Server error on UI

现在，让我们在`Register`页面上显示来自后端的错误。

![](img/8c70d7ed6da7c0183492976585f81f60.png)

Registration server error

现在，让我们创建一个`Profile`组件，以便在用户登录到应用程序后显示个人资料页面

在`components`文件夹下新建一个文件`Profile.js`并添加以下内容

在`actions`文件夹下新建一个文件`profile.js`，添加以下内容

打开`utils/constants.js`并添加另一个常量

```
export const UPDATE_PROFILE = 'UPDATE_PROFILE';
```

在`reducers`文件夹下新建一个文件`profile.js`，添加以下内容

添加`profileReducer`保存在`store/store.js`文件中

```
combineReducers({
  auth: authReducer,
  errors: errorsReducer,
  profile: profileReducer
})
```

现在，由于我们已经将`profileReducer`添加到商店中，我们可以使用`mapStateToProps`功能从`store.profile`的任何组件中访问商店中的个人资料信息。

在`router/AppRouter.js`文件中增加`Profile`组件

```
<Route path="/profile" component={Profile} />
```

如果您还记得，每当我们单击`Login`页面上的登录按钮时，我们都会向服务器上的`/signin` API 发送登录请求，一旦成功，我们就会将响应中的数据添加到 redux 存储中

![](img/2853e8f378733c76c37445c8fe9a2d0b.png)

Login success

但是，一旦登录成功，我们就不会重定向到任何组件。

让我们现在就做吧。

在`actions/auth.js`中，在`initiateLogin`内部，需要使用`react-router-dom`的`props.history.push(‘/profile’)`方法成功后重定向到`Profile`组件。

但是`history`对象只适用于`AppRouter.js`中`Route`提到的路线，如`Profile`、`Login`和`Register`组件，所以要访问这些路线之外的历史对象，我们需要使用 npm `history`库。

我们已经初步安装了它以及其他依赖项。所以打开`AppRouter.js`并为历史包添加导入。

```
import { createBrowserHistory } from 'history';
```

调用`createBrowserHistory`函数获取历史对象，我们可以将其导出为名为 export 的对象

```
export const history = createBrowserHistory();
```

我们还需要将这个历史变量传递给我们的`AppRouter`。

变化如此之大

```
<BrowserRouter>...</BrowserRouter>
```

到

```
<Router history={history}>...</Router>
```

并从`react-router-dom`导入`Router`。

```
import { Router } from 'react-router-dom';
```

所以，我们已经导出了历史对象，可以在`actions/auth.js`里面导入它

在`initiateLogin`功能内`dispatch(signIn(user));`后增加`history.push(‘/profile’);`

并在文件顶部为`history`添加导入

现在，登录应用程序，您将看到我们被重定向到配置文件页面。

![](img/65ec9bc9f66a2f1697d2fa26d9c2066b.png)

Login redirect demo

但是您会注意到，银行应用程序标题没有显示在配置文件页面上。因此，让我们创建一个`Header`组件，只为登录用户显示标题。

在`components`文件夹下新建一个文件`Header.js`，添加以下内容

现在，打开`AppRouter.js`，在`Router`内添加`Header`组件

现在，成功登录后，您将看到个人资料页面以及标题

![](img/78006618f1643600793540df7825be87.png)

Profile page

现在，我们将了解如何在刷新后维护会话。

在`src/utils/common.js`文件中添加以下代码

所以，你的完整的`common.js`将看起来像这样

让我们来理解代码。

如果你还记得，在`server/routers/auth.js`的`/signin`路线里面，我们叫`generateAuthToken`

```
const token = await generateAuthToken(user);
```

在这个函数中，我们通过为第一个参数传递 userid 和 email 来调用`jwt.sign`方法。所以生成的 jwt 令牌包含这些值来标识用户。

为了从 jwt 令牌中取出这些值，我们使用了接受令牌并返回这些值的`jwt-decode` npm 包。

因此，在`maintainSession`内部，我们通过传递令牌来使用`json_decode`，并且我们将提取的信息传递给`updateStore`函数，该函数将调度`signIn`动作创建器函数。

```
const decoded = jwt_decode(user_token);
updateStore(decoded);
```

现在，如果您再次登录，您将会看到，即使在刷新了`profile`页面之后，您也会看到，redux store 中的数据的 auth 对象并没有被删除。

![](img/94406d77e18bd3b7e8b59c5a08174f76.png)

redux store data retained even after refresh

这是因为，一旦我们成功登录并刷新页面，在`maintainSession`函数中，我们将检查本地存储中的令牌，并再次调用`signIn` action creator 函数，用从 jwt 令牌解码的值更新 redux 存储。

如果您注意到了，jwt 令牌有 3 个字符串，由两个点分隔。中间的字符串值包含我们在生成令牌时使用的实际值。

导航到[这个站点](https://www.base64decode.org/)，将以下值粘贴到文本区域，然后点击解码

```
eyJ1c2VyaWQiOiIzIiwiZW1haWwiOiJtbEBleGFtcGxlLmNvbSIsImlhdCI6MTU5MDg0NDU5OX0
```

您将看到 jwt 令牌中包含的实际数据。

![](img/847c9774daec4918615cc1028cc7d4f9.png)

Decode jwt token data

一旦用户登录，我们就不应该通过访问这些页面来再次登录或注册。因此，我们需要为当前页面添加一个检查，如果它是`/profile`或`/register`，那么我们重定向到`/profile`页面。

因此，改变

```
export const maintainSession = () => {
  const user_token = localStorage.getItem('user_token');
  if (user_token) {
    const decoded = jwt_decode(user_token);
    updateStore(decoded);
  } else {
    history.push('/');
  }
};
```

到

```
export const maintainSession = () => {
  const user_token = localStorage.getItem('user_token');
  if (user_token) {
    const currentPath = window.location.pathname;
    if (currentPath === '/' || currentPath === '/register') {
      history.push('/profile');
    }
    const decoded = jwt_decode(user_token);
    updateStore(decoded);
  } else {
    history.push('/');
  }
};
```

现在，一旦我们登录，我们将无法访问注册或登录页面。

现在，让我们添加代码，以便在组件加载后获得`profile`细节。

为了做到这一点，在`actions/auth.js`的`initiateLogin`函数中，在`history.push`调用之前，添加以下代码

```
dispatch(initiateGetProfile(user.email));
```

这将在 redux 存储中添加`profile`数据。

现在，让我们在服务器端创建一个 profile API 来获取和更新`profile`信息。

在`server/routes`文件夹下新建一个文件`profile.js`，添加以下内容

现在，在`server/index.js`内为`app.use`添加这条路线

现在让我们来理解代码。

在`middleware/auth.js`中，我们增加了一个中间件功能。

> 什么是中间件？

`Middleware`是一个可以访问请求和响应对象以及额外的 next 参数的函数。因此，每当我们向后端 Express 服务器发出 API 请求时，我们都可以在请求被发送到实际路由之前或之后执行一些代码。所以我们可以做以下事情。

1.我们可以执行日志记录，如请求的日志 URL、IP 地址等

2.此外，基于某些条件，我们可以限制对该路线的访问。

在我们的例子中，我们将使用中间件来限制对路由的访问。

因此，每当我们向 API 路由发出任何请求时，我们将检查身份验证令牌是否由客户端发送并且是否有效，然后将请求转发到私有路由，否则我们将立即发送错误，而不执行私有路由。

如果您记得，在`src/actions/auth.js`中，我们有`initiateLogin`动作创建器，我们在其中添加了这条语句

```
localStorage.setItem('user_token', user.token);
```

因此，如果登录成功，我们会将来自服务器响应的 jwt 令牌存储到本地存储中。因此，对于任何只有在用户登录时才应该访问的 API 路由，我们需要将这个令牌和请求一起发送给服务器。让我们看看当我们在发出获取`profile`信息的 API 请求时不发送令牌会发生什么。

我们已经添加了代码来从`componentDidMount`和`componentDidUpdate`方法内的`components/Profile.js`中的 redux 存储中获取`profile`数据。

但是我们不能进入`login`或`register`页面，因为我们已经登录了，所以现在，要测试 get `profile` API，只需在浏览器控制台中键入以下代码，然后点击 enter

```
localStorage.removeItem('user_token')
```

![](img/b8da6d8a6f841b6472afbb811104fd3a.png)

Deleting localStorage token value

所以现在你去 [http://localhost:3000/](http://localhost:3000/) 的时候会自动重定向到登录页面

现在再次登录，您可能会失望，因为您将在控制台中看到一个错误。

![](img/c6015ab4598be55b53770b9d24b93652.png)

如果您看到网络面板，您将看到`Authentication failed`服务器错误

![](img/dcfa562803bcf24926f01fda04d97a72.png)

Authentication failed server error

这是因为我们在`routes/profile.js`文件中添加了`authMiddleware`来获取`/profile`路线，并且这是一个私有路线，只有登录的用户才能访问该路线。

```
Router.get('/profile', authMiddleware, async (req, res) => {
```

当我们进入`/profile`路线时，将执行`authMiddleware`。

如果你看到`authMiddleware`函数的代码，你会发现它需要一个我们没有提供的`Authentication Header` ，因此我们得到了`Authentication failed`错误。

在`Authorization`请求头中，我们需要以下面的方式将 jwt 令牌作为`Bearer`令牌发送

```
Header name: header valueFor ex. Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyaWQiOiIzIiwiZW1haWwiOiJtbEBleGFtcGxlLmNvbSIsImlhdCI6MTU5MDg1NzMwNH0.uTwti-O79u_8sT-H70WsR4LyQDN3mttR6xYxn2a6VeQ
```

让我们先通过邮递员来做这件事，这样你会有一个想法。

让我们通过 postman 点击登录 API。

![](img/8db843d25d7ea1d0091b8af03d5304c3.png)

Login API postman

现在，我们将使用从登录响应中获得的令牌，并将相同的令牌用于`profile`路由

在 headers 选项卡中，我们将添加带有令牌值的`Authorization`标头

![](img/61c9bc558e3ba0575c087c938cb4f448.png)

Adding Authorization header

一旦您单击 send 按钮发送 API 请求，我们就会得到`profile`信息，而不会出现认证错误

![](img/f571c744cae6f6789d7032da681b696f.png)

profile API response

现在，让我们看看如何在代码中使用`Axios`发送授权头。

在`src/utils/common.js`文件中添加以下两个函数

正如您所看到的，我们以如下方式添加了一个标题

```
axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
```

所以在进行 API 调用之前，我们将执行这个函数，这个头将被自动添加到默认的请求头中。

打开`src/actions/profile.js`，在`initiateGetProfile`函数中，在`axios.get`调用之前添加一个对`setAuthHeader`函数的调用，并从`common.js`为其添加一个导入。

现在，让我们看看`profile`信息是否显示在页面加载上。在浏览器控制台中键入`localStorage.removeItem(‘user_token’)`并转到 [http://localhost:3000/](http://localhost:3000/) 并再次登录

这一次，您将看到自动填充的个人资料信息

![](img/5b3e8817fa45f8c60946345a0e7e3cdf.png)

Profile information populated

您还可以更新`profile`信息。

厉害！如果您检查 network 选项卡，您可以在 request headers 部分下看到我们的`Authorization`标题。

![](img/5ebfbe473909e1b788f713fb232757eb.png)

Profile API Headers

现在，你知道了如何发送`authorization`头，我们可以看看`middleware/auth.js`中`authMiddleware`的代码

从`req.header`我们得到标题为

```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyaWQiOiIzIiwiZW1haWwiOiJtbEBleGFtcGxlLmNvbSIsImlhdCI6MTU5MDg1NzMwNH0.uTwti-O79u_8sT-H70WsR4LyQDN3mttR6xYxn2a6VeQ
```

所以我们把它按空间分开，只取代币值

```
const token = req.header('Authorization').split(' ')[1];
```

然后使用来自`.env`文件的相同密钥，我们使用它来创建 jwt 令牌，我们使用`jwt.verify`方法来验证令牌是有效的，并取出存储在该令牌中的用户 id 和电子邮件，并将其存储为解码变量。

然后，我们检查令牌表中是否存在令牌，如果找到，我们将返回用户的详细信息以及添加到请求对象中的令牌，并调用下一个函数，该函数将执行预定路由的代码。

正如我们在得到`/profile`路线时已经`authMiddleware`

```
Router.get('/profile', authMiddleware, async (req, res) => {
```

当我们调用下一个函数`next()`时，来自`/profile`路径的代码将被执行。

如果在验证令牌或解码令牌时出现任何错误，来自`authMiddleware`的 catch 块将被执行，我们将把`Authentication failed`错误发送回客户端

```
res.status(400).send({
  auth_error: 'Authentication failed.'
});
```

> 现在，让我们添加注销功能

在`server/routes/auth.js`中添加以下代码

在`components`文件夹中新建一个文件`Logout.js`，内容如下

打开`actions/auth.js`并添加以下代码

完整的`auth.js`代码如下所示

如果您检查上面的`initiateLogout`函数，您将会看到，在进行 Axios 调用之前，我们正在调用`setAuthHeader`函数，因为这也是一个私有路由，因为只有登录的用户才允许注销。调用完成后，我们将移除`Authorization`头，从本地存储中删除令牌，并调用`signOut`动作创建器函数，这样它将从 redux 存储中清空 auth 对象。

注意，我们在派遣`signOut`之前添加了一个返回关键字。

```
return dispatch(signOut());
```

因此，由于这是在一个异步函数中，我们将在调用这个`initiateLogout`函数后得到一个承诺，并且在`Logout.js`中我们已经添加了`.then`调用，所以一旦我们被注销，我们将被重定向到登录页面

```
dispatch(initiateLogout()).then(() => history.push('/'));
```

现在，将`/logout`路线添加到`AppRouter`组件中

```
<Route path="/logout" component={Logout} />
```

现在，我们可以测试注销功能。

单击注销按钮，您将被重定向到登录页面。

![](img/7caa4e1fadc8c5fbe79122f205a24c1e.png)

Working application functionality

**几个改进:**

现在我们已经完成了我们的应用程序的一个流程，但是我们还可以做一个改进。如果您还记得，对于每一个像 get 或 post `/profile`这样的私有路由，在进行 API 调用之前，我们都在调用`setAuthHeader()`函数。我们有许多其他的途径，我们将在下一部分中探索，但是每次在进行 API 调用之前调用该函数是很麻烦的，或者可能会发生这样的情况，我们可能会忘记调用该函数，并且我们会收到`Authentication failed` 错误。因此，我们可以分离出完成调用该功能的功能，然后进行 API 调用，这是处理 API 的正确方式。让我们开始吧

在`src/utils`文件夹中创建一个新文件`api.js`并添加以下代码

在此，我们向每条`get`、`post`和`patch`路线发送以下参数。
1。API URL 作为第一个参数
2。作为第二个参数发送给 API 的数据
3。如果我们需要在进行 API 调用之前设置`Authorization`头，发送`true`作为第三个参数
4。如果我们需要在进行 API 调用后删除 auth 头，那么发送`true`作为第四个参数

在`src/actions/profile.js`内部，在`initiateGetProfile`函数内部，移除`axios.get`，从`api.js`调用 get 函数

```
const profile = await get(`${BASE_API_URL}/profile`);
```

在`initiateUpdateProfile`函数中，删除`axios.post`并添加以下代码行

```
const profile = await post(`${BASE_API_URL}/profile`, profileData);
```

同样在`src/actions/auth.js`内，在`initiateLogout`函数内，删除`axios.post`并添加以下代码行

```
await post(`${BASE_API_URL}/logout`, true, true);
```

好了...谢谢你忍受了我这么久。

您可以找到 Github 源代码，直到现在

*   使用类组件[这里](https://github.com/myogeshchavan97/fullstack_banking_app/tree/section-1-using-class-components)
*   在这里使用挂钩[和](https://github.com/myogeshchavan97/fullstack_banking_app/tree/section-1-using-hooks)

今天到此为止。我希望你学到了新东西。

**在这里** **查看这个多部分系列的下一部分** [**。**](https://medium.com/javascript-in-plain-english/create-a-full-stack-banking-application-using-react-part-2-6fb21200613a?source=friends_link&sk=0394e9bb66379594d2ebc7bffdfbfea5)

**别忘了直接在你的收件箱** [**这里订阅我的每周简讯，里面有惊人的技巧、窍门和文章。**](https://yogeshchavan.dev/)

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**