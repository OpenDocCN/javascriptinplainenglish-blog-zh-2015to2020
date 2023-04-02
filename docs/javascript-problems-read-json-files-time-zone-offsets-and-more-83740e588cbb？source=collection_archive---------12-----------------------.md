# JavaScript 问题—读取 JSON 文件、时区偏移等等

> 原文：<https://javascript.plainenglish.io/javascript-problems-read-json-files-time-zone-offsets-and-more-83740e588cbb?source=collection_archive---------12----------------------->

![](img/44c25cd82fff9a80be518a1dfcebfb0f.png)

Photo by [marina](https://unsplash.com/@marinajune?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 读取节点应用程序中的 JSON 文件

我们可以使用`readFile`或`readFileSync`来读取文件。

然后我们使用`JSON.parse`来解析读取的 JSON。

例如，我们可以写:

```
const fs = require('fs');
const obj = JSON.parse(fs.readFileSync('file', 'utf8'));
```

`readFileSync`同步读取文件，以字符串形式返回文本数据。

然后我们使用`JSON.parse`来解析数据。

如果我们想异步读取一个文件，我们可以写:

```
const fs = require('fs');
const obj;
fs.readFile('file', 'utf8', (err, data) => {
  if (err) throw err;
  obj = JSON.parse(data);
});
```

文本字符串在`data`参数中，而不是返回值中。

# 处理浮点数精度

如果用浮点数做算术，可能会得到意想不到的结果。

例如，如果我们有:

```
0.1 * 0.2
```

然后我们得到:

```
0.020000000000000004
```

而不是 0.02。

如果我们需要精确的结果，那么我们不能使用浮点运算的内置数字类型。

如果我们想隐藏小数位，我们可以使用`toFixed`、`toPrecision`、`Math.trunc`、`Math.floor`或`Math.ceil`方法来隐藏小数位。

第三种选择是只使用整数。

# 用 JavaScript 获取图像大小

我们可以使用`clientWidth`和`clientHeight`属性来获取图像的宽度和高度。

例如，我们可以写:

```
const img = document.getElementById('image'); 
const width = img.clientWidth;
const height = img.clientHeight;
```

我们获取 ID 为`image`的 img 元素，并获取那些属性。

如果我们有一个`Image`实例，我们可以得到`width`和`height`属性:

```
const img = new Image();
img.onload = () => {
  console.log(this.width, this.height);
}
img.src = 'http://placekitten.com/200/200'
```

# 转义 HTML 字符串

我们可以用 jQuery 的`text`方法对 HTML 字符串进行转义。

例如，我们可以写:

```
const someHtmlString = "<script>alert('hi');</script>";
$("div").text(someHtmlString);
```

我们有一个包含 JavaScript 代码的字符串。

`text`方法将返回字符串的转义版本。

我们用转义的 HTML 内容替换 div 的内容。

# 获取客户端的时区偏移量

`Date`实例使用`getTimezoneOffset`方法从客户端计算机获取时区偏移量。

例如，我们可以写:

```
const offset = new Date().getTimezoneOffset();
console.log(offset);
```

然后我们得到以分钟为单位的偏移量。偏移量是 UTC 和当地时区之间的时差。

它是 UTC 时间减去当前时区的时间。

时区不能偏移一小时。有些时区，如纽芬兰时区，比 UTC 晚 3.5 小时。

# 访问 Javascript 对象的第一个属性

我们可以用`Object.kets`方法访问 JavaScript 对象的第一个属性来获取对象的键。

它返回一个数组，所以我们可以用 index 0 来获取索引。

例如，我们可以写:

```
const obj = { foo: 'bar' };
obj[Object.keys(obj)[0]];
```

那么`Object.keys(obj)[0]`就是`'foo'`，所以我们得到`'bar'`。

从 ES2015 开始保证密钥的顺序。非数字键应该按照插入顺序返回。

# 在节点计算机中产生工作线程

我们可以使用`cluster`模块在我们的机器中创建工人。

例如，我们可以写:

```
const numCPUs = require('os').cpus().length;
if (cluster.isMaster) {
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
} else {
  http.Server((req, res) => { ... }).listen(8000);
}
```

如果工作者不是主服务器，那么我们就产生一个新的 HTTP 服务器。

否则，我们通过调用`cluster.fork()`来产生工人。

现在，我们可以在同一台机器上运行多个节点应用程序。

# 查看在元素上激发的事件

要查看一个元素触发的事件，我们可以通过按 F12 进入开发控制台。

然后，我们单击 Sources 选项卡。

在右侧，我们通过 BVreakpoints 和 exp[以及树展开事件监听器。

然后我们点击我们想要收听的事件。

然后，我们可以与我们想要检查的元素进行交互，如果事件被触发，我们将从调试器中获得断点。

![](img/e0a639c43e8fa8072d3380ebae6b3cef.png)

Photo by [YUCAR FotoGrafik](https://unsplash.com/@yucar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用内置属性得到图像的尺寸。

为了使调试更容易，我们可以在控制台中查看事件。

JSON 文件可以在节点应用中读取和解析。

## **简单英语的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**