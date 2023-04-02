# 如何保护您的 JWT 和 REST API

> 原文：<https://javascript.plainenglish.io/how-to-secure-your-jwt-json-web-token-and-your-rest-api-53d9bb4f9833?source=collection_archive---------10----------------------->

## 我如何以最安全的方式在 cookie 中实现 JWT

> 我读过一些关于如何保护你的 JWT 的文章。大多数人的共识是将 JWT 存储在 cookie 中，而存储 JWT 的一些根是本地存储。我会根据自己的想法，尝试以最安全的方式在 cookie 中实现 JWT，但也可以随意指出潜在的错误。

![](img/f36712bb5ea1da7107d47c656aaba7ef.png)

Photo by [Designecologist](https://www.pexels.com/@designecologist?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/silver-imac-displaying-collage-photos-1779487/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

我不会告诉你如何从头开始制作一个应用程序或类似的东西，但如果你想要完整的东西，只需评论，我会在 GitHub 上创建一个功能齐全的应用程序，但现在让我们开始吧。

我们的应用程序将是一个简单的程序，用户可以登录，因为我使用的是 NoSql 数据库，我将使用数据库中自动创建的 **_id** 为用户创建 **JWT** 。

现在我们获取 _id 并创建一个 JWT，然后通过 cookie 发送它。cookie 选项包括

> **httpOnly —在生成 cookie 时使用 httpOnly 标志有助于降低客户端脚本访问受保护 cookie 的风险。
> 过期—设置 cookie 的过期时间。
> 已签名——它有一个签名，因此它可以检测客户端是否修改了 cookie。**

将 JWT 存储在 cookie 中可以降低 XSS 攻击的风险，而使用 httpOnly 选项，CSRF 的风险也可以降到最低。下一部分，是我们如何检索 cookie。

我们只需解析签名的 cookie 并调用 JWT 上的 verify 方法来验证我们的 cookie 或 JWT 是否被任何终端用户修改过。

现在，如果您使用 React 作为前端框架并使用 axios 发送请求，请使用 axios 的**和 Credentials** 属性来发送 cookies。

在我看来，这是保护你的 JWT 和其他 API 的最好方法之一，但不是唯一的方法。请随意留下评论。