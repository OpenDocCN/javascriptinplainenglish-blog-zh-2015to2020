# 带节点和 JIMP 的 Base 64 图像

> 原文：<https://javascript.plainenglish.io/base-64-images-with-node-and-jimp-a590a144d827?source=collection_archive---------13----------------------->

## 创建可以嵌入到 HTML 或 CSS 中的图像的字符串表示

![](img/c34c1efa8725292449430defa144c992.png)

Image: [Pixabay](https://pixabay.com/illustrations/finger-touch-hand-structure-769300/)

JIMP 是 JavaScript 图像处理程序，它实际上是一个 NPM 包，它的众多特性之一是能够从图像文件创建 Base 64 字符串。这有多种用途(有些是邪恶的……)，在这篇文章中，我将演示如何在 HTML 文档中嵌入一个 Base 64 编码的图形作为`img src`。这对于像徽标或按钮图形这样的小图像很有用，可以减少 HTTP 请求的数量，虽然我使用的是 HTML，但是 Base 64 字符串也可以用在 CSS 中。

您可能知道，十进制使用数字 0-9，十六进制使用数字 0-9 和字母 A-f。64 进制当然需要 64 个不同的字符，使用 26 个大写字母、26 个小写字母、数字 0-9 和正斜杠字符，以及等号填充。(还有其他一些不太常见的标准。)因此，将文件、流或缓冲区转换为 Base 64 的任何代码将提供仅包含 ASCII 字符的字符串，该字符串可以使用任何协议安全地传输。

对于这个项目，我将编写一对简单的函数来打开一个图像文件，获取它的 Base 64 编码，然后将其插入到一个 HTML 文档中。你可以从 [Github 库](https://github.com/CodeDrome/base64-images-jimp)中抓取源代码。

https://www.npmjs.com/package/jimp,的 JIMP 的完整文档是[你可以安装它:](https://www.npmjs.com/package/jimp)

`npm install --save jimp`

这是生成 base 64 图像并将其嵌入 HTML 文件的源代码。

在`openLogo`函数中，我们读取一个图像文件(包含在源代码中)，然后调用 MIME 类型为`Jimp.AUTO`的`getBase64Async`来保存原始图像的文件类型。然后将得到的 Base 64 字符串传递给`base64ToPage`。

`base64ToPage`函数打开一个现有的 HTML 文件，用 Base 64 字符串替换`{{logo}}`,然后保存文件。这是诸如 Mustache 和 Handlebars 之类的模板引擎所执行的非常粗略的近似，并且只是一种将 Base 64 图像快速“n”到网页中的肮脏方式，而不是产品质量代码。

在现实世界中，使用 JIMP 生成的 base 64 图像的最简单方法可能是将其写入文本文件，然后将其复制/粘贴到您的代码中。生成一个 base 64 图像是非常耗时的，即使对于非常小的图形也是如此，因此不能对每个 HTTP 请求都即时完成—严格来说，这是对每个图像的一次性处理。

现在让我们运行代码。

`node base64image.js`

如果你在浏览器中打开**base64image.htm**，你会看到一个包含这个标志的页面。

![](img/f3755e6a3ae4e773ae90030f16bf6ce6.png)