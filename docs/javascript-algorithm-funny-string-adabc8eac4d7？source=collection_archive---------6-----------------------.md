# JavaScript 算法:有趣的字符串

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-funny-string-adabc8eac4d7?source=collection_archive---------6----------------------->

![](img/6446f3fe23472045c484c1803e9852f5.png)

对于今天的算法，我们将编写一个名为`funnyString`的函数，它接受一个输入:一个字符串，`s`。

这个函数的目标是确定一个字符串是否有趣。什么是搞笑串？有趣的字符串是指当您有两个相同字符串的副本时，其中一个是相反的，并且您比较字符串中每个字符及其前一个字符的 ASCII 值的绝对差异。如果原始字符串和反转字符串的绝对差异列表相同，则这是一个有趣的字符串。如果输入的字符串是有趣的字符串，该函数将返回`Funny`，如果不是，则返回`Not Funny`。让我们看一个例子:

```
let s = "abc";
```

我们看我们的文字输入，`abc`。`abc`的反过来就是`cba`。如果我们查看`abc`中的每个字符，并查看每个字符的 ASCII 值，结果如下:

```
a = 97
b = 98
c = 99
```

[这里有一个网站](https://www.cs.cmu.edu/~pattis/15-1XX/common/handouts/ascii.html)，它有一个简短的 ASCII 表，可以很好地用于这个算法。您还可以对 ASCII 表进行图像搜索，以用作快速参考。

当我们查看每个字符的十进制等效值时，它是[97，98，99]。对于反串`cba`，为【99，98，97】。让我们比较两个字符串中每个字符 ASCII 值的绝对差异。

```
**abc**
98 - 97 = 1
99 - 98 = 1**cba** 98 - 99 = -1
97 - 98 = -1
```

我们只关心绝对差异，所以我们会忽略负号。因为原始字符串和反向字符串中每个字符 ASCII 值的绝对差值是相同的，所以这是一个有趣的字符串。该函数将输出`Funny`。

让我们把这变成代码:

```
let sReverse = s.split("").reverse().join("");
```

我们的`sReverse`变量保存了字符串输入的反向版本。

由于没有可以反转字符串的字符串方法，我们使用`split()`方法将字符串转换成数组。空字符串输入使得数组在每个字符处分割字符串。然后我们可以使用`reverse()`方法来反转数组。为了将数组再次转换回字符串，我们使用了`join()`方法。空字符串输入使得我们在连接每个字符时不会在它们之间添加任何东西。

现在我们使用 for 循环遍历输入字符串中的每个字符。需要注意的是，我们希望索引从 1 开始，而不是从 0 开始。我们需要知道每个循环中当前索引位置和前一个位置的数组值。

```
for(let i = 1; i < s.length; i++){
    let difference = Math.abs(s.charCodeAt(i) - s.charCodeAt(i-1));
    let differenceR = Math.abs(sReverse.charCodeAt(i) - sReverse.charCodeAt(i-1));
    if(difference !== differenceR){
      return "Not Funny";
    }
  }
```

分解每一项，让我们看看`difference`和`differenceR` (R 意为反向)变量。

```
let difference = Math.abs(s.charCodeAt(i) - s.charCodeAt(i-1));
let differenceR = Math.abs(sReverse.charCodeAt(i) - sReverse.charCodeAt(i-1));
```

为了获得字符的 ASCII 值，我们使用了`charCodeAt()`方法。我们给它在给定字符串中的字符的索引，我们需要 ASCII 值。我们获取当前索引位置的 ASCII 字符值，并从它之前的索引位置中减去，以获得我们的差值。我们用`Math.abs()`方法包装我们的方程，得到我们的*绝对*差。我们对原始的和反转的字符串做同样的事情。

现在我们知道了我们的绝对差异，我们使用 if 语句来查看两个值是否不相等。当差异不再相等时，弦就不再有趣了。我们返回`“Not Funny”`。

```
if(difference !== differenceR){
   return "Not Funny";
}
```

如果 for 循环没有中断地结束，这意味着原始字符串和反向字符串的每个字符 ASCII 值的绝对差值相等。该功能将返回`Funny`。

```
return "Funny";
```

我们的代码到此结束。下面是该函数的其余部分:

```
function funnyString(s) {
  let sReverse = s.split("").reverse().join("");

  for(let i = 1; i < s.length; i++){
    let difference = Math.abs(s.charCodeAt(i) - s.charCodeAt(i-1));
    let differenceR = Math.abs(sReverse.charCodeAt(i) - sReverse.charCodeAt(i-1));
    if(difference !== differenceR){
      return "Not Funny";
    }
  }

  return "Funny";
}
```

如果你觉得这个算法有帮助，看看我的其他 JavaScript 算法解决方案:

[](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [## JavaScript 算法:Pangrams

### 对于今天的算法，我们要写一个叫做 pangrams 的函数，它接受一个字符串 s 作为输入。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-find-digits-8c639e9ab81a) [## JavaScript 算法:查找数字

### 对于今天的算法，我们将编写一个名为 findDigits 的函数，它将接受一个整数 n 作为输入。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-find-digits-8c639e9ab81a) [](https://codeburst.io/javascript-algorithm-designer-pdf-viewer-486c68638991) [## JavaScript 算法:设计器 PDF 查看器

### 对于今天的算法，我们将编写一个名为 designerPdfViewer 的函数，它将接受两个输入:一个…

codeburst.io](https://codeburst.io/javascript-algorithm-designer-pdf-viewer-486c68638991)