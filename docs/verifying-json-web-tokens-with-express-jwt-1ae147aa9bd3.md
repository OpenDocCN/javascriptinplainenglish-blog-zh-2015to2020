# 用 Express-JWT 验证 JSON Web 令牌

> 原文：<https://javascript.plainenglish.io/verifying-json-web-tokens-with-express-jwt-1ae147aa9bd3?source=collection_archive---------2----------------------->

![](img/f167b82d0e2a5523f140eb83bf328968.png)

Photo by [Gabriel Benois](https://unsplash.com/@gabrielbenois?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

随着单页面应用程序和纯 API 后端的使用，JSON web token(jwt)已经成为向我们的应用程序添加身份验证功能的一种流行方式。

在这篇文章中，我们将看看如何用 Express-JWT 验证 JSON web 令牌。

# 装置

`express-jwt`作为节点包提供。要安装它，我们运行:

```
npm install express-jwt
```

# 使用

我们可以使用`express-jwt`包中包含的`jwt`函数来验证令牌。

`jwt`是一个中间件功能，所以我们不必创建自己的中间件来验证令牌。

例如，我们可以如下使用`jwt`功能:

```
const express = require('express');
const jsonwebtoken = require('jsonwebtoken');
const jwt = require('express-jwt');
const app = express();const secret = 'secret';app.post('/auth', (req, res) => {
  const token = jsonwebtoken.sign({ foo: 'bar' }, secret);
  res.send(token);
})app.get('/protected', jwt({ secret }), (req, res) => {
  res.send('protected');
})app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们调用了`jsonwebtoken.sign`来在`auth`路由中发布令牌。

然后，我们可以通过将`Bearer`和我们的令牌放入授权请求头来调用`protected`路由。

授权头的一个例子是:

```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1Nzk0NzczMDd9.QW7FOvyJQ36dir0199nJTv07VhlNo9_cItGTkdyJeK8
```

如果令牌无效或者不存在，那么我们会得到一个错误。

我们还可以检查其他字段，如`audience`或`issuer`，如下所示:

```
const express = require('express');
const jsonwebtoken = require('jsonwebtoken');
const jwt = require('express-jwt');
const app = express();const secret = 'secret';
const audience = 'http://myapi/protected';
const issuer = 'http://issuer';app.post('/auth', (req, res) => {
  const token = jsonwebtoken.sign({ foo: 'bar' }, secret, { audience, issuer });
  res.send(token);
})app.get('/protected', jwt({ secret, audience, issuer }), (req, res) => {
  res.send('protected');
})app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们将`audience`和`issuer`添加到了`sign`方法调用的第三个参数中的对象，该对象包含了我们想要添加到颁发的令牌中的选项。

然后在`protected`路径中，我们用传递给`jwt`的 options 对象中的`audience`和`issuer`添加了`jwt`中间件。

如果令牌具有与代码匹配的`secret`、`audience`和`issuer`，那么`protected` 路由返回`protected`响应。否则，我们会得到一个错误。

我们可以验证用 Base64 编码的密码生成的令牌，如下所示:

```
const express = require('express');
const jsonwebtoken = require('jsonwebtoken');
const jwt = require('express-jwt');
const app = express();const secret = 'secret';app.post('/auth', (req, res) => {
  const token = jsonwebtoken.sign({ foo: 'bar' }, new Buffer(secret, 'base64'));
  res.send(token);
})app.get('/protected', jwt({ secret: new Buffer(secret, 'base64') }), (req, res) => {
  res.send('protected');
})app.listen(3000, () => console.log('server started'));
```

在上面的代码中，我们将`new Buffer(secret, ‘base64’)`传递给了`sign`方法的第二个参数，该方法从 Base64 编码的秘密生成一个令牌。

然后在`protected`路由处理程序中，我们可以用同样的秘密调用`jwt`中间件函数来验证令牌。

为了验证使用 RSA 私钥生成的令牌，我们可以编写以下代码:

```
const express = require('express');
const jsonwebtoken = require('jsonwebtoken');
const jwt = require('express-jwt');
const fs = require('fs');
const app = express();const publicKey = fs.readFileSync('public.pub');app.post('/auth', (req, res) => {
  const privateKey = fs.readFileSync('private.key');
  const token = jsonwebtoken.sign({ foo: 'bar' }, privateKey, { algorithm: 'RS256' });
  res.send(token);
})app.get('/protected', jwt({ secret: publicKey }), (req, res) => {
  res.send('protected');
})app.listen(3000, () => console.log('server started'));
```

令牌是在`auth`路由中生成的，方法是从文件系统上的一个文件中读取私钥，然后使用该私钥通过`sign`方法对令牌进行签名。

然后，我们可以使用与用于生成令牌的私钥相对应的公钥，通过编写以下内容来验证令牌:

```
jwt({ secret: publicKey })
```

要访问解码后的令牌，我们可以如下使用`req.user`属性:

```
const express = require('express');
const jsonwebtoken = require('jsonwebtoken');
const jwt = require('express-jwt');
const app = express();
const secret = 'secret';app.post('/auth', (req, res) => {
  const token = jsonwebtoken.sign({ foo: 'bar' }, secret);
  res.send(token);
})app.get('/protected', jwt({ secret }), (req, res) => {
  res.send(req.user);
})app.listen(3000, () => console.log('server started'));
```

在`protected`路线中，我们返回`req.user`作为回应。那么我们应该得到这样的结果:

```
{
    "foo": "bar",
    "iat": 1579478314
}
```

在回复内容中。

我们可以通过如下设置`requestProperty`属性来改变解码后的令牌所附加的属性:

```
app.get('/protected', jwt({ secret, requestProperty: 'auth' }), (req, res) => {
  res.send(req.auth);
})
```

然后我们得到与前一个例子相同的响应。

![](img/b74fa5d4ebd14fd65dc0f4efd2697425.png)

Photo by [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`express-jwt`中间件来验证我们的 JSON web 令牌。

它接受一个秘密和其他令牌选项，如`audience`和`issuer`，如果验证成功，则将解码后的令牌设置为 Express request 对象。

如果验证不成功，则返回一个错误。

它支持对称和非对称加密算法。