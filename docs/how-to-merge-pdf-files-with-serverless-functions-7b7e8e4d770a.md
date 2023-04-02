# 如何用无服务器功能合并 PDF 文件

> 原文：<https://javascript.plainenglish.io/how-to-merge-pdf-files-with-serverless-functions-7b7e8e4d770a?source=collection_archive---------3----------------------->

## 在 AWS Lambda 函数中使用 Ghostscript

![](img/a725cdce9f8dedee1301f0b922c79d03.png)

Photo by [Max LaRochelle](https://unsplash.com/@maxlarochelle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/join?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

无服务器是当今构建简单且经济高效的服务的热门技术。我们还可以使用它在云中运行一些实验，而不需要启动服务器实例。

在本文中，我们将创建一个 lambda 函数来连接 pdf 文件。乍一看，这似乎是一项容易完成的任务。但是当我们需要在无服务器的基础设施中运行我们的代码时，它有一些挑战。

## Ghostscript

![](img/bd0399b730d71dead38e5a8a3ffffb58.png)

为了能够合并 pdf，我们需要一个库或工具来完成。我们在寻找一种无需写太多代码就能合并 pdf 的东西。一个可以像做两个数的和一样连接 pdf 的工具。我们找到了 Ghostscript。

> Ghostscript 是用于 [PostScript](https://en.wikipedia.org/wiki/PostScript) 语言和 [PDF](https://en.wikipedia.org/wiki/PDF) 文件的解释器。它可以从 [Artifex Software，Inc](https://artifex.com/) 获得 [GNU GPL Affero 许可](https://www.gnu.org/licenses/agpl-3.0.html)或[商业使用许可](https://artifex.com/licensing/commercial/)。它已经被积极开发了 30 多年，并在此期间被移植到几个不同的系统中。Ghostscript 由 PostScript 解释器层和图形库组成。

如果您使用的是 Mac，安装 GhostScript 非常简单:

```
brew install ghostscript
```

合并两个文件就像:

```
gs -q -sPAPERSIZE=letter -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -sOutputFile=out.pdf in1.pdf in2.pdf
```

好的，这看起来很简单，但是我们如何将这段代码运行到 Node.js 函数中呢？

## 创建项目

让我们从创建一个新的无服务器项目开始。为此，我们将使用[无服务器框架](https://www.serverless.com/framework/docs/providers/aws/cli-reference/create/)。在您[安装了](https://www.serverless.com/framework/docs/providers/aws/guide/installation/)框架之后，转到您想要存储项目的目录并运行:

```
serverless create --template aws-nodejs --path pdfMergeService
```

这个命令将创建一个空的 Node.js 项目，我们将把它部署到 AWS。

## 获取要合并的 PDF 文件

如果我们想容易地做到这一点，我们可以只添加两个 pdf 文件到项目中，并从那里合并它们。但通常，我们会合并通过我们的服务上传的文件或存储在 AWS S3 桶中的文件。在这个例子中，我们将做最后一个，合并存储在 AWS S3 存储桶中的文件。然后，合并结果将被上传回 S3 存储桶。

在我们继续之前，有一个我们在 Lambda 函数中处理文件时要处理的问题。大多数用于合并 PDF 文件的库或工具都是从磁盘获取文件，而不是动态合并。在 Lambda 基础设施中运行我们的代码的一个问题是，我们无权更改附加到它的文件系统。那么，我们怎么合并文件呢？嗯，Lambda 函数可以访问一个临时目录`/tmp`，这个目录可以在函数运行时用作临时存储。

所以这个过程应该是:

*   从 S3 下载文件到`/tmp`目录
*   合并存储在`/tmp`的文件
*   将结果上传到 S3

先说第一个。我们将在`handler.js`中创建一个新功能:

```
const fs = require('fs');
const AWS = require('aws-sdk');
AWS.config.setPromisesDependency(require('bluebird'));

const s3 = new AWS.S3();const mergeFiles = async (s3Files) => {

    let filesToMerge = "";
    for(const file of s3Files) {
        const paramsFile = {
            Bucket: "<bucket-name>",
            Key: `${file}.pdf`
        };

        let readStream = await s3.getObject(paramsFile).promise();

        await fs.promises.writeFile(`/tmp/${file}.pdf`, readStream.Body);

        filesToMerge += `/tmp/${file}.pdf` + " ";
    }
}
```

这个函数接收一个文件名列表，我们希望从 S3 存储桶中合并这些文件名。然后，它检索每个文件，并将它们存储在`tmp`目录中。

## 合并文件

现在我们已经有了想要合并的文件，我们需要一种方法在 lambda 函数中运行 GhostScript。有几种方法可以做到这一点，但我们发现最简单的方法是使用 Lambda 层。要使用 GhostScript 工具添加层，请转到`serverless.yml`文件，并将以下内容添加到函数声明中:

```
functions:
  hello:
    handler: handler.hello
    memorySize: 128
    timeout: 40
    layers:
      - arn:aws:lambda:us-east-1:764866452798:layer:ghostscript:6
```

这使得在 Lambda 函数中可以通过 Linux 终端使用`gs`命令。但是我们如何从 Node.js 代码运行 Linux 命令呢？我们将使用一个名为`shelljs`的库。更新`mergeFiles`函数，如下所示:

```
const fs = require('fs');
const shell = require('shelljs');

const AWS = require('aws-sdk');
AWS.config.setPromisesDependency(require('bluebird'));

const s3 = new AWS.S3();const mergeFiles = async (s3Files) => {

    let filesToMerge = "";
    for(const file of s3Files) {
        const paramsFile = {
            Bucket: "<bucket-name>",
            Key: `${file}.pdf`
        };

        let readStream = await s3.getObject(paramsFile).promise();

        await fs.promises.writeFile(`/tmp/${file}.pdf`, readStream.Body);

        filesToMerge += `/tmp/${file}.pdf` + " ";
    }

    await shell.exec(`gs -dNOPAUSE -sDEVICE=pdfwrite -sOUTPUTFILE=/tmp/result.pdf -dBATCH ${filesToMerge}`);

}
```

最后，将结果上传到 S3 存储区:

```
const fs = require('fs');
const shell = require('shelljs');

const AWS = require('aws-sdk');
AWS.config.setPromisesDependency(require('bluebird'));

const s3 = new AWS.S3();const mergeFiles = async (s3Files) => {

    let filesToMerge = "";
    for(const file of s3Files) {
        const paramsFile = {
            Bucket: "<bucket-name>",
            Key: `${file}.pdf`
        };

        let readStream = await s3.getObject(paramsFile).promise();

        await fs.promises.writeFile(`/tmp/${file}.pdf`, readStream.Body);

        filesToMerge += `/tmp/${file}.pdf` + " ";
    }

    await shell.exec(`gs -dNOPAUSE -sDEVICE=pdfwrite -sOUTPUTFILE=/tmp/result.pdf -dBATCH ${filesToMerge}`);

    const fileContent = await fs.createReadStream(`/tmp/result.pdf`);

    const params = {
        Bucket: "<bucket-name>",
        Key: `results/result.pdf`,
        Body: fileContent,
        ContentType: "application/pdf"
    };

    const uploadResponse = await s3.upload(params).promise();

    return uploadResponse;
}
```

为了从导出的`hello`模块中调用这个函数，我们可以这样做:

```
await mergeFiles(["file1", "file2"]);
```

其中`file1`和`file2`是我们在 S3 存储桶中的 pdf 文件名。

## 部署解决方案

根据需要自定义代码后，可以通过运行以下命令轻松部署它:

```
sls deploy
```

这将使用您在计算机中配置的 AWS 凭据部署解决方案。(通常运行`aws config`)

## 资源

*   [https://www.ghostscript.com/](https://www.ghostscript.com/)
*   [https://www.serverless.com/](https://www.serverless.com/)
*   [https://aws.amazon.com/lambda/faqs/](https://aws.amazon.com/lambda/faqs/)
*   [https://docs . AWS . Amazon . com/lambda/latest/DG/configuration-layers . html](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)