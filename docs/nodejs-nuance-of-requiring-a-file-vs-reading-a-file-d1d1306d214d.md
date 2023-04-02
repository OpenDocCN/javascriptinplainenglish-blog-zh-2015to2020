# NodeJS —需要文件与读取文件的细微差别

> 原文：<https://javascript.plainenglish.io/nodejs-nuance-of-requiring-a-file-vs-reading-a-file-d1d1306d214d?source=collection_archive---------2----------------------->

## 我们将通过一个**运行 NodeJS 进程**期间**要求**与**直接读取**文件的**小细节**之一。

![](img/04ed46564780e07e1b74a48d8295970e.png)

***注意:这不是对 require 和 fs.readFile*** 的深入比较

## TLDR

如果想在运行 NodeJS 的整个过程中引用文件的最新版本**请使用 **fs** 包，否则如果想让数据**保持不变**或者不希望文件改变，请使用 **require** 。**

# 例子

编程时，通常会引用本地文件并在代码中使用它们，例如:

```
let **data** = **require**(‘**./data.json**’);
// data = {"name": "count": "value": 4}
```

但是，如果您需要更新文件的值，会发生什么呢？您可以尝试以下方式:

```
const **fs** = **require**('**fs**').promises;const **newData** = {"**name**": "count", "**value**": 1000000}await **fs**.**writeFile**('./**data**', JSON.stringify(**newData**))
```

**如果你打开文件，你会看到值已经改变。**

现在，您希望在稍后的代码中引用更新后的文件。然后和之前一样通过**要求**访问文件。

```
let **data** = **require**(‘./**data.json**’);
// data = {"**name**": "count": "**value**": 4}
```

哦…你还在引用原始文件。

让我们试着使用 **fs** 在同一个节点进程中读取文件。

```
let **data** = JSON.parse(**fs**.**readFileSync**('./**data**', 'utf8'))
// data = {"**name**": "count", "**value**": 1000000}
```

现在你正在使用更新的文件！！！

# 结论

我只是想提供一些见解，以防你在没有得到正确版本的文件时遇到问题。请记住，如果想要在整个运行的节点进程中引用文件的**最新版本**，请使用 **fs** 包，否则，如果想要数据**保持不变**或不希望文件发生变化，请使用**要求**。

## 附:更新！

丹尼尔·帕斯卡尔在关于 a `require.cache`的评论中提出了一个很好的观点。一定要去看看！