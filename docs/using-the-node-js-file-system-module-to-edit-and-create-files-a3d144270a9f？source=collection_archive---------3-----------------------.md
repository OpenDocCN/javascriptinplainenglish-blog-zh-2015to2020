# 使用 Node.js 文件系统模块编辑和创建文件

> 原文：<https://javascript.plainenglish.io/using-the-node-js-file-system-module-to-edit-and-create-files-a3d144270a9f?source=collection_archive---------3----------------------->

![](img/ab6c21b5172ee4a904650ec76d091e6a.png)

## JavaScript 的文件系统模块提供了一种使用 JavaScript 访问计算机文件系统的方法

它可以用来读取，创建，更新，删除和重命名文件。这对于编辑包含您想要操作的数据的文件来说非常强大。

我第一次熟悉这个模块是因为我需要将一个包含 JSON 格式的完整汉英词典的文件压缩成更适合我的应用程序的数据量。我需要一种方法来编写 JS 文件，该文件将一个大文件作为输入，过滤数据，创建一个包含 JSON 格式数据的新数组，然后将数据写入一个新文件，我可以将该文件复制到我的应用程序中。我将使用这本名为 CEDICT 的汉英词典(可以在[https://www.mdbg.net/chinese/dictionary?page=cedict](https://www.mdbg.net/chinese/dictionary?page=cedict)下载)。

首先，您需要用 JavaScript 文件(index.js)和您的输入文件初始化一个目录。接下来，需要模块，并将输入和输出文件设置为 JS 文件中的变量。

```
const fs = require('fs')let outputFile = "HSK1.json"const data = require('./cedict_1.json')
```

这里我需要 fs 模块，我将我的输出文件命名为 HSK1.json，并将我的输入文件设置为一个文件路径，以便可以访问它。现在我已经需要输入文件了，我可以像处理任何其他 JavaScript 数据一样处理它。我首先要编辑数据，使其只包含长度为 1 的字符的对象。我用过滤方法来做这个。

```
let output = data.filter(w => w.simplified.length == 1)
```

然后，我将搜索结果与我选择的一组字符进行比较。

```
const characters = ['我', '你', '他', '她', '这', '哪', '谁', '几', '多', '少', '一', '二', '三', '四', '五', '六', '七', '八', '九', '十', '零', '个', '岁', '本', '快', '不', '没', '很', '太', '都', '和', '在', '吗', '家', '北', '上', '下', '里', '年', '月', '日', '点', '水', '菜', '茶', '钱', '猫', '狗', '人', '书', '谢', '请', '是', '有', '看', '听', '叫', '来', '去', '吃', '喝', '开', '住', '爱', '想', '好', '大', '小', '冷', '热']let result = []JSON.stringify(output.forEach(word => {for(let i = 0; i < characters.length; i++){if (characters[i] === word.simplified){result.push(word)}}}))
```

然后我把结果字符串化。

```
let finalOutput = JSON.stringify(result)
```

然后，我使用 fs.writeFile 创建一个包含我编辑的数据的文件。fw.writeFile 有三个参数。第一个是字符串，应该是文件名或描述符。第二是数据本身。第三个是回调函数。

```
fs.writeFile(outputFile, output, function(err) {if (err) {return console.error(err);}})
```

这样，我们可以通过运行以下命令在终端中执行该文件:

```
$ node index.js
```

我们将编写一个包含编辑数据的新文件。