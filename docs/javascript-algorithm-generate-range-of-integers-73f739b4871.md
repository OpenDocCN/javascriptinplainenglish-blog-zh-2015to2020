# JavaScript 算法:生成整数范围

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-generate-range-of-integers-73f739b4871?source=collection_archive---------3----------------------->

![](img/7f6337b9abd796605281b25730dd8bec.png)

对于今天的算法，我们将编写一个名为`generateRange`的函数，它将接受三个整数`min`、`max`和`step`作为输入。

使用这三个整数输入，生成一个从最小值到最大值的整数范围，但在每一定数量的步骤之后。该函数将以数组的形式输出这些数字。根据步数，范围输出不必包括最大值。让我们看一个例子:

```
let min = 2;
let max = 12;
let step = 3;
```

在上面的例子中，我们从 2 数到 12，但是如果我们从 2 开始每隔三个数就数一次，我们会得到下面的数:

```
**2**, 3, 4, **5**, 6, 7, **8**, 9, 10, **11**, 12
// [2, 5, 8, 11]
```

该函数将输出:`[2, 5, 8, 11]`。

现在，我们继续用代码解决这个算法。

我们将创建一个空数组，并将其赋给变量`arr`。

```
let arr = [];
```

然后我们将使用 for 循环。我们的初始数字是`min`，我们一直循环直到到达`max`，每隔`step`次才输出数字。

```
for(let i = min; i <= max; i += step){
    arr.push(i);
}
```

我们将所有这些数字放入我们的`arr`数组。循环结束后，我们返回数组。

```
return arr;
```

在这个函数中，我们学习了如何根据 for 循环创建一个数组并用数字填充它。有了 for 循环，我们的初始值并不总是必须是`0`。我们的增量也是如此。不使用`i++`，如果你想增加一个大于 1 的数字，你可以使用加法赋值操作符`+=`。写入`i += 2`与写入`i = i + 2`相同。

这就结束了我们的功能。以下是剩余的代码:

```
function generateRange(min, max, step){
  let arr = [];
  for(let i = min; i <= max; i += step){
     arr.push(i);
  }

  return arr;
}
```

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://levelup.gitconnected.com/javascript-algorithm-lonely-integer-4397cd8b6ffc) [## JavaScript 算法:孤独的整数

### 我们要写一个函数，输出数组中唯一没有重复的数字。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-lonely-integer-4397cd8b6ffc) [](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [## JavaScript 算法:Pangrams

### 对于今天的算法，我们要写一个叫做 pangrams 的函数，它接受一个字符串 s 作为输入。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [](https://codeburst.io/javascript-algorithm-designer-pdf-viewer-486c68638991) [## JavaScript 算法:设计器 PDF 查看器

### 对于今天的算法，我们将编写一个名为 designerPdfViewer 的函数，它将接受两个输入:一个…

codeburst.io](https://codeburst.io/javascript-algorithm-designer-pdf-viewer-486c68638991)