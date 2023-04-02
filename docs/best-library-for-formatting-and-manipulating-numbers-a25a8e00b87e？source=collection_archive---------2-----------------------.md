# JavaScript 中格式化和操作数字的最佳库

> 原文：<https://javascript.plainenglish.io/best-library-for-formatting-and-manipulating-numbers-a25a8e00b87e?source=collection_archive---------2----------------------->

## 轻松转换数字，以 1.2K、4th、1.2GB 等格式显示

![](img/26b4a0c14fcf55116ba69243d49d87cc.png)

Photo by [Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天，我们将看到一个非常受欢迎和易于使用的库，它允许我们转换和操作数字。

很多时候我们需要显示 1400 到 1.4k，1000000 到 1M 这样的数字。

在字节的情况下，7884486213 到 8.88GB 等，或者在长数字中添加逗号以便更好地理解

我们自己很难处理所有的情况。

因此，为了使我们的任务更容易，有一个称为**数字库。它在 npm 上有大约 441K 的周下载量，所以你可以理解它有多受欢迎。**

使用它非常简单，所以让我们深入了解一下。

有两种使用方法。

1.在脚本标签
2 中包含 CDN。将其作为 npm 软件包安装

我们将使用 npm 的第二种方法

要安装该软件包，请运行以下命令

```
npm install numeral
```

**使用**导入库

```
import numeral from "numeral"; // ES6orconst numeral = require('numeral'); // Nodejs
```

**获取逗号分隔的数字**

```
const commaSeparated = numeral(50000).format("0,0")console.log(commaSeparated); // 50,000
```

**获取 k(千)、m(百万)格式的数字**

```
const kFormatted = numeral(23000).format("0a");const mFormatted = numeral(1230974).format("0.000a");console.log(kFormatted); // 23k
console.log(mFormatted); // 1.321m
```

**以序数格式获取它**

```
const ordinal = numeral(23).format("0o");
console.log(ordinal); // 23rd
```

**以字节格式获取**

```
const bytes = numeral(2348895676).format("0.00b");
console.log(bytes); // 2.35GB
```

**以指数形式获取数据**

```
const exponential = numeral(676565765722).format("0,0e+0");
console.log(exponential); // 7e+11
```

要探索其他格式和示例，请访问[http://numeraljs.com](http://numeraljs.com)

今天到此为止。希望你今天学到了新东西。

**别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周时事通讯，里面有惊人的技巧、诀窍和文章。**