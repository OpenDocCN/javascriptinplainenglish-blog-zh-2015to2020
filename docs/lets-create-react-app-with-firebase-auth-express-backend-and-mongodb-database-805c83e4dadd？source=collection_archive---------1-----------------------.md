# 让我们用 Firebase Auth、Express Backend 和 MongoDB 数据库创建一个 React 应用程序

> 原文：<https://javascript.plainenglish.io/lets-create-react-app-with-firebase-auth-express-backend-and-mongodb-database-805c83e4dadd?source=collection_archive---------1----------------------->

![](img/64cf9c0de63940049592f82b18223ae4.png)

Firebase 是一个非常好的软件开发平台，它可以让你访问身份验证、noSQL 数据库、存储等等。这是一个真正的发电站，我喜欢使用 Firebase 和 database Firestore 的便利性。

我们都知道在工作中使用正确工具的重要性。如果你用 Firebase 开发一个应用程序，你可能会遇到云功能不够用的情况，你需要一个单独的后端服务器来完成一些任务。您甚至可以使用 Firebase 作为您的主枢纽，并让其他微服务连接到它。

不管怎样，如果您要离开 Firebase，您需要进行身份验证。在 Firebase 和你单独的后端服务器上有不同的认证系统会很奇怪。

幸运的是，Google 允许我们在 Firebase 之外使用 Firebase 身份验证。这是通过将 Firebase 身份验证令牌和有效负载一起发送到后端服务器，并在那里进行验证来实现的。让我们看看如何解决这个问题。

这将是一个循序渐进的教程，我们建立一个简单的例子应用程序。我们将详细介绍每个步骤，您不需要任何 Firebase、React 或 Express 方面的经验。

# 目录

*   [我们正在建造的东西](#9da3)
*   [搭建消防基地](#f790)
*   [始反应前端](#28e4)
*   [开始快递后端](#1209)
*   [与后端通信](#fa67)
*   [从后端到前端](#a97b)
*   [在前端显示数据](#c101)
*   [收尾思路](#f8b9)

# 我们正在建造的东西

我们将建立一个简单的电话簿。用户将能够添加电话号码，并看到所有电话号码的列表。我们不会在这个项目中使用任何 css 样式。让我们看看我们将使用的技术。

**Firebase** 我们将只使用 Firebase 作为其认证平台。
[https://firebase.google.com/](https://firebase.google.com/)

我们将使用现代的钩子和基于箭头的 React 作为我们的前端。
[https://reactjs.org/](https://reactjs.org/)

**Express** 我们将在 NodeJS 中编写我们的后端，使用 Express 作为我们的后端框架
[https://expressjs.com/](https://expressjs.com/)

**MongoDB(MongoDB Atlas)** 我们将使用 MongoDB Atlas 作为我们的数据库——一个基于云的 MongoDB 版本，由 MongoDB 团队自己托管。对于这个应用程序的功能，常规的 SQL 数据库是最自然的选择。然而，在 npm 模块 Mongoose 的帮助下，MongoDB 将充当关系数据库，并为我们的数据提供模式。沿着这条路走下去的好处是，我们可以对结构化数据使用 Mongoose，对非结构化数据使用纯 MongoDB。在这个应用程序中，我们将只通过 Mongoose 使用 MongoDB。https://www.mongodb.com/

## Github 的完整代码

这个项目的完整源代码可以在 https://github . com/Devalo/Firebase-auth-react-express-MongoDB 找到

对于这样一个简单的应用程序来说，这是一个很大的变动。我们会玩得很开心的！

# 设置 Firebase

让我们从创建一个新的 Firebase 应用程序开始。移至[www.firebase.com](http://www.firebase.com)点击“开始”，用你的谷歌账户登录。

接下来，我们必须单击添加项目

![](img/353439f09a4f12ce78e053885ebc6870.png)

我们将为该应用程序命名

![](img/22446ad5aeb078fb10beae55360ec411.png)

我们不需要这个项目的谷歌分析

![](img/e1989d76522ccd25b7838767d285800d.png)

然后我们按“创建项目”。过一会儿，我们的项目就准备好了。准备好后，点击“继续”。

如果你看一下左边的菜单，你会发现认证标签。我们将选中它，然后单击“设置登录方法”。

![](img/42ac74771d79dce4135c6454385782ff.png)

将显示不同登录提供商的列表。我们只需要电子邮件/密码。我们悬停在它上面，点击铅笔并打开它。

![](img/6bd2825ca710a827f52277145156cdfc.png)

现在，我们不会在这个项目中创建新用户。我们将只允许用户登录。因此，我们将把新用户直接添加到 Firebase 身份验证中。点击“用户”

![](img/aabf0b817c03edc7e78db800ca9159bd.png)

并添加新用户。

![](img/4bef9049d85e659a01e51674a97ce441.png)

Firebase 会自动正确散列密码。您将无法从该面板对用户进行任何更改。所有其他更改必须以编程方式完成。然而，我们的第一个用户被创建:

![](img/a5881af74898024014f85bdfb2bcf08f.png)

# 开始反应前端

## 设置 React

有了 Firebase，我们将开始设置我们的前端。我们将使用 create-react-app 来引导我们的前端

```
$ npx create-react-app phone-frontend
...
Happy hacking!
$ cd phone-frontend
/phone-frontend $ npm start
```

如果一切按计划进行，我们应该在 [http://localhost:3000](http://localhost:3000) 看到这个

![](img/d0f1f92315f6e0f4d2550c9929d2fb49.png)

太好了。现在，我们的大部分工作将在 src 文件夹中完成。我们在 src 文件夹中的文件夹结构如下:

```
/components
  - /sessions
      Login.jsx
  - /phonebook
      AddNumber.jsx
      ListAllNumbers.jsx
/services
  - phonebookServices.js
App.js
index.js
```

我们将 index.js 更改为:

```
// index.js import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

和我们的 App.js:

```
// App.jsfunction App() {
  return (
    <div className="App">
      Hello World!
    </div>
  );
}export default App;
```

我们已经准备好开始我们的 React 前端工作。注意—此时您可能需要重新启动 react 服务器，以使更改生效。

现在，我们知道我们将有多个视图。我们需要一种方法来改变登录页面，列出所有电话号码，并添加号码。为此，我们将使用一个名为 react-router-dom 的 npm 模块。让我们安装它:

```
npm install react-router-dom
```

我们还需要安装 firebase 包:

```
npm install firebase
```

现在，让我们创建我们的登录路径，并使它在我们没有登录时总是登陆到我们的登录页面。

## 将 firebase 添加到我们的前端

在之前的步骤中，我们创建了一个新的 Firebase 应用程序。我们需要从 firebase 网站上获取一些配置。

回到 firebase 网站，单击左侧导航栏中最上面的链接——项目概述。我们需要添加一个新的 web 应用程序:

![](img/e8845860b870c6216f6df448f70cfa95.png)

我们给它起了个名字:

![](img/3fe2ac6db82cb08e146c2967220fed82.png)

我们复制 firebaseConfig 对象，然后单击“继续到控制台”

![](img/18d06df2f1045d64e145a779ee8c5766.png)

在一个生产应用中，你可以把这些配置放在环境变量中，但是 Google 声明这不是绝对必要的。这是因为 Firebase 安全性基于多个规则，这些规则限制和开放了应用程序。依我拙见，隐藏这些类型的设置总是更好。不过，我可能错了。

在我们的 src 文件夹中，我们将创建一个新文件——fire . js，并将我们的配置添加到其中。该文件应该如下所示:

```
// fire.js import firebase from 'firebase';const firebaseConfig = {
    apiKey: "AIzaSyAHDUwcM0zO4yMCoyazo4w-HOeKfGsQL2g",
    authDomain: "phone-book-fe436.firebaseapp.com",
    databaseURL: "[https://phone-book-fe436.firebaseio.com](https://phone-book-fe436.firebaseio.com)",
    projectId: "phone-book-fe436",
    storageBucket: "phone-book-fe436.appspot.com",
    messagingSenderId: "410083027161",
    appId: "1:410083027161:web:e69ec743153a72d48df6c8"
};try {
  firebase.initializeApp(firebaseConfig);
} catch (err) {
  if (!/already exists/.test(err.message)) {
    console.error('Firebase initialization error', err.stack);
  }
}const fire = firebase;
export default fire;
```

我们将在这个文件中使用 firebase。我们现在准备开始认证用户。

## **设置认证**

Firebase 为我们提供了一种非常简单的方法来检查我们是否通过了身份验证。我们将使用 useState()钩子创建一个新的状态，并将结果存储在那里:

```
// App.js import React, { useState } from 'react';import fire from './fire.js';function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

    fire.auth().onAuthStateChanged((user) => {
      return user ? setIsLoggedIn(true) : setIsLoggedIn(false);
  });

  console.log('logged in?', isLoggedIn);
  return (
    <div className="App">
      Hello World!
    </div>
  );
}export default App;
```

我们从 React 库中导入 useState，从 firebase 配置文件中导入 firebase。我们使用。onAuthStateChanged()函数来检查用户是否登录。如果我们已经登录，我们将把 isLoggedIn 状态设置为 true。

如果我们检查我们的控制台，它清楚地表明我们没有登录。

我们将创建第一条路线。现在，对于每个请求，我们都希望在继续之前检查我们是否已登录。

```
// App.jsimport { BrowserRouter as Router, Switch, Route } from 'react-router-dom';...<div className="App">
      <Router>

        {!isLoggedIn
          ? (
            <>
              <Switch>
                <Route path="/">
                  <Login />
                </Route>
              </Switch>
            </>
          ) 
          : (
            <>
              Hello World!
            </>
          )}
      </Router>
    </div>
...
```

所有路由都是交换机组件的子组件，而交换机组件是路由器组件的子组件。如果我们没有登录，除了登录组件，我们将看不到任何东西。我们可以通过查看空白页来验证这一点。

让我们转到登录组件，看看我们能否在页面上添加一些内容。

我们将首先创建一个简单、可行的表单，记录电子邮件和密码输入:

```
// components/session/Login.jsx import React, { useState } from 'react';
import fire from '../../fire.js';const Login = () => {
    const [email, setEmail] = useState();
    const [password, setPassword] = useState();

    const handleSubmit = (e) => {
        e.preventDefault();
        console.log(`submitted email: 
          ${email} password: ${password}`);
    }
    return (
        <div>    
        <h2>Login</h2>
            <form onSubmit={handleSubmit}>
                <input
                    type="text"
                    onChange={({ target }) =>     
                      setEmail(target.value)}
                    placeholder="Email"
                />
                <br />
                <input
                    type="password"
                    onChange={({ target}) => 
                      setPassword(target.value)}
                    placeholder="Password"
                />
                <br />
                <button type="submit">
                    Sign in
                </button>
            </form>
        </div>
    )
};export default Login
```

我们设置了两个新的状态挂钩，它们将保存我们的电子邮件和密码状态。每个输入都有一个 onChange 属性，它直接记录每个状态钩子的输入。如果我们按下“登录”按钮，我们输入的数据将显示在我们的控制台上。相当整洁。登录将变得非常容易。我们只需向提交函数添加一行代码:

```
// components/session/Login.jsx...const handleSubmit = (e) => {
  e.preventDefault();
  fire.auth().signInWithEmailAndPassword(email, password)
    .catch((error) => {
      console.error('Incorrect username or password');
    });
  }...
```

这条简单的线。如果您输入了错误的电子邮件和密码:

![](img/de6acf042133637d5fe441d91243b6c6.png)

如果我们使用正确的用户名和密码登录，我们将恢复我们的 Hello World！文本，我们的控制台现在打印 true(来自保存会话状态的状态钩子)。

花点时间，让它深入人心。这是难以置信的简单认证。我不认为你能比这更容易得到它。

我们可能也应该想出一个注销的方法。幸运的是，Firebase 也让这变得非常简单。替换文本“Hello World！”在 App.js 中使用:

```
// App.js <span onClick={signOut}>
  <a href="#">Sign out</a>
</span>
```

并向 App 组件添加一个新功能:

```
const signOut = () => {
    fire.auth().signOut()
};
```

就是这样！单击“注销”后，我们会被重定向回登录表单。我们有了一个好的开始！

## 为我们的电话号码创建视图

我们之前为我们的电话号码创建了一个组件文件。现在是开始实现我们的数据显示方式的好时机。让我们从创建一条新路线开始。我们会将我们的路线放在“登出”链接的下方:

```
// App.jsimport ListAllNumbers from './components/phonebook/ListAllNumbers';...<span onClick={signOut}>
  <a href="#">Sign out</a>
</span>
<Switch>
  <Route path="/">
    <ListAllNumbers />
  </Route>
</Switch>...
```

你一保存文件，应用程序就会崩溃。没关系。我们还没有创建 ListAllNumbers 组件。让我们来看看组件文件。

现在，我们将创建一个非常简单的表，我们将在其中显示我们的数据:

```
// components/phonebook/ListAllNumbers.jsximport React from 'react';
import { Link } from 'react-router-dom';const ListAllNumbers = () => {
  return (
    <div>
      <Link to="/add-number">Add number</Link>
      <h2>Phone numbers</h2>

      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Number</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Foo Bar</td>
            <td>999888777</td>
          </tr>
        </tbody>
      </table>
    </div>
  ) 
};export default ListAllNumbers;
```

![](img/be7b82df5c2dba2a0547d81f29468c45.png)

这是一个杰作..

## 创建添加电话号码的视图

在开始查看任何逻辑之前，我们将创建最后一个视图。我们将在原有路线的基础上建立一条新路线。

整个 App.js 将看起来像这样:

```
// App.js import React, { useState } from 'react';
import { BrowserRouter as Router, Switch, Route} from 'react-router-dom';import fire from './fire.js';
import Login from './components/session/Login';
import ListAllNumbers from './components/phonebook/ListAllNumbers';
import AddNumber from './components/phonebook/AddNumber';
function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

    fire.auth().onAuthStateChanged((user) => {
      return user ? setIsLoggedIn(true) : setIsLoggedIn(false);
  });

  const signOut = () => {
    fire.auth().signOut()
  };

  console.log(isLoggedIn);
  return (
    <div className="App">
      <Router>
        {!isLoggedIn
          ? (
            <>
            <Switch>
              <Route path="/">
                <Login />
              </Route>
            </Switch>
            </>
          ) 
          : (
            <>
            <span onClick={signOut}>
              <a href="#">Sign out</a>
            </span>
            <Switch>
              <Route path="/add-number">
                <AddNumber />
              </Route>
              <Route path="/">
                <ListAllNumbers />
              </Route>
            </Switch>
            </>

          )}
      </Router>
    </div>
  );
}export default App;
```

完美。我们的路线已经配置好了。让我们跳转到 AddNumber 组件并添加必要的代码:

```
// components/phonebook/AddNumber.jsimport React, { useState } from 'react';
import { Link } from 'react-router-dom';const AddNumber = () => {
  const [name, setName] = useState();
  const [phone, setPhone] = useState();

  const handleSubmit = (e) => {
    e.preventDefault();

    console.log(`submitted: ${name} - ${phone}`);
  };

  return (
    <div>
      <Link to="/">View phonebook</Link>
    <h2>Add Number</h2>
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Name"
        onChange={({ target }) => setName(target.value)}
      /><br />
      <input
        type="text"
        placeholder="Number"
        onChange={({ target }) => setPhone(target.value)}
      /><br />

      <button type="submit">
        Add number
      </button>
    </form>
    </div>
  )
};export default AddNumber;
```

正如我们对登录字段所做的那样，我们将两个输入字段都存储在一个状态钩子中。在提交时，我们将输入数据打印到控制台。完美。这和我们的前端一样棒。我们现在已经完成了前端。我们应该创建后端，然后连接数据库。我们将很快返回前端。

# 开始快速后端

我们将在 framework Express 的帮助下构建后端。我们的后端将是一个独立的应用程序。让我们从前端光盘，并为我们的后端创建一个新的文件夹:

```
/phone-frontend $ cd ..
$ mkdir phone-backend
$ cd phone-backend
/phone-backend $ npm init
```

我们用“npm init”开始一个新的 JS 项目。这里的问题现在对我们来说都不重要，所以只需点击回车直到结束。完成后，我们将在手机后端文件夹中创建一个 package.json 文件。

由于我们的后端将是一个 JSON-API，我们不打算使用任何快速生成器来引导我们的应用程序。我们将从安装 Express 开始:

```
/phone-backend $ npm install express
```

并创建我们的端点索引. js 文件:

```
/phone-backend $ touch index.js
```

在我们的索引文件中，我们将编写一个快速服务器:

```
// index.js const express = require('express');const app = express();app.get('/', (req, res) => {
    res.send('Hello World');
});const PORT = 3001;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```

这就是创建服务器端点的全部工作。我们可以用 app.get()定义一条路由。如果你前往 [http://localhost:3001](http://localhost:3001) 网站，你会收到那条可爱的信息:

![](img/f6e8a933145aae9244aa9e0cfe779345.png)

让我们考虑一下应用程序的流程。

*   前端将向我们的根路由发送一个 GET 请求，查询所有条目
*   后端将接收请求，并从数据库中提取
*   前端将向我们的根路由发送一个 POST 请求，添加一个条目
*   后端将接收请求，并添加到数据库

一个好的起点是路线框架。我们将这样构建我们的应用程序:

```
- controllers
  phones.js
- models
  phone.js
index.js
```

## 写出我们的控制器，安装 nodemon

我们将从包含路线的控制器开始。只有两个，所以编码相当容易。我们将使用快速路由器，它是一种功能强大的内置路由器。

我们的控制器文件将如下所示:

```
// controllers/phones.js const phonesRouter = require('express').Router();phonesRouter.get('/', (req, res) => {
  return res.send('Hi, from within the phones router GET'); 
});phonesRouter.post('/', (req, res) => {
  return res.send('Hi, from within the phones router POST');
});module.exports = phonesRouter;
```

两条路线。一个用于 GET 请求，一个用于 POST 请求。它们在被调用时会返回一条消息。现在，我们将它导入到 index.js 中，并用 phoneRouter 替换我们之前的路由。请注意，我们在 url 中添加了“/api”。我们查询 API 的 url 将是 http://localhost:3001/api

```
const express = require('express');const phonesRouter = require('./controllers/phones');
const app = express();app.use('/api', phonesRouter);const PORT = 8080;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```

如果你尝试移动到我们的新网址，什么也不会发生。事实证明，当我们修改代码时，我们的应用程序不会自动重新加载。为了解决这个问题，我们将安装一个名为 nodemon 的 npm 包，它将做到这一点—代码更改时重新加载:

```
/phone-backend $ npm install nodemon
```

为了使用 nodemon，我们将在 package.json 文件中创建一个小脚本:

```
// package.json..."scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon index.js"
  },
...
```

“开发”脚本将使用 nodemon 启动我们的应用程序。我们用 npm run dev 命令启动我们的应用程序:

```
/phone-backend $ npm run dev
> nodemon index.js[nodemon] 2.0.6
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node index.js`
Server is running on port 3001
```

太棒了，如果你返回 [http://localhost:3001/api，](http://localhost:3001/api,)你会收到一条直接来自路由的消息。我们即将从数据库中获取数据。关于这一点——让我们建立一个新的 MongoDB Atlas 数据库

## 设置 MongoDB 地图集

我们需要做的第一件事，是前往 MongoDB Atlas 并创建我们的帐户:前往 https://www.mongodb.com/cloud/atlas/signup[并创建一个新用户。](https://www.mongodb.com/cloud/atlas/signup)

选择空闲群集:

![](img/636e737081a9a331accb358808d36e95.png)

您将能够选择哪个提供商来托管您的数据库，以及该地区。我打算用谷歌托管的数据库。单击“创建集群”:

![](img/f5d20e69d1a9a1b4fb1040f60d555edc.png)

将出现一个向导。一旦集群启动并运行，让我们按照它的步骤进行。这可能需要几分钟时间:

![](img/bb1b2a3c7c3f307eda751465c2078829.png)

我们将从创建第一个数据库用户开始。前往数据库访问:

![](img/14bf3cf768fa513a78e9f53b76ab2e97.png)

创建新用户:

![](img/f19346f0b5075da07dd7c057f76987ff.png)

并添加用户。一旦完成，我们需要将我们的服务器列入白名单。我们将在这里使用标准 IP 0.0.0.0:

![](img/f0b6a91eb7082815b4bcc2413d682a85.png)

将 IP 地址列入白名单提供了额外的安全层。然而，我们的后端将登录到用户名和密码的数据库，所以它不是完全开放的。

一旦完成，下一步将是连接我们的数据库到我们的后端。

我们将选择连接我们的应用程序:

![](img/f6a9d92a39308e5f690fd78ae4eca230.png)

复制网址。这是我们将用来连接数据库的 url。保存 url 以备后用:

![](img/88dfda45806a8479d2369d633e6d9b00.png)

MongoDB Atlas 已配置。让我们回到我们的后端，把它连接起来。

## 将后端连接到 MongoDB Atlas

如前所述，我们将使用 Mongoose 而不是纯 MongoDB。这将为我们提供一些结构。(这有点违背了使用 noSQL 数据库的目的，但是有了 Mongoose，您可以两者兼得)。让我们安装它:

```
/phones-backend $ npm install mongoose
```

为了简单起见，我们将在 index.js 中添加连接到 MongoDB 的代码。当然，您可以将它存储在其他地方，并将其导入 index.js:

```
// index.jsconst mongoose = require('mongoose');...mongoose.connect(
 'mongodb+srv://foobar:[foobar123@cluster0.ylunw.mongodb.net](mailto:foobar123@cluster0.ylunw.mongodb.net)/phonebook?retryWrites=true&w=majority',
 { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    console.log('Connected to database');
  })
  .catch((err) => {
    console.log('Error connecting to DB', err.message);
  });...
```

将您之前复制的 MongoDB URL 粘贴到 mongoose 的 connect 函数中。用您自己的凭证替换<password>和<dbname>。当您保存时，服务器将重新启动，您会在控制台中收到可爱的消息，说明您已连接到数据库。如果由于某种原因出现错误，请确保您输入了正确的凭据。</dbname></password>

我们的后端服务器和数据库之间的连接现在已经完成。这意味着我们可以开始从数据库中取出东西，并把东西放入其中。在此之前，让我们回到前端。

# 与后端通信

如果我们回顾一下我们之前编写的应用程序的流程

*   前端将向我们的根路由发送一个 GET 请求，查询所有条目
*   后端将接收请求，并从数据库中提取
*   前端将向我们的根路由发送一个 POST 请求，添加一个条目
*   后端将接收请求，并添加到数据库

我们可以看到前端需要向后端发送一个 get 请求。我们希望在请求中包含一个 firebase auth 令牌。这样，后端可以在允许访问数据库之前验证用户。

我们希望将与后端通信的代码与视图组件本身分开。在我们的 AddNumber.jsx 文件中，我们将把表单数据发送给一个服务，该服务将处理后端通信。如果您还没有在服务文件夹中创建 phonebookServices.js 文件，这将是一个很好的时机。

## 将数据传递给服务

首先，让我们让前端和后端互相通信。在开始发送 firebase 令牌之前，我们将重点关注这一点。在 AddNumber.jsx 中，我们将向 handleSubmit()函数添加一些代码:

```
// components/phonebook/AddNumber.jsx import { addToPhonebook } from '../../services/phonebookServices';...const handleSubmit = (e) => {
    e.preventDefault(); if (name && phone) {
      phonebookServices.addToPhonebook(name, phone);
    }
  console.log('You must enter a name and a number');
  };...
```

如果两个字段都填写了，我们会将数据发送给我们的服务。如果没有，我们打印到控制台。如果你真的填了东西并提交了，这个应用就会爆炸。
接下来让我们来修复服务吧！

## 将数据从服务传递到数据库，然后再传递回来。

该服务将负责与后端服务来回通信。我们可以使用内置的 fetch 库来做到这一点。我们将使用 axios 库，因为我发现它更容易使用。让我们安装它:

```
/phone-frontend $ npm install axios
```

我们知道我们的后端服务器运行在 [http://localhost:3001/api，](http://localhost:3001/api,)，所以是时候将表单数据发送到那个 url 了。在我们的服务文件中:

```
// services/phonebookServices.js import axios from 'axios';const url = '[http://localhost:3001/api'](http://localhost:3001/api');export const addToPhonebook = (name, number) => {
  const payload = {
    name,
    number,
  }
  axios.post(url, payload);
};
```

我们创建一个新的 JS 对象，并用后端 url 将其传递给 axios。前所未有的兴奋，我们填写并发送我们的表格:

![](img/f128d085eea5d56c8720a8b544838888.png)

哇哦。这不起作用。嗯，这是因为跨产地资源共享，简称 CORS。

> 来自 mozilla 开发者
> 
> **跨源资源共享** ( [CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS) )是一种机制，它使用额外的 [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP) 头来告诉浏览器，让运行在一个[源](https://developer.mozilla.org/en-US/docs/Glossary/origin)的 web 应用程序访问来自不同源的选定资源。当 web 应用程序请求来源(域、协议或端口)与其自身不同的资源时，它会执行跨来源 HTTP 请求。

我们将通过安装一个名为 cors 的模块来解决这个问题。转到后端，安装它:

```
/phone-backend $ npm install cors
```

并将其应用到我们的后端:

```
// index.jsconst cors = require('cors');...const app = express();app.use(cors());...
```

安装了 cors 后，我们再次尝试我们的前端，一切都如预期的那样工作:

![](img/296bcd849a0383847b1146b62257adfe.png)

这太完美了。前端和后端现在正在通信。我们需要做的下一件事，是弄清楚如何将 Firebase 令牌和有效负载一起从前端发送到后端。

## 前端 Firebase 令牌

Firebase 的伟大之处在于它非常易于使用。创建令牌也是如此。我们将在服务文件中创建一个新函数，它将为我们创建令牌。

```
// services/phonebookService.jsimport fire from '../fire';...const createToken = async () => {
  const user = fire.auth().currentUser;
  const token = user && (await user.getIdToken()); const payloadHeader = {
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${token}`,
    },
  };
  return payloadHeader;
}...
```

这个函数从 Firebase Auth 库中获取当前用户。我们使用当前用户来获取 id 令牌。我们需要将这个令牌放在我们的请求头中。最简单的方法是创建并返回包含令牌的头。

在 addToPhonebook 函数中，我们将使用一个异步 try…catch 块来增强代码，并将我们新生成的 auth 头包含在有效负载中:

```
// services/phonebookServices.js...export const addToPhonebook = async (name, number) => {
  const header = await createToken(); const payload = {
    name,
    number,
  }
  try {
    const res = await axios.post(url, payload, header);
    return res.data;} catch (e) {
    console.error(e);
  }
};
```

现在，让我们创建一个从数据库中获取所有电话号码的函数。我们在同一个文件中创建它:

```
// services/phonebookServices.js... export const getPhonebookEntries = async () => {
  const header = await createToken();try {
    const res = await axios.get(url, header);
    return res.data;
  } catch (e) {
    console.error(e);
  }
}
```

我们再次使用 axios。这一次，我们将向后端发送一个 GET 请求。这将查询所有电话簿条目的后端。让我们回到后端。

# 从后端到前端

回顾我们的申请流程

*   前端将向我们的根路由发送一个 GET 请求，查询所有条目
*   后端将接收请求，并从数据库中提取
*   前端将向我们的根路由发送一个 POST 请求，添加一个条目
*   后端将接收请求，并添加到数据库

我们可以跨越两步，这样就把步骤缩小到:

*   后端将接收请求，并从数据库中提取
*   后端将接收请求，并添加到数据库

在我们实际获取和发送到数据库之前，我们应该验证用户。认证令牌包含在请求中。这意味着我们可以创建一个中间件，它将通过管道进入请求并对其稍作修改。我们将修改它，以便一旦请求到达我们的控制器和路由，我们能够在请求中找到经过验证的用户。

## 创建 firebase 中间件

因为我们使用 Firebase 作为我们的身份验证提供者，所以我们需要安装 Firebase 后端模块。这将允许我们用 Firebase 验证身份验证令牌。让我们安装它:

```
/phonebook-backend $ npm install firebase-admin
```

我们需要生成一个谷歌服务帐户私钥文件。按照在[https://firebase.google.com/docs/admin/setup](https://firebase.google.com/docs/admin/setup)的指示

![](img/5f0b5455d36f03aaca1f89767bd11d04.png)

我们需要前往[https://console . firebase . Google . com/project/_/settings/service accounts/adminsdk](https://console.firebase.google.com/project/_/settings/serviceaccounts/adminsdk)来创建它。也从 Google 的配置示例中复制 url

一旦你下载了 JSON 文件，把它添加到我们后端的根目录。你也应该把它的名字改成更简单的名字。我调用了 mine serviceAccount.json，这是将配置放入它自己的环境变量的好时机。如果你愿意，请随意。

让我们创建一个新文件— authenticateToken.js，并将其放在根目录中。我们将在这里编写中间件:

```
// authenticateToken.jsconst admin = require('firebase-admin');
const serviceAccount require('./serviceAccount.json');admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: '[https://phone-book-fe436.firebaseio.com'](https://phone-book-fe436.firebaseio.com'),
});async function decodeIDToken(req, res, next) {
  const header = req.headers?.authorization;
  if (header !== 'Bearer null' && req.headers?.authorization?.startsWith('Bearer ')) {const idToken = req.headers.authorization.split('Bearer ')[1];try {
      const decodedToken = await admin.auth().verifyIdToken(idToken);
      req['currentUser'] = decodedToken;
    } catch (err) {
      console.log(err);
    }
  }next();
}module.exports = decodeIDToken;
```

我们导入带有 Firebase 配置的 JSON 文件，并使用它在后端初始化 Firebase。我们创建了一个函数——decodeIDToken，它检查传入的请求是否有承载令牌。如果令牌存在，我们将其发送到 Firebase 进行验证。如果被验证，我们把它放到请求中。这样，我们的航线就可以使用它了。

我们还需要将它添加到我们的 index.js 中:

```
// index.js... const decodeIDToken = require('./authenticateToken');...app.use(decodeIDToken);...
```

我们现在能够对每个传入的请求进行身份验证:

```
// controllers/phones.js... phonesRouter.post('/', (req, res) => {
  const auth = req.currentUser;
  if (auth) {
    console.log('authenticated!', auth);
    return res.send('Hi, from within the phones router POST');
  }
  return res.status(403).send('Not authorized')
}); 
```

我们能够直接从请求中获取经过身份验证的用户。如果存在，我们将用户打印到控制台:

![](img/ffd3bb706aec254e24cbe994a2cbc166.png)

## **创建电话簿**

我们终于可以真正开始使用猫鼬了。我们将使用 mongoose 创建一个新的电话簿模型。在我们空空的手机里:

```
// models/phone.jsconst mongoose = require('mongoose');const phoneSchema = new mongoose.Schema({
  name: String,
  number: String,
});phoneSchema.set('toJSON', {
  transform: (doc, returnedObject) => {
    returnedObject.id = returnedObject._id.toString();
    delete returnedObject._id;
    delete returnedObject.__v;
  },
});module.exports = mongoose.model('Phone', phoneSchema);
```

首先，我们创建数据库模式。这里，我们的两个字段都是字符串。就像我们前院的田地。接下来，我们确保从数据库返回的对象包含字符串形式的 id。最初，它是作为一个对象出现的。

这就是我们的模型。我们现在能够发布到数据库。

## 将条目过帐到数据库

为了能够从前端正确管理有效负载，我们需要向我们的应用程序添加一个内置的中间件。添加到 index.js:

```
app.use(express.json());
```

我们将对我们的发布路线进行一些更改:

```
// controllers/phones.js...const Phone = require('../models/phone');...phonesRouter.post('/', (req, res) => {
  const auth = req.currentUser;
  if (auth) {
    const phone = new Phone(req.body);
    const savedPhone = phone.save();return res.status(201).json(savedPhone);
  }
  return res.status(403).send('Not authorized');
});
```

我们验证请求，从我们的模型创建一个新的电话对象并保存它。

如果我们看一下我们的 MongoDB 集合:

![](img/d1ac9b53a822dc80f15c857ac694b9ac.png)

有救了！

趁我们进展顺利，我们来编一条路。

## 从数据库中获取条目

我们编码了我们的获取路线:

```
// controllers/phones.js...phonesRouter.get('/', async (req, res) => {
  const auth = req.currentUser;
  if (auth) {
    const phones = await Phone.find({});
    return res.json(phones.map((phone) => phone.toJSON()));
  }
  return res.status(403).send('Not authorized');
});...
```

猫鼬让我们很容易。通过传入一个空对象，我们查询集合中的所有文档。然后，我们将对每个对象进行循环，应用我们在模型文件中所做的更改。这将删除 __v 字段，并修复 id。之后我们将它返回前端。

# 在前端显示数据

如果我们再看一下我们的步骤:

*   后端将接收请求，并从数据库中提取
*   后端将接收请求，并添加到数据库

我们确实在从数据库中提取数据。我们也在增加数据库。这意味着我们正在接近我们的最终目标。现在，让我们弄清楚如何在前端列出所有条目

我们的服务文件正在返回接收到的数据。让我们从 ListAllNumbers 组件中调用 getPhonebookEntries()。完整的 ListAllNumbers 组件将如下所示:

```
// components/phonebook/ListAllNumbers.jsximport React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';
import { getPhonebookEntries } from '../../services/phonebookServices';const ListAllNumbers = () => {
  const [entries, setEntries] = useState();useEffect(() => {
    const fetchEntries = async () => {
      const fetchedEntries = await getPhonebookEntries();
      setEntries(fetchedEntries);
    }
    fetchEntries();
  }, [])if (entries === undefined) {
    return null;
  }return (
    <div>
      <Link to="/add-number">Add number</Link>
      <h2>Phone numbers</h2>

      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Number</th>
          </tr>
        </thead>
        <tbody>
          {entries.map((entry) => (
          <tr>
            <td>{entry.name}</td>
            <td>{entry.number}</td>
          </tr>
          ))}
        </tbody>
      </table>
    </div>
  ) 
};export default ListAllNumbers;
```

我们建立了一个州来保存我们的参赛作品。我们通过使用 useEffect 钩子中的服务来查询数据库。在模板内部，我们用一个 map 函数循环遍历条目，并显示每个条目。

> 从 React 文档中:
> 
> **`**useEffect**`**是做什么的？**通过使用这个钩子，你告诉 React 你的组件需要在渲染后做一些事情。React 将记住您传递的函数(我们称之为“效果”)，并在执行 DOM 更新后调用它。在这种情况下，我们设置了文档标题，但是我们也可以执行数据获取或者调用其他一些命令式 API。**

**我们从后端接收数据，并将其设置为状态。因为查询数据库是异步操作，所以我们检查条目是否存在。如果没有，我们返回 null。一旦数据从后端返回，条目将自动填充，我们的数据将会出现。**

**如果我们尝试添加另一个条目，并返回到我们的电话簿:**

**![](img/06f66656a00de6960532cb52bc20f0a2.png)**

**一切都按照预期进行存储和提取。**

# **结束语**

**就像我之前说过的，Firebase 并不总是适合这项工作的工具。然而，我们总是需要某种认证。Firebase 为我们带来了一个真正的大礼包。**

**我希望你从这篇大文章中学到了一些东西。你可以在[https://github . com/Devalo/Firebase-auth-react-express-MongoDB](https://github.com/Devalo/Firebase-auth-react-express-mongodb)找到完整的源代码**

**直到下次
斯蒂芬·巴克伦德·瓦卢瓦**