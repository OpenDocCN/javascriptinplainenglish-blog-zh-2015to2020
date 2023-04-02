# Node.js 提示—发送消息、添加行尾和 S3 上传

> 原文：<https://javascript.plainenglish.io/node-js-tips-send-message-adding-end-of-line-and-s3-uploads-699a5fe466da?source=collection_archive---------11----------------------->

![](img/b60bcf898db2b4602f1254d2a642a02d.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 使用 Socket.io 向特定客户端发送消息

我们可以通过使用`emit`方法用 socket.io 向特定的客户端发送消息。

例如，我们可以写:

```
io.to(socket.id).emit("event", data);
```

我们用我们想要向其发送消息的客户机的 ID 调用`io.to`方法。

# 用 Node.js 中的 JSON 对象进行响应

我们可以使用带有 Express 的`res.json`方法来返回 JSON 响应。

例如，我们可以写:

```
res.json({ foo: 'bar' });
```

返回我们传入的 JSON 对象。

# 检查是否安装了 Node.js

我们可以通过使用`node -v`命令显示节点版本来检查节点是否已安装。

# 使用 node-mongodb-native 选择并更新文档 by _id

为了通过文档的`_id`来选择和更新文档，我们可以使用`mongo.ObjectID`构造函数来创建 ID 对象。

例如，我们可以写:

```
const mongo = require('mongodb');
const id = new mongo.ObjectID(theId);
collection.update({ '_id': id });
```

`theId`是 ID 的字符串。

然后我们用它作为`'_id'`属性的值。

# 如何在 Node.js 中追加到新行

我们可以通过使用`open`和`write`方法在节点应用程序的文件中添加新的一行。

为了添加新行，我们使用`os.EOL`属性来添加新行字符。

这样，新行将在所有平台上正确插入。

例如，我们可以写:

```
const os = require("os");const addNewLine = (text) => {     
  fs.open('/path/to/file', 'a', 666, (e, id) => {
    fs.write(id, `${text}${os.EOL}`, null, 'utf8', () => {
      fs.close(id, () => {
       console.log('file is updated');
      });
    });
  });
}
```

我们创建了`addNewLine`方法来打开文件。

我们用追加权限打开它，如`'a'`所示。

666 是文件权限，包括读和写。

然后我们在回调中获得文件 ID，这样我们就可以用它来更新文件。

在`fs.write`调用中，`id`是文件 ID。第二个参数是文本内容。

`os.EOL`是平台无关的新行常量。

`'utf8'`是 UTF-8 编码。

然后在写入完成时调用回调。

然后我们用`fs.close`清理文件句柄。

# 通过 Node.js 将 base64 编码的图像上传到亚马逊 S3

要用 S3 上传 base64 图像，我们可以将其转换到缓冲区。

然后我们可以调用`putObject`来上传文件。

例如，我们可以写:

```
const AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');
const s3Bucket = new AWS.S3( { params: { Bucket: 'someBucket'} } );const buffer = new Buffer(req.body.imageBinary.replace(/^data:image\/\w+;base64,/, ""),'base64');const data = {
  Key: 123, 
  Body: buffer,
  ContentEncoding: 'base64',
  ContentType: 'image/jpeg'
};s3Bucket.putObject(data, (err, data) => {
  if (err) { 
    console.log(err, data);
  } else {
    console.log('success');
  }
});
```

我们首先从`config.json`文件中获取凭证，以对 AWS SDK 进行身份验证。

然后，我们从 base64 字符串创建一个缓冲区。

我们必须取出`data:image\/\w+;base64`部分。

然后我们创建一个具有`Key`、`Body`、`ContentEncoding`和`ContentType`属性的`data`对象。

`Key`是文件的路径。

`Body`有文件的内容。

`ContentEncoding`有编码。

`ContentType`有数据类型。

然后我们调用`putObject`来上传文件。

`config.json`有:

```
{
  "accessKeyId":"xxxxxxxxxxxxxxxx",
  "secretAccessKey":"xxxxxxxxxxxxxx",
  "region":"us-east-1"
}
```

# 使用节点 child_process 的 exec 的 Stdout 缓冲区问题—超出 maxBuffer

如果我们得到“超出最大缓冲区”错误，我们可以使用设置`exec`上的`maxBuffer`选项来增加缓冲区大小。

例如，我们可以写:

```
exec('ls', { maxBuffer: 1024 ** 2 }, (error, stdout, stderr) => { 
  console.log(error, stdout); 
});
```

我们在第二个参数中设置对象的`maxBuffer`大小。

![](img/3a056616b4cde5e364f6c850f9f8d222.png)

Photo by [Jametlene Reskp](https://unsplash.com/@reskp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`io.to`方法将消息发送给给定的客户端。

要发送 JSON 响应，我们可以使用`res.json`。

我们可以用 ID 更新。

可以增加`exec`的最大缓冲区大小。

我们使用`os.EOL`常量来添加一个平台无关的行尾字符。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**