# Node.js 提示——全局对象、同步请求、测试的共享代码

> 原文：<https://javascript.plainenglish.io/node-js-tips-global-objects-synchronous-requests-shared-code-for-tests-153191d91c2d?source=collection_archive---------3----------------------->

![](img/61578ba0028b5b1bc813394efe6ca20f.png)

Photo by [riccardo giorato](https://unsplash.com/@riccardogiorato?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# Node.js 中的同步请求

我们可以用`sync-request`包在节点应用程序中发出同步请求。

例如，我们可以写:

```
const request = require('sync-request');try {
  const res1 = request('GET', '/yrl1');
  const res2 = request('GET', '/url2');
  //...
} 
catch (e) {
  //...
}
```

我们导入`sync-request`包并使用它的`request`函数同步发出请求。

然而，像往常一样，同步代码是阻塞的，所以不应该在生产应用程序中使用。

但是，可能对脚本有好处。

# 获取一个 MongoDB 数据库的模式，该数据库是用 Mongoose 在另一个模型中定义的

我们可以用`model`方法得到 Mongoose 模式。

例如，我们可以写:

```
const PersonSchema = require('mongoose').model('person').schema
```

为`person`集合获取 Mongoose 模式。

# 检测 Node.js 或浏览器环境

为了检查一段代码是在 Node 中运行还是在浏览器中运行，我们可以检查`window`对象是否存在。

如果是，那么我们知道它正在浏览器中运行。

否则，它在 Node 中运行。

例如，我们可以写:

```
const app = window ? window : global;
```

以获得相应平台的全局变量。

`window`是浏览器的全局对象。

`global`是节点的全局对象。

# readFile 和 readFileSync 的区别

`fs.readFile`接受回调并异步读取文件。

无论何时完成，回调函数都会被调用并给出结果。

同步读取文件，并在完成后返回文件内容。

我们应该在大多数应用程序中使用`readFile`,因为它不会阻止应用程序的其他部分运行。

然而，`readFileSync`适用于除了脚本中的代码之外，我们不运行任何其他东西的脚本。

# 如何在 Express.js 应用程序中检测环境

我们可以使用`app.get`方法来获取 Express 应用程序中的环境变量。

例如，我们可以写:

```
app.get('env')
```

获取环境变量。

然后我们可以写:

```
if (app.get('env') === 'development') {
  //...
}
```

检查应用程序是否在`'development'`环境中运行。

# 解析 Express 中的表单数据请求有效负载

`body-parser`包让我们可以解析 Express 中的请求负载。

例如，我们可以写:

```
const bodyParser = require('body-parser');
app.use(bodyParser.json());
app.use(bodyParser.urlencoded());

app.use(bodyParser.urlencoded({ extended: true }));app.post('/post',(req, res) => {
  console.log(req.body) 
})
```

我们需要`body-parser`，这样我们就可以调用`json`方法来解析 JSON。

我们调用`urlencoded`以便它可以解析表单数据有效载荷。

`extended`意味着我们可以解析嵌套在表单数据请求中的 JSON。

那么解析后的请求负载将在`req.body`中可用。

或者，我们可以使用`express-formidable`中间件来做同样的事情。

我们可以通过运行以下命令来安装它:

```
npm install express-formidable
```

然后我们可以通过写来使用它:

```
const express = require('express');
const formidable = require('express-formidable');const app = express();app.use(formidable());app.post('/upload', (req, res) => {
  res.send(JSON.stringify(req.fields));
});
```

我们只是传入了`formidable`中间件。

然后我们用`req.fields`对象获得解析后的请求负载。

# 按顺序运行异步 Mocha 共享测试代码

我们可以通过使用`before`钩子来运行多个测试之间共享的代码，从而按顺序运行异步 Mocha 测试。

例如，我们可以写:

```
let someCondition = false;
//...describe('is dependent on someCondition', () => {
  const beforeTest = (done) => {
    if (someCondition) {
      done();
    }
    else {
      setTimeout(() => beforeTest(done), 1000);
    }
  } before((done) => {
    beforeTest(done);
  }); it('does something', () => { 
    // ...
  });})
```

在使用`beforeTest`函数运行测试之前，我们检查是否满足`someCondition`。

否则，我们 1 秒后再调用`beforeTest`。

我们这样做，直到`someCondition`是`true`。

我们将`done`传递给`beforeTest`函数，这样我们就可以在`someConditional`最终变成`true`时调用它。

一旦`done`被调用，测试将会运行。

![](img/13e26c30e7d59ec14d3d6b4b92776986.png)

Photo by [Aleksandr Rogozin](https://unsplash.com/@rogozin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在运行测试之前运行一些检查，并在运行测试之前等待条件变为真。

我们可以使用`sync-request`包来运行同步请求。

然而，它阻塞了应用程序的其余部分，所以它应该只在脚本中使用。

我们可以通过检查哪个全局对象可用来检测一段代码运行的平台。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**