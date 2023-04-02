# Node.js 提示-发送电子邮件、哈希、承诺和快速项目结构

> 原文：<https://javascript.plainenglish.io/node-js-tips-send-emails-hashing-promises-and-express-project-structure-83697d78b1aa?source=collection_archive---------8----------------------->

![](img/571d325c034e1010780d32a48f0d569b.png)

Photo by [Ehimetalor Akhere Unuabona](https://unsplash.com/@theeastlondonphotographer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究编写节点应用程序时常见问题的一些解决方案。

# 异步/等待隐式返回承诺

异步函数隐式返回承诺。

例如，如果我们有:

```
const increment = async (num) => {
  return num + 1;
}
```

然后，这返回了一个承诺`num`增加 1。

这意味着我们可以通过书写来使用它:

```
increment(2)
  .then(num => console.log(num))
```

如果我们传入 2 来递增，那么回调中的`num`应该是 3。

# 获取 Node.js 中字符串的 SHA1 哈希

我们可以通过使用`crypto`模块得到字符串的 SHA1 散列。

例如，我们可以写:

```
const crypto = require('crypto');
const sha = crypto.createHash('sha1');
sha.update('foo');
sha.digest('hex');
```

我们用想要散列的文本来调用`update`。

并调用`digest`返回散列的十六进制字符串。

然而，由于 SHA1 不是很安全，我们应该改为创建 SHA256 哈希:

```
const crypto = require('crypto');
crypto.createHash('sha256').update('foo').digest('hex');
```

我们用`'sha256'`调用`createHash`来创建 SHA256 哈希。

其余的都一样。

# 让 Axios 自动在其请求中发送 Cookies

我们可以通过`withCredentials`选项让 Axios 在其请求中发送 cookies。

例如，我们可以写:

```
axios.get('/api/url', { withCredentials: true });
```

我们只需将其设置为`true`即可发送饼干。

# 带有 Gmail 的节点邮件程序

我们可以使用带有 Gmail SMTP 服务器的 Nodemailer 发送电子邮件。

例如，我们可以写:

```
const nodemailer = require('nodemailer');
const smtpTransport = require('nodemailer-smtp-transport');const transporter = nodemailer.createTransport(smtpTransport({
  service: 'gmail',
  host: 'smtp.gmail.com',
  auth: {
    user: 'email@gmail.com',
    pass: 'password'
  }
}));const mailOptions = {
  from: 'email@gmail.com',
  to: 'friend@example.com',
  subject: 'Hello Email',
  text: 'hello friend'
};transporter.sendMail(mailOptions, (error, info) => {
  if (error) {
    console.log(error);
  } 
  else {
    console.log(info.response);
  }
});
```

我们称之为`createTransport`方法来创建一个`transporter`对象让我们发送电子邮件。

我们输入 SMTP 服务器地址，将`server`设置为`'gmail'`，并输入我们 Gmail 帐户的用户名和地址。

然后，我们可以通过设置发件人和收件人电子邮件、主题和内容`text`来设置邮件选项。

最后，我们用`mailOptions`在`transporter`上调用`sendMail`发送电子邮件。

回调在完成后被调用。

`error`有错误，`info`有发送邮件的结果。

# 用值交换密钥 JSON

我们可以用一个对象的值来交换密钥。

为此，我们可以使用`Object.keys`和 for-of 循环。

例如，我们可以写:

```
const result = {};
for (const key of Object.keys(obj)){
   result[obj[key]] = key;
}
```

我们用`Object.keys`得到键，然后把`obj`的值作为`result`的键，把`key`作为它们的值。

# 构建一个快速应用程序

要构建一个 Express 应用程序，我们可以将生产代码放在根级别的`app`文件夹中。

然后在它里面，我们有控制器的`controllers`文件夹。

`models`文件夹里可以有模特。

`views` foder 有观点。

`test`文件夹就在根目录下，有测试。

在里面，我们有`models`和`views`文件夹，用于每种代码的测试。

# 带有 require 的 Node.js ES6 类

我们可以通过导出来导出我们想要的类到`require`:

`animal.js`

```
class Animal {
  //...
}
module.exports = Animal;
```

然后我们可以写:

`app.js`

```
const Animal = require('./Animal');
const animal = new Anima();
```

我们需要`animal`模块并使用该类。

# 在承诺链中之前或之后放置 catch

如果我们有一个承诺链，那么我们可以将`catch`方法放在任何我们想要捕捉被拒绝的承诺的地方。

然而，如果我们把它们放得更早，那么如果在它之后出现的承诺被拒绝，那么这个错误就不会被`catch`发现。

然而，如果我们把`catch`放在前面，那么后面的承诺可以继续，直到后面来的承诺之一被拒绝。

![](img/2e1f1ab2232570d6522e213a5d19c23a.png)

Photo by [Mariana Montes de Oca](https://unsplash.com/@marianamontesdeoca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以将`catch`方法放在任何我们想要的地方，但是它们可能会根据位置捕获不同的错误。

Axios 可以使用`withCredentials`选项发送 cookies。

我们可以通过获取键和值并交换它们来交换对象的键和值。

Nodemailer 可以用来发送电子邮件。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**