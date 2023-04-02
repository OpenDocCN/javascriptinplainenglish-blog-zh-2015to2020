# 使用 serve-favicon 在 Express 应用程序中提供网站图标

> 原文：<https://javascript.plainenglish.io/serving-favicons-in-express-apps-with-serve-favicon-e72fd50f72cd?source=collection_archive---------3----------------------->

![](img/56aa1ae8568e782d12403b5a84503e2d.png)

Photo by [Drew Coffman](https://unsplash.com/@drewcoffman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果我们有一个渲染模板的 Express 应用程序，我们可以使用`serve-favicon`中间件来服务 Favicons。

收藏夹图标是浏览器标签左侧的图标。

在本文中，我们将看看如何使用它在用户的浏览器中显示我们最喜欢的 favicon。

# 我们为什么需要这个模块？

`serve-favicon`模块让我们在日志中间件中排除对 favicon 的请求。

它还将图标缓存在内存中，通过减少磁盘访问来提高性能。

此外，它提供了一个基于图标内容的`ETag`，而不是文件系统属性。

最后，这个模块服务于最兼容的`Content-Type`。

该模块专用于为带有 GET `/favicon.ico`请求的图标提供服务。

# 装置

我们可以通过运行以下命令来安装它:

```
npm install serve-favicon
```

# 选择

`favicon`功能由该模块提供。它有两个参数，分别是`path`和`options`，并返回一个中间件来服务来自给定`path`的 favicon。

`options`对象接受一个属性，即`maxAge`属性。它以毫秒为单位设置`cache-control max-age`指令。默认值为 1 yeat。它也可以是一个被`ms`模块接受的字符串。

![](img/1e1120e52cc131c1c09656cd04fbd587.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 例子

## 简单用法

`serve-favicon`中间件应该在其他中间件之前，这样它就不会与其他对`favicon.ico`的请求冲突。

例如，我们可以用`serve-favicon`加载一个图标，如下所示:

```
const express = require('express');
const bodyParser = require('body-parser');
const favicon = require('serve-favicon')
const path = require('path')
const app = express();
app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')))
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.use(express.static('public'));app.get('/', (req, res) => {
  res.sendFile('public/index.html');
});app.listen(3000);
```

然后我们得到:

![](img/bafa2db00287426005bd71ff2d87b76d.png)

## 设置最大年龄

我们也可以如下设置`maxAge`选项:

```
const express = require('express');
const bodyParser = require('body-parser');
const favicon = require('serve-favicon')
const path = require('path')
const app = express();
const iconPath = path.join(__dirname, 'public', 'favicon.ico');
const options = {
  maxAge: 200 * 60 * 60 * 24 * 1000
}
app.use(favicon(iconPath, options));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.use(express.static('public'));app.get('/', (req, res) => {
  res.sendFile('public/index.html');
});app.listen(3000);
```

然后，我们将图标的有效期设置为 200 天。

# 结论

我们可以使用`serve-favicon`中间件来缓存对 favicon 的请求并提供服务，而不是每次用户请求 favicon 时都加载它。

还可以设置过期时间，这样 favicon 请求结果就不会永远被缓存。

它应该在其他中间件之前包含，这样它就不会与其他请求 favicon 的中间件冲突。