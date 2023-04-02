# JWT 是如何运作的？实施

> 原文：<https://javascript.plainenglish.io/how-jwt-works-in-depth-354cb5dc360d?source=collection_archive---------3----------------------->

![](img/b26b24def70eb09271f24f4ea42d26c7.png)

## 让我们构建一个 NodeJS 库来创建、签名和验证 JWT 令牌

# JWT 的工作方式——实施

为什么以及如何工作？让我们构建一个 NodeJS 库来创建、签名和验证 JWT 令牌。

在本文中，我们将重点构建一个简单的 JWT 库，用于处理 JSON Web 令牌的签名和验证。

# 什么是 JSON Web 令牌(JWT)？

JSON Web Token (JWT)是一个开放标准( [RFC 7519](https://tools.ietf.org/html/rfc7519) )，用于在端点之间以 JSON 对象的形式安全地传输信息。

JWT 由三个主要部分组成，报头、有效载荷和签名，每个部分都用点分隔。我们将简要介绍这些不同的部分。

# 页眉

标头通常指定签名算法和令牌类型。

```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

`alg` -标识用于生成签名的算法，HS256 表示该令牌使用 HMAC-SHA256 进行签名。`typ`——指令牌类型

这个 JSON 被字符串化，然后被 base64url 编码。

```
const header = {
  "alg": "HS256",
  "typ": "JWT"
};const encodedHeader = base64url.encode(JSON.stringify(header));
```

# 有效载荷

第二部分是令牌的有效负载，它包含一组声明和应用程序识别用户所需的任何元数据。

```
{
  "name": "John",
  "iat": 1591024013,
  "exp": 1892023997
}
```

此示例具有标准发布时间声明(`iat`)、到期时间声明(`exp`)和自定义声明`name`。然后，有效负载对象被字符串化并进行 base64Url 编码，形成 JWT 的第二部分。

```
const payload = {
  "name": "John",
  "iat": 1591024013,
  "exp": 1892023997
};
const encodedPayload = base64url.encode(JSON.stringify(payload));
```

# 签名

JWT 的第三部分也是最后一部分是签名。它由经过编码的报头、经过编码的有效载荷、秘密字符串和应用报头中指定的算法组成。

如果我们使用 HMAC SHA256 算法，签名将以如下方式创建:

```
HMACSHA256(encodedHeader + "." + encodedPayload, secret)
```

通过向我们的令牌字符串添加签名，可以验证所提供令牌的所有权和完整性。

# 让我们构建一个简单的节点。JS 库

如果你想快速一瞥最终结果，你可以看看这个 [GitHub repo](https://github.com/alexcambose/jwt) 。

首先，我们需要创建一个新的 NodeJS 项目。

```
npm init -y
```

一旦完成，您应该在您的目录中准备好一个 *package.json* 。这意味着您可以开始安装项目所需的依赖项。

我们将需要 Lodash 和 Base64 模块。

```
npm i -S base64url lodash
```

# 我们的入口

让我们首先创建一个 index.js 文件，它将导出我们的两个主要函数，`verify`和`sign`。

```
// index.js
const verify = require('./verify');
const sign = require('./sign');module.exports = {
  verify,
  sign,
};
```

# 符号功能

我们需要一个函数，它接受一个有效载荷、一个秘密和一些附加选项，并返回一个 JWT。

```
// sign.js
module.exports  = (payloadData, secret, options = {}) => {
};
```

由于 options 对象可以包含报头和有效载荷参数/声明，我们只需要在必要的时候过滤和添加它们。为了简单起见，我将使用洛达什的`pick`和`omit`函数。顾名思义，`_.pick(object, ['key1', 'key2', ...])`函数将获取一个对象和一个键数组，并将返回一个只包含数组中指定的键的新对象。`omit`功能与`pick`相反。更多信息请参见[选择](https://lodash.com/docs/4.17.15#pick)和[省略](https://lodash.com/docs/4.17.15#omit)的正式文件。

A starting point for the sign function

在设置了报头和有效载荷之后，是时候看看我们如何生成签名了。在本指南中，我们将重点介绍如何使用 HMACSHA256 算法生成签名。这可以使用本机`crypto`模块来完成。

```
const crypto =  require('crypto');
// ...
const signature = crypto.createHmac('sha256', secret).update(`${encodedHeader}.${encodedPayload}`).digest('base64');
```

一些潜在的小改进是根据`exp`索赔中提供的秒数自动计算有效期，并在没有指定秘密的情况下自动将标题`alg`设置为`none`。

计算`exp`索赔时:

```
if (options.exp) {
  options.exp += Math.round(new Date().getTime() / 1000);
}
```

并且在没有提供秘密的情况下自动将`alg`设置为无(并且返回没有签名的令牌)

```
if (!secret || header.alg === 'none') {
  header.alg = 'none';
  const encodedHeader = base64url(JSON.stringify(header));return `${encodedHeader}.${encodedPayload}.`;
}
```

最终的工作代码如下所示:

The final sign function

# 验证功能

验证功能需要获取一个令牌和一个秘密，并检查签名是否正确。尽管不一定是必需的，我们也可以对有效载荷进行 base64Url 解码，以便进一步使用。

签名验证意味着，如果我们用秘密对给定的头和有效载荷签名，结果将与令牌中提供的签名相同。

```
// recalculate the signature based on the header and payload
const signature = crypto.createHmac('sha256', secret).update(`${tokenHeader}.${tokenPayload}`).digest('base64');// the essential part, checking if the signatures match
if (base64url.fromBase64(signature) !== tokenSignature) {
  throw new Error('Invaid signature');
}
```

我将再次指出，签名基本上是一个哈希函数，它采用 header+load+secret 作为输入，其中一个部分的任何更改都会完全改变生成的哈希。

除了检查签名是否正确以及我们增加了`exp`理赔计算外，我们还可以在这里检查基于当前日期令牌是否仍然有效。

```
// if the `exp` claim is set, verify if it's not expired
if (payload.exp && payload.exp < Date.now()) {
  throw new Error('Expired token');
}
```

最终的验证函数如下所示:

The verify function

## 演示

让我们尝试创建一个令牌，然后使用我们的新函数对其进行验证。

Demo of signing and verifying a token

请注意，这还不是最终的实现，我强烈建议您坚持使用已经构建好的模块，因为在创建、签名和验证 JWT 时，还有更多的事情需要考虑。本文旨在让您对内核的工作原理有一个初步的了解。

# 让我们构建一个简单的 NodeJS 库

如果你想快速一瞥最终结果，你可以看看这个 [GitHub repo](https://github.com/alexcambose/jwt) 。

首先，我们需要创建一个新的 NodeJS 项目。

```
npm init -y
```

一旦完成，您应该在您的目录中准备好一个 *package.json* 。这意味着您可以开始安装项目所需的依赖项。

我们将需要 Lodash 和 Base64 模块。

```
npm i -S base64url lodash
```

# 我们的入口

让我们首先创建一个 index.js 文件，它将导出我们的两个主要函数，`verify`和`sign`。

```
// index.js
const verify = require('./verify');
const sign = require('./sign');module.exports = {
  verify,
  sign,
};
```

# 符号功能

我们需要一个函数，它接受一个有效载荷、一个秘密和一些附加选项，并返回一个 JWT。

```
// sign.js
module.exports  = (payloadData, secret, options = {}) => {
};
```

由于 options 对象可以包含头部和有效负载参数/声明，我们只需要在必要时过滤和添加它们。出于简单的原因，我将使用 Lodash 的`pick`和`omit`函数。顾名思义，`_.pick(object, ['key1', 'key2', ...])`函数将接受一个对象和一个键数组，并将返回一个只包含数组中指定键的新对象。`omit`功能与`pick`相反。欲了解更多信息，请参见官方文件[选择](https://lodash.com/docs/4.17.15#pick)和[省略](https://lodash.com/docs/4.17.15#omit)。

符号功能的起点

在设置了头部和有效载荷之后，是时候看看我们如何生成签名了。在本指南中，我们将重点介绍如何使用 HMACSHA256 算法生成签名。这可以使用本机`crypto`模块来完成。

```
const crypto =  require('crypto');
// ...
const signature = crypto.createHmac('sha256', secret).update(`${encodedHeader}.${encodedPayload}`).digest('base64');
```

一些潜在的小改进将是根据`exp`声明中提供的秒数自动计算到期日期，并在没有指定秘密的情况下自动将标题`alg`设置为`none`。

用于计算`exp`索赔

```
if (options.exp) {
  options.exp += Math.round(new Date().getTime() / 1000);
}
```

并且在没有提供秘密的情况下自动将`alg`设置为 none(并且还返回没有签名的令牌)

```
if (!secret || header.alg === 'none') {
  header.alg = 'none';
  const encodedHeader = base64url(JSON.stringify(header));return `${encodedHeader}.${encodedPayload}.`;
}
```

最终的工作代码如下所示:

最终符号函数

# 验证功能

verify 函数需要一个令牌和一个秘密，并检查签名是否正确。尽管不是必须的，我们也可以对有效载荷进行 base64Url 解码，以备将来使用。

签名验证意味着，如果我们用秘密对给定的报头和有效载荷进行签名，结果将与令牌中提供的签名相同。

```
// recalculate the signature based on the header and payload
const signature = crypto.createHmac('sha256', secret).update(`${tokenHeader}.${tokenPayload}`).digest('base64');// the essential part, checking if the signatures match
if (base64url.fromBase64(signature) !== tokenSignature) {
  throw new Error('Invaid signature');
}
```

我要再次指出，签名基本上是一个散列函数，它以报头+有效载荷+秘密作为输入，其中任何一部分的改变都会完全改变结果散列。

除了检查签名是否正确之外，由于我们添加了`exp`索赔计算，我们还可以在这里检查基于当前日期的令牌是否仍然有效。

```
// if the `exp` claim is set, verify if it's not expired
if (payload.exp && payload.exp < Date.now()) {
  throw new Error('Expired token');
}
```

最终的验证函数如下所示:

验证功能

## 演示

让我们尝试创建一个令牌，然后使用我们的新函数来验证它。

签名和验证令牌的演示

请注意，这还不是最终的实现，我强烈建议您坚持使用已经构建好的模块，因为在创建、签名和验证 JWT 时，还有更多的事情需要考虑。本文旨在让您对内核的工作原理有一个初步的了解。

你可以在 [GitHub](https://github.com/alexcambose/jwt) 上找到源代码。

感谢阅读！

*附:如果你喜欢这篇文章，点击推荐按钮或与朋友分享，这将意味着很多。*