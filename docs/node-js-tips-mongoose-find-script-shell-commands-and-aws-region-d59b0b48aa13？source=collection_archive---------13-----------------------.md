# Node.js 提示—mongose 查找、脚本、Shell 命令和 AWS 区域

> 原文：<https://javascript.plainenglish.io/node-js-tips-mongoose-find-script-shell-commands-and-aws-region-d59b0b48aa13?source=collection_archive---------13----------------------->

![](img/bc8b71d8f7f413b31af46c45116a5470.png)

fPhoto by [Ray Hennessy](https://unsplash.com/@rayhennessy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 将变量从 Jade 模板文件传递到脚本文件

要将 Jade 模板中的变量传递给脚本，我们可以插入脚本代码。

例如，我们可以写:

```
app.get('/', (req, res) => {
  res.render( 'index', { layout: false, name: 'bar' } );
});
```

`index.jade`

```
script.
  name="#{name}";
```

我们使用了`script`标签，并在`index.jad`中插入了`name`变量。

# 猫鼬选择一个特定的领域与寻找

我们可以通过将字段名作为参数传递来选择带有`find`的特定字段。

例如，我们可以写:

```
app.get('/name', (req, res, next) => {
  dbSchemas.Foo.find({}, 'name', (err, val) => {
    if(err) {
      return next(err);
    }
    res.send(val);
  });
});
```

我们通过将字段名作为第二个参数传递来选择一个`Foo`文档的`name`字段。

# 异步瀑布和异步系列的区别

让我们将结果传递给下一个函数。

`async.series`将所有结果传递给最终回调。

# 为 Node.js 编写异步函数

我们编写回调函数，将错误对象的`err`和`result`作为参数。

例如，我们可以写:

```
file.read('/foo.txt', (err, result) => {
  if (err) {
    return console.log(err);
  }
  doSomething(result);
});
```

回调之前的参数是我们想在`read`中使用的任何参数。

回调是一个节点样式的回调，第一个参数是`err`对象。

`result`有第二个参数的计算结果。

# 在 Node.js AWS SDK 中配置区域

我们可以用`AWS.config.update`方法配置 AWS 区域。

例如，我们可以写:

```
const AWS = require('aws-sdk');
AWS.config.update({ region: 'us-east-1' });
```

配置该区域。

# 将参数传递给承诺函数

为了向 promise 函数传递参数，我们创建了一个函数，它接受我们想要的参数并返回其中的 promise。

例如，我们可以写:

```
const login = (username, password) => {
  return new Promise((resolve, reject) => {
    //...
    if (success) {
      resolve("success");
    } else {
      reject(new Error("error"));
    }
  });
}
```

我们通过使用`Promise`构造函数返回承诺。

`username`和`password`是我们传入的参数。

然后我们可以通过写来使用它:

```
login(username, password)
  .then(result => {
    console.log(result);
  })
```

我们使用`then`来获取传递给`resolve`的值。

# 使用 Node.js 执行 Shell 命令

要在节点应用程序中运行 shell 命令，我们可以使用`child_process`模块。

例如，我们可以写:

```
const spawn = require('child_process').spawn;
const child = spawn('ls', ['-l']);
let resp = "";child.stdout.on('data', (buffer) => { 
  resp += buffer.toString() 
});child.stdout.on('end', () => { 
  console.log(resp) 
});
```

我们使用`child_process`的`spawn`方法。

然后我们将命令作为第一个参数传递给`spawn`。

命令行参数作为第二个参数。

然后我们可以用`child.stdout.on`方法观察结果。

我们监听`'data'`事件以获得输出，并连接发送的部分。

当它结束时，我们听`'end'`来得到整个结果。

# 监控 Node.js 应用程序的内存使用情况

我们可以使用 node-memwatch 包来监视节点应用程序的内存使用情况。

我们可以通过运行以下命令来安装它:

```
npm install memwatch
```

然后我们可以通过写来使用它:

```
const memwatch = require('memwatch');
memwatch.on('leak', (info) => { 
  //...
});
```

我们使用`on`方法来监视内存泄漏。

`info`有关于泄密的信息。

它大概是这样的:

```
{ 
  start: Fri, 29 Jun 2020 15:12:13 GMT,
  end: Fri, 29 Jun 2020 16:12:33 GMT,
  growth: 67984,
  reason: 'heap growth over 5 consecutive GCs (20s) - 11.67 mb/hr' 
}
```

它将向我们展示内存使用的增长。

还有一些方法可以观察堆的使用情况，并区分不同时间的内存使用情况。

内置的`process`模块有`memoryUsage`方法来获取我们 app 的内存使用情况。

例如，我们可以写:

```
process.memoryUsage();
```

来获取内存使用情况。

![](img/ae008e33cf21c5387105bf3d6bb5be08.png)

Photo by [Ozgur Kara](https://unsplash.com/@karaozgur?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过`process`模块或第三方模块观察内存使用情况。

为了将数据传递给 Jade 模板中的脚本标记，我们可以插入代码。

如果我们想要一个带有自己参数的承诺，我们可以在函数中返回一个承诺。

Mongoose 可以用来查找给定字段的数据。