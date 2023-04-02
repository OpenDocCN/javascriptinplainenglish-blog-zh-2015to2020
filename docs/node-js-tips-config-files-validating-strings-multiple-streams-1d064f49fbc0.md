# Node.js 提示—配置文件、验证字符串、多个流等等

> 原文：<https://javascript.plainenglish.io/node-js-tips-config-files-validating-strings-multiple-streams-1d064f49fbc0?source=collection_archive---------5----------------------->

![](img/94da1a63e5f0f21c7565c1d55d4e9ea1.png)

Photo by [Zé Ferrari Careto](https://unsplash.com/@zmefc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# Node.js 的验证库

为了验证 Node 应用程序中的数据，我们可以使用一个库来简化我们的工作。

例如，我们可以使用`validator`包。

我们可以通过书写来使用它”

```
const check = require('validator').check;
const sanitize = require('validator').sanitize;check('test@email.com').len(6, 64).isEmail();
check('abc').isInt();const int = sanitize('0123').toInt();
const bool = sanitize('true').toBoolean();
```

我们使用`len`方法检查电子邮件的长度是否在 6 和 64 之间。

然后我们调用`isEmail`方法来检查它是否是一封电子邮件。

`check`用验证器方法检查字符串。

`sanitize`清理字符串。

`isInt`检查整数。

如果它无效，那么就会抛出一个异常。

将字符串转换成整数。

`toBoolean`将字符串转换成布尔值。

# 将相同的可读流通过管道传输到多个可写目标

我们可以将相同的可读流通过管道传输到多个可写目标。

例如，我们可以写:

```
const spawn = require('child_process').spawn;
const PassThrough = require('stream').PassThrough;const a = spawn('ls');
const b = new PassThrough();
const c = new PassThrough();a.stdout.pipe(b);
a.stdout.pipe(c);let output = '';b.on('data', (chunk) => {
  output += chunk.length;
});b.on('end', () => {
  console.log(count);
  c.pipe(process.stdout);
});
```

我们可以通过管道将使用`spawn`方法运行的`ls` shell 命令的输出传输给`PassThrough`实例。

我们获得了属性`stdout`，这是一个可读的流，并对其调用`pipe`以将其传输到我们用`PassThrough`构造函数创建的可写流。

然后，我们通过向`data`和`end`方法添加监听器来监听流。

# 。rc 配置文件

任何人都可以制作`.rc`文件来创建自己的配置文件。

例如，我们可以创建一个包含 JSON 的`.rc`文件。

例如，我们可以写:

```
const fs = require('fs');
fs.readFile('./.barrc', 'utf8', (err, data) => {
  if (err) {
    return console.error(err);
  }
  console.log(JSON.parse(data));
})
```

我们调用`readFile`来读取文件，然后我们可以解析存储在其中的 JSON。

它还可以保存 YAML 或任何其他标准文本格式。

# 确定一个字符串是否是 MongoDB ObjectID

我们可以通过使用`ObjectId.isValid`方法来确定一个字符串是否是 MongoDB 对象 ID。

例如，我们可以写:

```
const ObjectId = require('mongoose').Types.ObjectId;
ObjectId.isValid('4738');
```

它检查一个字符串是否是 12 个字符长的字符串，或者它的格式是否正确。

# 在 MongoDB 和 Mongoose 中执行全文搜索

我们可以通过创建索引在 MongoDB 中执行全文搜索。

然后我们可以用`find`的方法来搜索我们想要的字段。

例如，我们可以写:

```
const schema = new Schema({
  name: String,
  email: String,
  address: {
    street: String,
  }
});
schema.index({name: 'text', 'address.street': 'text'});
```

我们还可以通过编写以下内容将所有字段包含在索引中:

```
schema.index({'$**': 'text'});
```

那么我们可以通过书写来称呼`find`:

```
Person.find({ $text: { $search: search }})
  .skip(20)
  .limit(20)
  .exec((err, docs) => { 
    //... 
  });
```

# 使用 console.log 更新一行，而不是创建一个新行

我们可以清除现有的行，然后用`process`模块写入新的行。

例如，我们可以写:

```
process.stdout.write("Hello, World");
process.stdout.clearLine();
process.stdout.cursorTo(0);
process.stdout.write("\n");
```

我们用一些文本调用了`write`来在屏幕上显示它。

然后我们打电话给`clearLine`来清理线路。

然后我们用 0 调用`cursorTo`将光标移回左侧。

然后我们在屏幕上写下行尾字符。

# 用 Node.js 将对象写入文件

在 Node app 中把对象写入文件，我们可以用`writeFileSync`来写。

我们可以通过编写以下内容来编写数组:

```
fs.writeFileSync('./data.json', arr.join(',') , 'utf-8');
```

我们调用`join`将条目组合成一个字符串。

然后`writeFileSync`会将字符串写入`data.json`。

要将一个对象的内容写入一个文件，我们可以使用`util.inspect`将其转换成一个格式化的字符串。

然后我们把它写入一个文件。

例如，我们可以写:

```
const util = require('util');
fs.writeFileSync('./data.json', util.inspect(obj) , 'utf-8');
```

![](img/7ede52ece907920a4589640c7180279d.png)

Photo by [Taylor Kopel](https://unsplash.com/@taylorkopel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以把一个对象写入一个文件。

如果我们使用一个库，验证字符串是很容易的。

我们可以将可读流通过管道传输到多个写流中。

此外，我们可以用我们想要的任何格式制作我们自己的`.rc`配置文件。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**