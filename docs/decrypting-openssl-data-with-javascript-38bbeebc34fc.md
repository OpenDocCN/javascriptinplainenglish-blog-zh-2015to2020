# 用 JavaScript 解密 OpenSSL 数据

> 原文：<https://javascript.plainenglish.io/decrypting-openssl-data-with-javascript-38bbeebc34fc?source=collection_archive---------11----------------------->

![](img/e93b3658f8182eac38f564bfedf1387a.png)

我最近编写了一个移动应用程序来跟踪库存。库存有大约 1000 条记录，应用程序只读取数据。我选择以 JSON 格式存储数据，并从 AWS S3 存储桶中托管它。这允许使用公共 S3 URL 下载文件。

我不在乎文件是否被其他人下载，所以我不需要认证，但想保持内容的秘密，所以我加密了它。密码短语包含在程序的某个地方，但是用户必须输入 PIN 来完成加密密码短语。因为文件是通过 https 获得的，所以在应用程序解密之前，这可以保持数据的秘密。该文件不应以未加密的形式存储在应用程序中。

在上传到 AWS 之前，我使用了`openssl`命令来加密。我使用 Expo 中的`FileSystem`组件将文件下载到我的 React 本地应用程序中。然后读取、解密和解析加密的文件，创建一个 JSON 对象。该应用程序只是在一个长列表中显示 JSON。

我们需要一个 NPM 模块来为 React Native 解密被`openssl`加密的数据。大多数 NodeJS 程序中都使用了`crypto-js` NPM 模块，但是它在 React Native 中不起作用。需要一个纯 JavaScript 实现。幸运的是，一些好人已经用纯 JavaScript 重新实现了`crypto-js`，并将其发布为`crypto-es` NPM 模块。这个模块可以在 React Native、NodeJS 和浏览器中工作。

`openssl`可以对数据进行二进制或 Base64 编码。我发现使用`FileSystem`组件和`crypto-es`的组合来下载和解密 Base64 编码的文件是最容易的。Base64 编码通常被格式化为 76 个字符的行，在末尾有一个换行符。`openssl`可以创建一个长字符串来代替，这是最好的格式。对文件进行编码的命令行是:

```
% openssl enc -aes-256-cbc -base64 -A < file.txt > encoded_file.txt
```

`-base64`选择带行的 Base64 编码，`-A`使行成为长字符串。

我试过不用`-base64`解密文件，用`crypto-es`解密`-A`都没有成功。我确信这是可能的，但是使用这种格式对我来说效果最好。

我们可以通过 NodeJS 使用这个 JavaScript 文件:

```
import fs from 'fs'; import CryptoES from 'crypto-es'; if (process.argv.length == 3) { const pass = process.argv[2].split(':')[1]; const data = fs.readFileSync(0, 'utf-8'); function decodeBase64String(encrypted, pass) { var decrypted = CryptoES.AES.decrypt(encrypted, pass); var utf8 = CryptoES.enc.Utf8.stringify(decrypted); fs.writeFileSync(1, utf8); } decodeBase64String(data, pass); } else { console.log('need -pass:PASS'); }
```

将该文件保存到 decrypt.js。

现在，让我们用 NodeJS 来解密它:

```
% node decrypt.js -pass:PASS < secretmsg.b64s The file contents are displayed.
```

编写整个移动应用程序要复杂得多，但为`openssl`找出正确的选项集是最具挑战性的，以至于我很长时间都避免在 JavaScript 中使用加密。既然我想通了，我就可以想用多少加密就用多少。希望这也能帮助您将加密技术添加到您的武器库中。

*原载于 2020 年 11 月 29 日【https://focusedforsuccess.com】[](https://focusedforsuccess.com/decrypting-openssl-data-with-javascript/)**。***