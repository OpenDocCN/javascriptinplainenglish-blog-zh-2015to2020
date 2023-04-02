# 具有节点和护照 JS 的 JWT 身份验证

> 原文：<https://javascript.plainenglish.io/jwt-auth-with-node-and-passport-js-c41a91d333e0?source=collection_archive---------0----------------------->

![](img/5adb3e94ce5d4761ad46c819057fd2c1.png)

Cover Image

JWT 是在双方之间安全传输数据的开放标准。它与身份验证系统一起使用，以发出经过身份验证的请求。它包括`header`、`payload`和`signature`。JWT 是一种无状态认证机制，即它在客户端维护会话，而不是将其存储在服务器中。

JWT 的工作流程非常简单。JWT 是用一个秘密密钥创建的，这个秘密密钥将被秘密保存在你的应用程序中。当您的应用程序从任何客户端接收到 JWT 密钥时，您的应用程序会用私钥验证该密钥。从客户端修改公钥将导致身份验证失败。JWT 由用点分隔的三个不同部分组成。最初的部分是标题。第二部分是有效载荷，第三部分是签名。JWT 令牌的语法是

```
HEADER + '.' + PAYLOAD + '.' + SIGNATURE
```

[](https://jwt.io/) [## JWT。超正析象管(Image Orthicon)

### JSON Web Token (JWT)是一种简洁的、URL 安全的方式，用于表示要在双方之间传输的声明。的…

jwt.io](https://jwt.io/) 

Passport JS 是 Node 和 Express JS 的认证中间件。Passport JS 可以与任何 Express JS 应用程序一起使用。它为我们提供了一个名为 Passport JWT 的策略，帮助我们进行认证请求，并验证令牌是否有效。

[](http://www.passportjs.org/packages/passport-jwt/) [## 护照-jwt

### 使用 JSON Web 令牌进行身份验证的 Passport 策略。这个模块让您使用 JSON 来验证端点…

www.passportjs.org](http://www.passportjs.org/packages/passport-jwt/) 

**目录**

1.  [初始化一个节点项目](#5fe1)
2.  [创建 JWT 令牌](#29e0)
3.  [实施 passport 的 JWT 战略](#c91b)

1.  **初始化一个节点项目**

首先，让我们创建一个新的 Node js 项目。下面的命令创建一个新文件夹，然后初始化我们项目的节点。

```
mkdir jwt_passport
cd jwt_passport/
npm init -y
touch index.js
```

现在安装所需的软件包:

```
npm i express cookie-session mongoose passport passport-jwt jsonwebtoken body-parser
```

安装后，将下面的代码复制到您的`index.js`文件中。

`index.js`

```
const express = require('express')
const mongoose = require('mongoose')
**const bodyParser = require('body-parser')** const app = express()**app.use(bodyParser.json())** app.get('/',(req,res)=>{
  res.send('Hello world')
})mongoose.connect(**"mongodb://localhost/louji", {useNewUrlParser: true, useUnifiedTopology: true}**);mongoose.connection.once('open',function(){
  console.log('Connected to Mongo');
}).on('error',function(err){
  console.log('Mongo Error', err);
})app.listen(8000,()=>{
  console.log('Serve is up and running at the port 8000')
})
```

现在使用`node index.js`启动服务器。然后导航到 [http://localhost:8000/](http://localhost:8000/) 。您应该会看到浏览器中显示“Hello world”。

2.**创造 JWT 令牌**

首先，让我们为用户创建一个 JWT 令牌。为此，让我们创建一个注册 API。

因为我们要处理用户注册，所以我们需要一个用户模型。我已经创建了一个带有电子邮件和密码属性的用户模型。将其复制到您的模型文件中。

`Models/User.js`

```
const  mongoose = require('mongoose')**const UserSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
  },
  password: {
    type: String,
    required: true, 
  }
})**module.exports = mongoose.model('User',UserSchema)
```

创建一个名为`passport.js`的新文件，复制下面的代码。这个文件将成为验证我们的 JWT 令牌的中间件。

`passport.js`

```
const passport = require('passport')
**const passportJWT = require("passport-jwt");
const JWTStrategy   = passportJWT.Strategy;**
const ExtractJWT = passportJWT.ExtractJwt;const User = require('./Models/User')passport.use(new JWTStrategy({
 **jwtFromRequest: ExtractJWT.fromAuthHeaderAsBearerToken(),
  secretOrKey   : 'joanlouji'**
},
 function (jwtPayload, done) {
 **return User.findById(jwtPayload.sub)**   .then(user => 
   {
     return done(null, user);
   }
 ).catch(err => 
 {
   return done(err);
 });
}
))
```

创建中间件后，指向`index.js`文件并复制下面的代码。在这段代码中，我创建了一条名为`/register`的新路线，并将用户值插入到数据库中。如果用户值有效，将生成 JWT 令牌。

`index.js`

```
const express = require('express')
const mongoose = require('mongoose')
const bodyParser = require('body-parser') **const User = require('./Models/User')
const jwt = require('jsonwebtoken');
require('./passport')**
const app = express()**genToken = user => {
  return jwt.sign({
    iss: 'Joan_Louji',
    sub: user.id,
    iat: new Date().getTime(),
    exp: new Date().setDate(new Date().getDate() + 1)
  }, 'joanlouji');
}**app.use(bodyParser.json())app.get('/',(req,res)=>{
  res.send('Hello world')
})
**app.post('/register', async function (req, res, next) {
  const { email, password } = req.body;

  //Check If User Exists
  let foundUser = await User.findOne({ email });
  if (foundUser) {
    return res.status(403).json({ error: 'Email is already in use'});
  }

  const newUser = new User({ email, password})
  await newUser.save()** **// Generate JWT token
  const token = genToken(newUser)
  res.status(200).json({token})
});**mongoose.connect("mongodb://localhost/louji", {useNewUrlParser: true, useUnifiedTopology: true});mongoose.connection.once('open',function(){
  console.log('Connected to Mongo');
}).on('error',function(err){
  console.log('Mongo Error', err);
})app.listen(8000,()=>{
  console.log('Serve is up and running at the port 8000')
})
```

现在尝试用邮箱和密码向[http://localhost:8000/register](http://localhost:8000/register)发出 post 请求。如果他们是有效的，你会收到一个 JWT 令牌。

![](img/5fa611418c08ff07ac5861eefbdf0174.png)

JWT Token

3.**实施 passport 的 JWT 战略**

现在让我们创建一个路由来发出一个经过身份验证的请求。要做到这一点，只需通过`passport.authenticate`到路线。这将检查请求是否有 JWT 令牌，以及 JWT 令牌是否有效。

`index.js`

```
**const passport = require('passport')****app.get('/secret', passport.authenticate('jwt',{session: false}),(req,res,next)=>{
  res.json("Secret Data")
})**
```

在没有授权令牌的情况下向[http://localhost:8000/secret](http://localhost:8000/secret)发出 GET 请求时，您将得到一条错误消息。

![](img/747a22e5e7f034698ebc75e8e69a733f.png)

Accessing a route without JWT token

现在用 JWT 令牌发出同样的请求。你会看到秘密数据。

**注**:在令牌前添加持票人。语法是`Authorization: bearer JSON_WEB_TOKEN_STRING`。 [Passport JS 还提供了从请求中获取令牌的其他不同方式](https://github.com/mikenicholson/passport-jwt)。

![](img/4410886a78c2bac42fba71cf4cfeaa3a.png)

Accessing a route with JWT token

如有任何疑问，请随时联系我。电子邮件:sjlouji10@gmail.com。领英:[https://www.linkedin.com/in/sjlouji/](https://www.linkedin.com/in/sjlouji/)

我的 GitHub 上的完整代码:

[](https://github.com/sjlouji/Passport-Strategies---Medium/tree/master/jwt_passport) [## sjlouji/Passport-策略-中等

### 护照策略。在 GitHub 上创建一个帐户，为 sjlouji/Passport-Strategies-Medium 开发做出贡献。

github.com](https://github.com/sjlouji/Passport-Strategies---Medium/tree/master/jwt_passport) 

编码快乐！