# 快速响应对象指南—重定向和模板

> 原文：<https://javascript.plainenglish.io/guide-to-the-express-response-object-redirection-and-templates-f6714591bd50?source=collection_archive---------3----------------------->

![](img/a6f70d35e715ad3a3cb5f8838925b263.png)

Photo by [Gabriel Benois](https://unsplash.com/@gabrielbenois?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Express response 对象允许我们向客户端发送响应。

可以发送各种响应，比如字符串、JSON 和文件。此外，除了主体之外，我们还可以向客户端发送各种标头和状态代码。

在本文中，我们将查看响应对象的各种属性，包括发送`Links`响应头、重定向到不同的路径和 URL，以及呈现 HTML。

# 方法

## 资源链接(链接)

`links`方法将一个带有链接的对象连接在一起，并发送带有链接的`Link`响应头。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res) => {
  res
    .links({
      next: '[http://foo.com?page=1'](http://foo.com?page=1'),
      last: '[http://foo.com?page=2'](http://foo.com?page=2')
    })
    .send();
});app.listen(3000, () => console.log('server started'));
```

然后我们得到`Link`响应头的值是:

```
<http://foo.com?page=1>; rel="next", <http://foo.com?page=2>; rel="last"
```

当我们向`/`路径提出请求时。

## 资源位置(路径)

`location`方法发送带有指定的`path`参数作为值的`Location`响应头。

例如，我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res) => {
  res
    .location('[http://medium.com'](http://medium.com'))
    .send();
});app.listen(3000, () => console.log('server started'));
```

然后我们得到:

```
http://medium.com
```

当我们向`/`路由发出请求时，作为`Location`响应头的值。

## 资源重定向([状态，]路径)

`redirect`方法重定向到用指定的`status`指定的 URL 或路径。如果未指定状态，默认值为 302。

例如，我们可以如下使用它来重定向到另一个路由:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res) => {
  res.redirect('/bar');
});app.get('/bar', (req, res) => {
  res.send('bar');
});app.listen(3000, () => console.log('server started'));
```

然后我们应该看到`bar`，因为我们重定向到了`bar`路线。

我们可以指定如下状态代码:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res) => {
  res.redirect(301, '/bar');
});app.get('/bar', (req, res) => {
  res.send('bar');
});app.listen(3000, () => console.log('server started'));
```

我们还可以重定向到完整的 URL:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));app.get('/', (req, res) => {
    res.redirect('[http://google.com'](http://google.com'));
});app.listen(3000, () => console.log('server started'));
```

然后，当我们在浏览器中进入`/`时，我们会在浏览器中显示 Google。

我们可以使用以下内容重定向回推荐人:

```
res.redirect('back')
```

我们也可以重定向到一个更高级别的 URL，默认为`/`。

我们可以如下使用它:

```
const express = require('express');
const bodyParser = require('body-parser');const app = express();
const post = express.Router();app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));post.get('/', (req, res) => {
    res.redirect('..');
});app.get('/', (req, res) => {
    res.send('hi');
});app.use('/post', post);app.listen(3000, () => console.log('server started'));
```

然后，如果我们转到`/post`，就会得到`hi`，因为我们看到了重定向到`'..'`。

![](img/0ae9f543d59dcd224b15cfcd0dc2dc90.png)

Photo by [Joanna Nix](https://unsplash.com/@joanna_nix?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## res.render(视图[，局部变量] [，回调])

`res.render`让我们使用模板来呈现 HTML。一旦我们设置了 HTML 模板引擎，我们就可以使用模板文件来显示结果。

对象拥有我们可以在模板中访问的属性。

而`callback`可以得到错误和呈现的 HTML 字符串。

`view`参数包含带有模板文件名的字符串。视图的文件夹是在我们调用`res.render`之前设置的，所以我们不需要完整的文件夹名。

例如，我们可以使用 Pug 模板来呈现模板，如下所示:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))
app.set('views', './views');
app.set('view engine', 'pug');app.get('/', (req, res, next) => {
  res.render('index', { pageTitle: 'Hello', pageMessage: 'Hello there!' });
})
app.listen(3000, () => console.log('server started'));
```

然后我们可以将名为`index.pug`的模板文件添加到`views`文件夹中，如下所示:

```
html
  head
    title= pageTitle
  body
    h1= pageMessage
```

然后我们得到:

![](img/8de497e743943d3f840d8062e903648d.png)

我们还可以向 render 方法传递一个回调，如下所示:

```
const express = require('express')
const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))
app.set('views', './views');
app.set('view engine', 'pug');app.get('/', (req, res, next) => {
  res.render(
    'index',
    { pageTitle: 'Hello', pageMessage: 'Hello there!' },
    (err, html) => {
      if (err) {
        next(err);
      }
      else {
        console.log(html);
      }
    }
  );
})
app.listen(3000);
```

然后我们从`console.log`中得到渲染后的 HTML:

```
<html><head><title>Hello</title></head><body><h1>Hello there!</h1></body></html>
```

任何错误都将通过`next`功能发送到快速错误处理器。

# 结论

我们可以用`render`方法渲染 HTML 结果。

使用`redirect`方法，我们可以用不同的响应代码重定向到不同的路径和 URL。

我们可以使用`links`和`locations`方法分别发送`Links`和`Locations`响应头。