# Node.js 最佳实践—配置文件、观察器和请求

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-profile-watch-and-requests-4f657b02c2ec?source=collection_archive---------1----------------------->

![](img/ad607433c013793a27e580c4a37db8d8.png)

Photo by [John Torcasio](https://unsplash.com/@johntorcasio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用节点的内置分析器

Node 自带分析器，让我们可以观察应用程序的性能。

要使用它，我们只需在运行应用程序时添加`--prof`选项:

```
node --prof app.js
```

然后，我们通过运行以下命令来处理由此输出的 tick 文件:

```
node --prof-process isolate-0x???????-v8.log
```

然后我们可以读取 tick 文件的处理结果。

有一个`[Summary]`部分包含来自概要分析的数据。

它包括运行哪种代码，如库代码和非库代码。

它还显示了代码是用什么语言编写的。

# 使用代码更改监视器自动重启节点应用程序

为了使开发节点应用程序更容易，我们应该使用代码更改监视器。

有了它们，当我们更改代码文件时，应用程序会重新启动。

有几个软件包可以做到这一点。

其中一个是 Nodemon。

我们可以通过运行以下命令来安装它:

```
npm install -g nodemon
```

然后我们使用`nodemon`而不是`node`来运行我们的应用程序。

另一个实现这个功能的包是 Forever。

我们可以通过运行以下命令来安装它:

```
npm install -g forever
```

然后我们开始运行我们的应用程序:

```
forever start app.js
```

它有一些选项，比如将日志附加到文件而不是 stdout，将进程 ID 保存到文件，等等。

Node-supervisor 是我们可以使用的另一个包。

要安装它，我们运行:

```
npm install -g supervisor
```

它有几个选项来改变它的行为，比如出错时不重启，等等。

# 正确使用 Node.js 中的日志记录

`console.log`有几个问题。

一旦我们建立了我们的应用程序，我们必须删除它们，以避免污染我们的日志文件。

此外，我们没有过滤它们的选项。

更好的替代方法是使用`debug`模块进行记录。

要使用它，我们需要通过编写:

```
const debug = require('debug')('my-app');
```

其中`'my-app'`是我们的应用程序名称。

然后我们可以用`debug`函数记录事情:

```
debug("hello world", someVar, someOtherVar);
```

我们可以将任何想要的东西传递给函数来记录它们。

为了在运行应用程序时打开调试消息，我们运行:

```
DEBUG=my-app node app.js
```

当我们需要时，`DEBUG`的值应该匹配我们传入的名字。

应用程序实例的名称也可以有命名空间:

```
const debug = require("debug")("my-app:startup");
```

这让我们可以精确地区分每个级别的调试。

所以我们可以运行:

```
DEBUG=my-app:startup node app.js
```

记录启动消息和:

```
DEBUG=my-app:* node app.js
```

记录命名空间中的所有消息。

# 正确使用承诺

我们应该避免每次提出要求时都做出新的承诺。

相反，我们可以将它放在一个函数中并重用它:

```
const axios = require('axios');const makeRequest = (options) => {
  return axios(options)
};const getRequest = (url) => {
  const options = {
    method: "GET",
    url
  };
  return makeRequest(options);
};const getProfile = (profileId) => {
  return getRequest(`/profile/${profileId}`);
};
```

我们向 Axios 发出请求，它返回一个承诺。

这样，我们可以为所有请求重用通用请求代码。

![](img/17731fce32e4b7a6edd85bbae2f294f6.png)

Photo by [Bradley Ziffer](https://unsplash.com/@bradleyziffer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用内置的分析器来分析我们的应用程序。

此外，当代码发生变化时，我们可以使用包来重启我们的应用程序。

`debug`模块适合记录调试消息。

我们可以创建一个公共函数来处理所有的 HTTP 请求。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**