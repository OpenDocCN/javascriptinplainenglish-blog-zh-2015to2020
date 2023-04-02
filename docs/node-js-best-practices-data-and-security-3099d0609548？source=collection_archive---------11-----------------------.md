# Node.js 最佳实践—数据和安全性

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-data-and-security-3099d0609548?source=collection_archive---------11----------------------->

![](img/84948f1de674ca8df398e94eed8895b9.png)

Photo by [Overture Creations](https://unsplash.com/@overture_creations?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 启用 TLS/SSL

这是最基本的安全最佳实践。

我们不希望任何人窥探发送和接收的数据。

这也很重要，因为它可以确保数据在传输过程中不会被修改。

我们可以在任何网络主机上安装 SSL 证书。

Let's Encrypt 为我们提供了可以自动更新的免费 SSL 证书。

此外，即使我们通过 SSL 通信，我们的节点应用程序也不应该直接暴露在互联网上。

例如，我们可以写:

```
app.set(‘trust proxy’, ‘1.0.0.0’);
```

让流量从我们的代理流向我们的应用。

然后，除了 Nginx 这样的反向代理之外，我们还可以使用它来使我们的应用成为反向代理。

代理还应该设置`X-Forwarded-Proto: https` HTTP 头。

# 测试 HTTPS 证书传输

我们可以使用 Qualys SSL Labs、nmap、OpenSSL 或 sslyze 测试 HTTPS 证书传输。

我们可以通过运行以下命令来运行 nmap:

```
nmap --script ss-cert,ssl-enum-ciphers -p 443 example.com
```

我们用这些选项测试 SSL 传输。

`example.com`是我们的站点。

443 是 SSL 用来通信的端口。

使用 sslyze，我们运行:

```
sslyze.py --regular example.com:4444
```

对于 OpenSSL 客户端，我们运行:

```
echo ‘q’ | openssl s_client -host example.com -port 443
```

# 检查已知的安全漏洞

我们应该及时了解最新的已知安全漏洞。

我们可以用一些工具来检查我们的依赖关系。包括 Snyk、Node Security Project、Retire.js 等网站。

# 对发送到应用程序的所有不可信数据进行编码

为了防止不受信任的数据运行任何恶意的东西，我们应该净化它们，使它们不能作为代码运行。

有几种方法来编码它们。

HTML 编码可以用后端的 escape-html 包来完成。

标签内的任何内容都会被转义。

CSS 编码可以用 css.escape 库来完成。

JavaScript 编码可以用 js-string-escape 库来完成，它在浏览器和 Node 上都可以工作。

要转义 URL，我们可以在前端使用`encodeURIComponent`函数转义它们。

而`urlencode`可以用在后端。

# 防止参数污染，以阻止可能的未捕获异常

我们应该防止可能产生异常的参数污染。

如果我们对解析的查询字符串使用了意外的数据类型，那么在解析它们的时候我们可能会得到异常，我们试图对它们做一些事情。

例如，如果我们有给定的端点:

```
app.get('/foo', function(req, res){
  if(req.query.name){
    res.status(200).send(req.query.name.toUpperCase())
  } else {
    res.status(200).send('Hi');
  }
});
```

然后，如果我们查询:

```
http://example.com:8080/foo?name=james&name=mary
```

那么`req.query.name`将会是一个数组，因为我们有 2 个带有键`name`的值。

数组没有`toUpperCase`方法，所以我们会有一个错误。

因此，我们应该对此进行检查，这样如果`req.query.name`是一个数组，我们就可以阻止调用`toUpperCase`方法。

![](img/73b3faa9f27ef7541648aa61c4dd1f4f.png)

Photo by [Andrew Spencer](https://unsplash.com/@iam_aspencer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该对任何应用启用 SSL/TLS。我们可以免费获得 SSL 证书。

还有，我们要检查我们的数据，并对它们进行转义。

我们应该检查安全漏洞。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**