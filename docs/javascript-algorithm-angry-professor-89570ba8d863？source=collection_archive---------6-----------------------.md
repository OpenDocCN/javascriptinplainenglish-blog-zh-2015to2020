# JavaScript 算法:愤怒的教授

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-angry-professor-89570ba8d863?source=collection_archive---------6----------------------->

![](img/1c3edf9622ec18e61b7b2593fc1c590c.png)

对于今天的算法，我们将编写一个名为`angryProfessor`的函数，它将接受两个输入:一个整数`k`和一个数组`a`。

有一位教授对他的学生在过去几周的缺席感到非常沮丧。他决定，如果有一定数量的学生在场，他将取消上课，而不是教一屋子的蟋蟀。

给定数组中每个学生的到达时间和继续上课所需的最小出席人数，如果课程取消，该函数将输出`YES`，否则将输出`NO`。让我们举个例子:

```
let k = 4;
let a = [0, -1, 0, 3, 5];
```

对于我们的输入，`k`变量携带开始上课所需的最少学生人数。

`a`变量是一个数组，包含每个学生的到达时间。所有小于 1 的数字表示学生提前或准时到达。所有大于 0 的数字表示学生迟到的分钟数。

如果我们查看我们的数组，我们会看到只有 3 名学生准时到达(0，-1 和 0)。教授要求至少有 4 名学生到场才能继续上课。由于只有 3 名学生到达，课程被取消，因此函数将输出`YES`。

如果`k = 2`，则课程不会取消，因为 3 名学生到达，而教授只要求 2 名学生开始上课。

让我们把这变成代码:

```
let onTime = a.filter(function(value){
    return value < 1;
});
```

我们首先创建一个名为`onTime`的变量，它将保存所有提前和准时到达的学生的数组。记住，所有小于 1 的数字都表示学生提前或准时到达。我们使用 filter 方法(在回调函数的帮助下)来使用我们的数组输入`a`，提取所有小于 1 的值，并将其放入一个单独的数组中。那个单独的数组是我们的`onTime`变量。

```
if (onTime.length >= k){
    return "NO";
}else{
    return "YES";
}
```

下一步是使用 if 语句来确定类是否被取消。如果到达的学生人数大于开始上课所需的学生人数，则`NO`，不取消上课。如果没有，那么`YES`，课程被取消。

以下是我们代码的其余部分:

```
function angryProfessor(k, a) {

  let onTime = a.filter(function(value){
    return value < 1;
  }); if (onTime.length >= k){
    return "NO";
  }else{
    return "YES";
  }}
```

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://levelup.gitconnected.com/javascript-algorithm-utopian-tree-b38100fff575) [## JavaScript 算法:乌托邦树

### 对于今天的算法，我们要写一个名为 utopianTree 的函数，它接受一个输入:一个整数 n。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-utopian-tree-b38100fff575) [](https://codeburst.io/javascript-algorithm-designer-pdf-viewer-486c68638991) [## JavaScript 算法:设计器 PDF 查看器

### 对于今天的算法，我们将编写一个名为 designerPdfViewer 的函数，它将接受两个输入:一个…

codeburst.io](https://codeburst.io/javascript-algorithm-designer-pdf-viewer-486c68638991) [](https://levelup.gitconnected.com/javascript-algorithm-the-hurdle-race-93447c054b38) [## JavaScript 算法:跨栏赛跑

### 对于今天的算法，我们将编写一个名为跨栏赛跑的函数，它将接受一个整数 k 和一个数组高度。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-the-hurdle-race-93447c054b38)