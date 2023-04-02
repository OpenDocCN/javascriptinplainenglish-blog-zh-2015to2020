# Node.js 最佳实践— HTTPS

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-https-a69d8d254426?source=collection_archive---------6----------------------->

![](img/bedace3706fc2f9cca984611a1caaff9.png)

Photo by [Abbie Love](https://unsplash.com/@misslove?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 安全传输

没有 HTTPS，通信是通过发送明文来完成的。

这意味着任何人都可以接入通信信道来获取数据。

因此，我们应该通过 HTTPS 交流。

我们可以使用强密码来加密我们的通信，这样我们就不必担心不安全的通信。

此外，我们必须测试密码、密钥和重新协商是否正确配置。

并且证书必须有效。

我们可以使用 nmap 和 sslyze 这样的工具来检查这些东西。

例如，我们可以运行:

```
nmap --script ssl-cert,ssl-enum-ciphers -p 443,465,993,995 www.example.com
```

检查来自 example.com 的证书

使用 sslyze，我们可以运行:

```
./sslyze.py --regular example.com:443
```

做检查

# HSTS

我们应该添加 Strict-Transport-Security 头来强制 HTTPS 连接到服务器。

标题如下:

```
strict-transport-security:max-age=631138519
```

如果存在，将从 web 应用程序返回。

我们可以检查:

```
curl -s -D- https://example.com/ | grep -i Strict
```

检查是否有以`Strict`开头的报头，以检查报头的响应。

# 拒绝服务

为了防止拒绝服务攻击，我们应该采取一些措施来防止它们。

如果用户尝试登录的次数太多并且失败了，我们应该将他们锁定一段时间。

这减轻了暴力猜测攻击。

少量的登录尝试会在一段时间内禁止更多的登录尝试。

可能需要几分钟，然后会因后续故障而增加。

也有可以挂起应用程序的正则表达式。

像重复分组和重复模式或重叠交替模式这样的模式是正则表达式，会使我们的节点应用程序崩溃。

例如，`(a+)+`这样的模式是易受攻击的正则表达式，会导致大量计算。

为了检查这些正则表达式，我们可以使用 [safe-regex](https://www.npmjs.com/package/safe-regex) 包。

# 错误处理

我们应该小心错误处理，不要泄露我们应用程序的敏感信息。

它们可以泄露对攻击者有用的信息。

因此，我们应该记录它们，而不是显示给用户。

# NPM

我们应该检查我们的节点应用程序中需要的包。

为了检查易受攻击的包，我们可以使用`nsp`包。

我们运行:

```
npm i nsp -g
```

安装软件包。

然后，我们使用以下内容审核包覆面提取:

```
nsp audit-shrinkwrap
```

或者我们可以查看`package.json`:

```
nsp audit-package
```

# 无服务器

无服务器从 AWS Lambda 的引入开始。

这将成为构建应用程序的一种流行方式。

我们可以创建直接从服务器运行的代码。

# Node.js 应用程序的结构

我们先用`npm init`创建一个节点应用。

这将创建`package.json`，它包含我们的节点项目的元数据。

它们包括名称、版本、描述、脚本等等。

它还包含软件包信息。

![](img/01b2db317be91678222634f8839c2fa2.png)

Photo by [Keenan Barber](https://unsplash.com/@keebarber?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一些工具来分析我们的应用程序的安全性。

我们可以通过用`npm init`创建一个`package.json`来构建我们的应用程序。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 找到所有内容的链接！