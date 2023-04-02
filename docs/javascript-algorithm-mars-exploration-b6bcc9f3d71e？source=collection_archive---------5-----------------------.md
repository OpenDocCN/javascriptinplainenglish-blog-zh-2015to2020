# JavaScript 算法:火星探索

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-mars-exploration-b6bcc9f3d71e?source=collection_archive---------5----------------------->

![](img/166bb261c80599ebf75ed1716549354e.png)

对于今天的算法，我们将编写一个名为`marsExploration`的函数，它将接受一个字符串`s`作为输入。

一艘宇宙飞船被送往火星，但坠毁了。它向地球发出一系列“SOS”求救信号。由于宇宙辐射，一些“SOS”信息在传输过程中被改变了。该函数的目标是确定在传输过程中有多少字母被修改。这里有一个例子:

```
let s = "SOFSOSSISSOW";
```

我们的输入是一个字符串，显示当原始信息在传输之前是`“SOSSOSSOSSOS”`时地球接收到了什么。我们将原始消息与修改后的消息进行比较:

```
SO**F**SOSS**I**SSO**W** // the altered message earth received
SOSSOSSOSSOS // the original message
```

以粗体突出显示，在传输过程中仅更改了三个字母，因此，该函数将输出 3。

让我们把这变成代码。

```
let sosCount = s.length/3;
let original = "SOS".repeat(sosCount);
let errorCount = 0;
```

`sosCount`变量计算应该出现在原始消息中的`“SOS”`字符串的数量。我们的输入只给我们修改过的消息，所以我们需要生成原始的字符串。由于字符串`“SOS”`中有 3 个字符，我们将输入的长度计数除以 3。

知道原始消息中应该有多少个`“SOS”`。我们使用 string 方法`repeat()`将一个字符串复制一定的次数。我们使用`sosCount`变量作为参数来指示我们想要重复`“SOS”`的次数。我们将该数字赋给`original`变量。现在我们有了原始的字符串。

`errorCount`变量计算消息中被修改的字母的数量。这是我们将在函数结束时返回的变量。

接下来，我们将使用 for 循环遍历字符串中的所有字母。

```
for(let i = 0; i < s.length; i++){
   if(s[i] != original[i]){
     errorCount++;
   }
 }
```

在 if 语句中，我们比较了来自消息`original`和函数输入`s`的两个字母。如果字母不匹配，我们增加变量`errorCount`。

循环结束后，我们返回`errorCount`变量。这应该能告诉我们有多少字母，如果有的话，在从火星到地球的传输过程中被改变了。

这就结束了我们的功能。以下是剩余的代码:

```
function marsExploration(s) {
  let sosCount = s.length/3;
  let original = "SOS".repeat(sosCount);
  let errorCount = 0;

  for(let i = 0; i < s.length; i++){
    if(s[i] != original[i]){
      errorCount++;
    }
  }

  return errorCount;
}
```

以下是我的一些其他 JavaScript 算法，你可以看看:

[](https://codeburst.io/javascript-algorithm-camelcase-4df119b6216e) [## JavaScript 算法:CamelCase

### 对于今天的简短算法，我们将创建一个名为 camelcase 的函数，它将接受一个字符串输入 s。

codeburst.io](https://codeburst.io/javascript-algorithm-camelcase-4df119b6216e) [](https://levelup.gitconnected.com/javascript-algorithm-beautiful-days-at-the-movies-81561dd03212) [## JavaScript 算法:看电影的美好时光

### 对于今天的算法，我们要写一个名为 beautifulDays 的函数，它接受三个整数作为…

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-beautiful-days-at-the-movies-81561dd03212) [](https://levelup.gitconnected.com/javascript-algorithm-the-hurdle-race-93447c054b38) [## JavaScript 算法:跨栏赛跑

### 对于今天的算法，我们将编写一个名为跨栏赛跑的函数，它将接受一个整数 k 和一个数组高度。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-the-hurdle-race-93447c054b38)