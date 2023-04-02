# 使用快速会话在服务器上存储用户会话

> 原文：<https://javascript.plainenglish.io/storing-user-sessions-on-the-server-with-express-session-422fe11bc500?source=collection_archive---------0----------------------->

![](img/a811349defeea015b7bc342c7b8827fe.png)

Photo by [Carlos Macías](https://unsplash.com/@carlosm2514?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了存储机密的会话数据，我们可以使用`express-session`包。它将会话数据存储在服务器上，并为客户端提供一个会话 ID 来访问会话数据。

在本文中，我们将研究如何使用它来存储临时用户数据。

# 装置

`express-session`是节点包。我们可以通过运行以下命令来安装它:

```
npm install express-session
```

然后，我们可以通过添加以下内容将它包含在我们的应用程序中:

```
const session = require('express-session');
```

# 使用

`session`函数返回一个中间件，让我们存储会话。它采用具有以下属性的选项对象:

## 甜饼干

会话 ID cookie 的设置对象。默认值为:

```
{ path: '/', httpOnly: true, secure: false, maxAge: null }.
```

它具有以下属性:

**cookie.domain**

它指定了`Domain Set-Cookie`属性的值。默认情况下没有设置域。大多数客户端会认为 cookie 只适用于当前域。

**cookie.expires**

这为`Expires Set-Cookie`属性的值指定了`Date`对象。默认情况下不设置截止日期。这意味着 cookie 将在会话结束时被删除。

如果`expires`和`maxAge`都被设置，那么最后一个被定义。`expires`不该直接设定。我们应该设置`maxAge`。

**cookie.httpOnly**

指定`HttpOnly Set-Cookie`属性的布尔值。如果为真，则设置`HttpOnly`属性。否则就不是了。默认情况下，仅设置`HttpOnly`属性。

**cookie.maxAge**

计算`Expires Set-Cookie`属性时使用的毫秒数。这是通过将当前服务器时间加上`maxAge`毫秒来计算`Expires`日期时间。默认情况下没有设置最大年龄。

**cookie.path**

设置`Path Set-Cookie`属性值。默认设置为`/`，这是域的根路径。

**cookie.sameSite**

用于表示`SameSite Set-Cookie`属性的值的布尔值或字符串。它可以是:

*   `true` —将`SameSite`属性设置为`Strict`，以严格执行相同站点
*   `false` —不设置`SameSite`属性
*   `'lax'` —将`SameSite`属性设置为`Lax`,以表示宽松的同站点执行
*   `'strict'`将`SameSite`属性设置为`Strict`以进行严格的同站点执行

**cookie.secure**

`Secure Set-Cookie`属性的布尔值。设置了`Secure`属性。否则就不是了。

如果设置为`true`，则不会通过 HTTP 连接发送 Cookies。

但是，还是推荐。如果应用托管在代理之后，那么如果`secure`设置为`true`，则必须设置`trust proxy`。

## genid

生成新会话 ID 的函数。默认值是一个使用`uid-safe`库生成 id 的函数。

## 名字

要在响应中设置的会话 ID cookie 的名称。默认值为`'connect.sid'`。

## 代理人

通过`X-Forwarded-Proto`头设置 cookies 时信任反向代理。

默认值为`undefined`。其他可能性包括:

*   `true` —将使用`X-Forwarded-Proto`割台
*   `false` —忽略所有标头，仅当连接是直接 SSL/TLS 连接时，才认为该连接是安全的
*   `undefined` —使用快速设置中的`'trust proxy'`设置

## 重新保存

强制将会话保存回会话存储，即使该会话在请求期间从未被修改。

当一个客户端向我们的服务器发出两个并行请求时，这可能会造成争用情况，并且在一个请求上对会话所做的更改可能会在另一个请求结束时被覆盖。

默认为`true`，但默认可能会在将来改变。`false`作为选项更有意义。

## 旋转

强制在每个响应上设置会话标识符 cookie。到期时间被重置为原来的`maxAge`，重置到期倒计时。

默认为`false`。

当此选项设置为`true`且`saveUnintialized`选项为`false`时，cookie 不会在未初始化会话的响应上设置

## 存储单元化

强制将未初始化的会话保存到存储中。当会话是新的但未被修改时，它是未初始化的。将此项设置为`false`可减少服务器存储空间的使用，并符合存储 cookies 前需要许可的法律。

## 秘密

这是密码签名会话 ID cookie 的必需选项。它可以是一个字符串或多个字符串机密的数组。

## 商店

会话存储实例。它默认为一个新的`MemoryStore`实例。

## 复原

通过`delete`控制取消`req.session`的设置，设置为`null`等的结果。

默认为`'keep'`。可能的值有:

*   `'destroy'` —当响应结束时，会话将被删除
*   `'keep'` —将保留会话，但不保存请求期间所做的修改。

![](img/5d6b5542e04306fb5f8e4692870a3b53.png)

Photo by [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 请求会话

我们通过使用`req.session`来访问会话数据。数据通常是 JSON 格式的，所以嵌套对象没问题。

`req.session`自带以下方法:

## 重新生成(回调)

我们称之为重新生成会话。一旦它被调用，一个新的会话 ID 和`Session`实例将在`req.session`被初始化，并且`callback`将被调用。

## 销毁(回调)

销毁会话。`req.session`将被取消设置。一旦被调用，就会调用`callback`。

## 重新加载(回调)

从存储中重新加载会话数据，并重新填充`req.session`对象。一旦完成，就会调用`callback`。

## 保存(回调)

将会话保存回存储区，用内存中的内容替换存储区的内容。

这个一般不需要调用。

## 触摸()

更新`maxAge`属性。这通常不应该被调用，因为这是自动完成的。

## 身份证明（identification）

我们可以用`id`属性获得会话 ID。

## 甜饼干

该属性包含 cookie 内容。这也让我们可以为用户修改 cookie。

**Cookie.maxAge**

我们可以设置`maxAge`属性来改变以毫秒为单位的到期时间。

**cookie . original image**

我们可以用它来获得最初的`maxAge`值，单位是毫秒。

# 请求会话 ID

我们可以用`sessionId`获得加载会话的会话 ID。

# 实现我们自己的会话存储

会话存储必须用附加方法实现`EventEmitter`。

## store.all(回调)

一个可选的方法，将存储区中的所有会话作为一个数组。`callback`签名是`callback(error, sessions)`，一旦会话被检索，它就会被调用。

## store.destroy(sid，回调)

在给定会话 ID 的情况下，从存储中删除会话所需的方法。回调有签名`callback(error)`。一旦会话被删除，它就会被调用。

## store.clear(回调)

从存储中删除所有会话的可选方法。回调签名是`callback(error)`，它在商店清空后调用。

## 存储长度(回调)

计算存储中所有会话的可选方法。回调有签名`callback(error, len)`。

## store.get(sid，回调)

通过会话 ID 获取会话数据所需的方法(`sid`)。回调具有签名`callback(error, session)`。

如果找到，则返回会话，否则返回`null`或`undefined`。

## store.set(sid、会话、回调)

给定会话 ID`sid`，设置会话所需的方法。一旦会话被设置，回调就具有签名`callback(error)`。

## store.touch(sid、会话、回调)

建议实施此方法。这用于向存储发出给定会话处于活动状态的信号。回调具有签名`callback(error)`。

# 例子

一个例子是存储浏览次数，并将该数据保存 365 天。我们可以这样做:

```
const express = require('express');
const bodyParser = require('body-parser');
const session = require('express-session');
const app = express();
app.set('trust proxy', 1)
app.use(session({
  secret: 'secret',
  resave: true,
  cookie: {
    maxAge: 24 * 60 * 60 * 365 * 1000
  }
}))app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res) => {
  if (req.session.views) {
    req.session.views++;
  }
  else {
    req.session.views = 1;
  }
  res.send(`${req.session.views} views`);
})app.listen(3000);
```

在代码中，我们使用了`secret`以便对 cookie 进行签名。然后在我们的路由处理器中，我们可以通过获取和设置我们创建的`req.session.views`来获取和设置数据。

然后，当我们不断重新加载`/`路线时，我们应该会看到视图的数量。通过设置`maxAge`它将在 365 天内到期，因此该数字将继续上升。

在响应头中，我们应该知道`Set-Cookie`响应头的值如下:

```
connect.sid=s%3Ad6jfPu5awWj3EXPF-MdtU8cPzjOY5NRT.33nyXjKdShPAPyw9WWwY4sywP44IOX5SPSt2WUkH0cs; Path=/; Expires=Mon, 28 Dec 2020 00:47:31 GMT; HttpOnly
```

我们有会话 ID 和到期时间的值。

# 结论

我们可以使用`express-session`包在服务器端保存会话 cookie 数据。

有许多选项，如各种 cookie 属性的内容和到期时间。

其他设置如 ID，是否只在 HTTPS 保存 cookie 等等都可以设置。

cookies 将存储在会话存储中。如果我们对现有的可供我们使用的商店不满意，我们也可以实现我们自己的商店。