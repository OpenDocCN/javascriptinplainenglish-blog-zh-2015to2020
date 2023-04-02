# Node.js 提示——异步和映射、异步和 MongoDB，以及用 S3 复制文件

> 原文：<https://javascript.plainenglish.io/node-js-tips-async-and-map-async-and-mongodb-and-copying-files-with-s3-df814b7e81d1?source=collection_archive---------5----------------------->

![](img/9f9dc9ec991a8d026510f5eb002c29d1.png)

Photo by [Ilianna Brett](https://unsplash.com/@pineapplepuppy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在映射中调用异步函数

我们可以使用`Promise.all`将函数映射到异步函数。

例如，我们可以写:

```
const asyncMap = async (teachers) => {
  const data = await Promise.all(teachers.map(async teacher => {
    return {
      ...teacher,
      image: `${teacher.name}.jpg`
   }
  }));
  //...
}
```

我们将`teachers`数组映射到带有`map`的承诺数组。

一个`async`函数返回一个承诺，所以我们可以在对`map`的回调之前添加`async`关键字。

然后我们可以使用`await`和`Promise.all`来并行调用所有的承诺。

之后我们可以随心所欲地处理结果。

# 用 Nodemon 监视目录的变化

我们可以使用`--watch`选项来查看多个文件和目录。

例如，我们可以运行:

```
nodemon --watch src app.js
```

观看`src`文件夹和`app.js`。

# 使用 Node.js 的 AWS SDK 将 Amazon S3 中的所有对象从一个前缀复制到另一个前缀

要将对象从一个键复制到另一个键，我们可以使用`listObjects`来列出条目。

然后我们可以使用`copyObject`将条目复制到密钥位置。

例如，我们可以写:

```
const AWS = require('aws-sdk');
const async = require('async');
const bucketName = 'foo';
const oldPrefix = 'bar/';
const newPrefix = 'baz/';
const s3 = new AWS.S3({ params: { Bucket: bucketName }, region: 'us-west-2' });const done = (err, data) => {
  if (err) { console.log(err); }
  else { console.log(data); }
};s3.listObjects({ Prefix: oldPrefix }, (err, data) => {
  if (data.Contents.length) {
    async.each(data.Contents, (file, cb) => {
      const params = {
        Bucket: bucketName,
        CopySource: `${bucketName}/${file.Key}`,
        Key: file.Key.replace(oldPrefix, newPrefix)
      };
      s3.copyObject(params, (copyErr, copyData) => {
        if (copyErr) {
          console.log(copyErr);
        }
        else {
          console.log(params.Key);
          cb();
        }
      });
    }, done);
  }
});
```

我们用`oldPrefix`调用`listObjects`来获取 lol 路径中的项目。

条目存储在`data.Contents`中。

然后我们使用`async.each`遍历每个对象。

`Bucket`有桶名。

`CopySource`有原始路径。

`Key`有了新的钥匙，我们称`replace`为`oldPrefix`，用`newPrefix`代替`oldPrefix`。

我们将`params`对象传递给`copyObject`方法。

然后我们会得到结果。

一旦我们这样做了，我们就调用`cb`来完成迭代。

我们调用前面定义的`done`来打印任何错误或结果。

# Node.js 中的顺序 MongoDB 查询

我们可以使用`async`和`await`以同步方式进行 MongoDB 查询。

它看起来是同步的，因为它是顺序的，但它是异步的。

例如，我们可以写:

```
const getPerson = async () => {
  const db = await mongodb.MongoClient.connect('mongodb://server/db');
  if (await db.authenticate("username", "password")) {
    const person = await db.collection("Person").findOne({ name: "james" });
    await db.close();
    return person;
  }
}
```

`return`返回解析为`person`的承诺。

它实际上并不返回`person`。

`db.collection`获取我们想要查询的集合。

`findOne`在对象中找到一个符合条件的条目。

我们寻找具有`name`值`'james'`的条目。

查询的解析值被设置为`person`。

完成查询后，我们调用`db.close`。

然后我们返回`person`将返回的承诺解析为该值。

# 自动将标题添加到 Express 应用程序中的每个“呈现”响应

我们可以制作自己的中间件，为每个响应添加一个头。

例如，我们可以写:

```
app.use((req, res, next) => {
  res.header('foo', 'bar');
  next();
});
```

使用`res.header`，我们将带有值`'bar'`的`'foo'`响应头添加到所有响应中。

然后我们调用`next`运行下一个中间件。

这段代码应该添加到路由之前，这样它就可以在每个路由之前运行。

![](img/bd6f55a660c4a778054ec8ea472076c1.png)

Photo by [Saketh Upadhya](https://unsplash.com/@saketh_upadhya?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以调用`map`将每个条目映射到一个承诺。

然后我们调用`Promise.all`来并行调用它们。

我们可以使用 AWS SDK 在 S3 复制文件。

MongoDB 方法支持承诺。

我们可以用中间件给所有的路由添加响应头。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**