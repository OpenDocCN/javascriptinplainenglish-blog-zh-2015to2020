# Node.js 提示—相关文档、解压缩文件和解析 HTML

> 原文：<https://javascript.plainenglish.io/node-js-tips-related-documents-unzipping-files-and-parsing-html-2194858c4eab?source=collection_archive---------7----------------------->

![](img/c847f734b6c54e153dd22e792bb6a459.png)

Photo by [Gustavo Zambelli](https://unsplash.com/@zamax?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在 Socket.io 中创建房间

要在 socket.io 中创建房间，我们只需按名称加入房间。

不一定要创造出来。

例如，我们可以写:

```
socket.on('create', (room) => {
  socket.join(room);
});
```

我们用房间名来听`create`事件。

我们得到我们想要加入的房间的参数`room`。

然后我们可以发出事件，这样`join`就运行了:

```
socket.emit('create', 'room');
```

`'create'`是事件名称。

`'room'`是房间名。

# 在 Mongoose 中使用多模式填充

我们可以链接`populate`调用来选择多个相关的文档。

例如，我们可以写:

```
Person.find()
  .populate('address')
  .populate('phone')
  .exec((err, results) => {
    //...
  });
```

获得带有`address`和`phone`型号的`Person`。

然后我们在`results`中得到所有相关的文档。

此外，我们可以使用空格分隔的字符串调用`populate`来指定多个模式，而无需多次调用`populate`。

例如，我们可以写:

```
Person.find()
  .populate('address phone')
  .exec((err, results) => {
    //...
  });
```

这是从 mongose 3.6 版本开始提供的。

# 在节点应用程序的内存中下载并解压缩一个 zip 文件

我们可以用`adm-zip`包下载并在内存中解压一个 zip 文件。

要安装它，我们运行:

```
npm install adm-zip
```

然后我们可以下载并解压缩一个文件，如下所示:

```
const fileUrl = 'https://github.com/notepad-plus-plus/notepad-plus-plus/releases/download/v7.8.6/npp.7.8.6.bin.zip'const AdmZip = require('adm-zip');
const http = require('http');http.get(fileUrl, (res) => {
  let data = [];
  let dataLen = 0;res.on('data', (chunk) => {
  data.push(chunk);
  dataLen += chunk.length;})
.on('end', () => {
  const buf = Buffer.alloc(dataLen); for (let i = 0, pos = 0; i < data.length,; i++) { 
    data[i].copy(buf, pos); 
    pos += data[i].length; 
  } const zip = new AdmZip(buf);
  const zipEntries = zip.getEntries(); for (const zipEntry of zipEntries) {
    if (zipEntry.entryName.match(/readme/))
      console.log(zip.readAsText(zipEntry));
    }
  };
});
```

我们使用`http`模块的`get`方法发出 GET 请求来获取 zip 文件。

然后，我们将响应数据块放入`data`数组。

一旦我们这样做了，我们用`Buffer.alloc`创建一个新的缓冲区。

然后我们根据位置将`data`中的组块放入缓冲区。

一旦我们将所有内容复制到缓冲区中，我们就用缓冲区创建了一个新的`AdmZip`实例。

然后我们可以从中获取条目。

我们遍历了 for-of 循环中的条目。

# 对 Node.js 使用 DOM 解析器

有几个针对节点应用的 DOM 解析器库。

一个是`jsdom`库。

我们可以通过书写来使用它:

```
const jsdom = require("jsdom");
const dom = new jsdom.JSDOM(`<p>Hello world</p>`);
dom.window.document.querySelector("p").textContent;
```

我们将一个 HTML 字符串传入`JSDOM`构造函数。

然后我们可以使用`dom.window.document.querySelector`方法从解析后的 DOM 树中获取一个元素。

还有图书馆。

它比`jsdom`更快更灵活，但是 API 更复杂。

要使用它，我们可以写:

```
const htmlparser = require("htmlparser2");
const parser = new htmlparser.Parser({
  onopentag(name, attrib){
    console.log(name, attrib);
  }
}, { decodeEntities: true });
parser.write(`<p>Hello world</p>`);
parser.end();
```

我们使用`htmlparser.Parser`构造函数，它通过解析 HTML 字符串时运行的`onopentag`处理程序获取一个对象。

`name`有标签名，`attrib`有属性。

`decodeEntities`意味着我们解码文档内的实体。

`cheerio`是另一个 DOM 解析器库。它有一个类似 jQuery 的 API，所以应该很多人都很熟悉。

我们可以通过书写来使用它:

```
const cheerio = require('cheerio');
const $ = cheerio.load(`<p>Hello world</p>`);
$('p').text('bye');
$.html();
```

将 HTML 字符串加载到一个 DOM 树中。

然后我们可以使用返回的`$`函数来选择元素并操作它。

![](img/df181edfb308719ab4da850d003584c4.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不必明确地创建一个房间来加入它。

我们可以用库压缩文件。

有许多库可以让我们将 HTML 字符串解析成 DOM 树。

我们可以用 Mongoose 的`populate`来获取相关文档。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**