# Node.js 最佳实践—秘密

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-secrets-d8e59bdca15b?source=collection_archive---------8----------------------->

![](img/242b21f8f4c4996be531a35fc37f8dfa.png)

Photo by [Peter Žagar](https://unsplash.com/@ortodummie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用 Node.js 生成随机字符串

我们可以用`cryptoRandomBytes`方法生成随机字符串。

其他方法可能不像我们想象的那样随机。

这使得应用程序容易受到加密攻击。

# 证明

我们应该对重要的服务和帐户使用多因素身份认证。

应该经常轮换密码，包括 SSH 密钥。

应该在任何地方应用强密码策略。

不要使用任何默认凭据发布应用。

应该只使用 OAuth、OpenID 等标准身份验证方法。

基本认证是不安全的，所以我们不应该使用它。

Auth 端点应该有速率限制，这样攻击者就不能使用暴力攻击来攻击我们的应用程序。

当登录失败时，我们不应该让用户知道用户名或密码验证失败。

错误消息应该是通用的，以避免猜测。

使用集中的用户管理系统也是避免多个帐户的一个好主意。

# 访问控制

应该遵循最小特权原则。

这意味着应该授予用户最少的权力来完成他们的工作。

除了帐户管理之外，不要使用 root 帐户。

所有实例或容器都应该使用角色或服务帐户运行。

我们可以将权限分配给组，而不是用户。

这使得权限管理更加容易和透明。

# 安全错误配置

对生产环境内部的访问只能通过内部网络进行。

这可以通过 SSH 或其他方式完成。

永远不要暴露内部服务。

内部网络访问应该仅限于少数用户。

如果我们使用 cookies，我们应该将其配置为安全模式，仅通过 SSL 发送。

它们也应该配置到同一个站点，这样只有来自同一个域的请求才会回复给定的 cookies。

我们还应该将 cookie 设置为 HttpOnly，以防止客户端 JavaScript 代码访问 cookie。

服务器应受到严格和限制性访问规则的保护。

应通过安全威胁建模确定威胁的优先级。

应该使用 HTTP 和 TCP 负载平衡器添加 DDOS 攻击保护。

我们应该定期进行渗透测试。

# 敏感数据暴露

我们应该只接受 SSL/TLS 连接，并对报头实施严格的传输安全性。

网络应该分成子网，以确保每个节点拥有最少的访问权限。

应该阻止所有不需要访问互联网的服务和实例访问互联网。

秘密应该存储在 KMS 自动气象站、KMS 谷歌云等保险库产品中。

敏感的实例元数据应该与元数据一起锁定。

传输中的数据应该加密。

秘密不应该出现在日志语句中。

普通密码不应显示在前端，后端不应以纯文本形式存储敏感信息。

# 具有已知安全漏洞的组件

应对 Docker 图像进行扫描，找出已知的漏洞。

应启用自动修补和升级，以避免运行没有安全补丁的过时操作系统版本。

用户应该拥有 ID、访问和刷新令牌，以便访问令牌可以定期刷新并且是短期的。

![](img/2e8cc76c71564d1641bda28bed38ae9a.png)

Photo by [Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

要保护我们的数据安全，需要考虑很多安全问题。

为了保持严格的访问控制，我们必须完成所描述的所有项目，甚至更多。