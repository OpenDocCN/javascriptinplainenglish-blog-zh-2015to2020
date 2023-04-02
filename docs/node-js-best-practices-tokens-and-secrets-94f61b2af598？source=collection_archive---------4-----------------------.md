# Node.js 最佳实践—令牌和秘密

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-tokens-and-secrets-94f61b2af598?source=collection_archive---------4----------------------->

![](img/04d5e12b9a9ac04f5230a119bcb8881e.png)

Photo by [Eric Prouzet](https://unsplash.com/@eprouzet?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 支持将 jwt 列入黑名单

我们应该能够将 JSON web 令牌列入黑名单，这样我们就可以锁定恶意用户，

大多数系统都没有这样做的机制。

我们可以添加不可信令牌的列表，以防止它们登录。

# 防止针对授权的暴力攻击

通过速率限制可以防止对授权的暴力攻击。

例如，我们可以通过阻止重复失败的登录请求来限制登录尝试。

# 以非 root 用户身份运行 Node.js

应该使用非根用户来运行节点应用程序。

这样，他们可以在我们的系统中为所欲为。

我们可以把它放到 Docker 图像中，或者用`-u`标志来设置它。

# 使用反向代理或中间件限制有效负载的大小

有效载荷的大小应该受到限制，以避免我们的系统超载。

这有助于防止 DOS 攻击。

如果请求体很小，造成的损害也就小。

我们可以用`limit`选项设置 [express body parser](https://github.com/expressjs/body-parser) 来接受小尺寸的有效载荷。

# 避免 JavaScript eval 语句

我们可以避免 JavaScript `eval`语句。

它们不安全，因为代码是从字符串运行的。

这也使得优化和调试变得不可能。

`setTimeout`、`setInterval`和`Function`构造函数也从字符串中运行代码。

所以我们也应该避免向它们传递字符串。

# 防止邪恶的正则表达式使单线程执行过载

有些正则表达式我们应该避免。

为了使数据验证更容易，我们可以使用 validator.js 这样的库，或者查找可以与 [safe-regex](https://github.com/substack/safe-regex) 一起使用的安全正则表达式，以检测易受攻击的正则表达式模式，从而避免错误。

坏的正则表达式会使我们的应用程序容易受到阻断事件循环的 DOS 攻击。

这会让我们的应用挂起。

# 避免使用变量加载模块

我们不应该用变量调用`require`。

这样，我们就不能让攻击者向`require`函数传递任何东西。

例如，不写:

```
const insecure = require(helperPath);
```

我们写道:

```
const uploadHelpers = require('./helpers/upload');
```

这也适用于我们传入的其他路径，比如当我们用`fs.readFile`读取文件时。

# 在沙箱中运行不安全的代码

如果我们有任何不安全的代码，我们应该在沙箱中运行它们。

这样，他们就无法接触到外部世界，并可能造成损害。

NPM 软件包可以被沙箱化。专用进程也可以被沙箱化。

# 使用子进程时要格外小心

如果我们在节点应用程序中运行子流程，我们应该清理命令字符串，这样我们就可以无风险地运行。

如果我们不逃离他们，那么攻击者可以随心所欲地运行，这可能是灾难性的。

# 对客户端隐藏错误详细信息

如果有任何关于暴露我们系统内部的错误的细节，我们应该对客户隐藏它们。

这样，攻击者找到攻击我们应用程序的方法的机会就大大降低了。

像路径、堆栈跟踪等任何东西都应该被隐藏。

![](img/3a84d7106ed2b2b0d2c91cf1462c920d.png)

Photo by [Lubo Minar](https://unsplash.com/@bubo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该隐藏敏感数据，隔离有风险的代码，并对任何潜在的恶意字符串进行转义。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**