# JavaScript 算法:病毒式广告

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-viral-advertising-168a872cb557?source=collection_archive---------4----------------------->

![](img/a6a8709e452685ad4d627625cdf67be1.png)

对于今天的算法，我们将编写一个名为`viralAdvertising`的函数，它将接受一个整数`n`作为输入。

想象你正在采用一种新的病毒式广告策略。你推出一款新产品，第一天你就在社交媒体上与 5 个人分享。一半的人喜欢这个广告，每个喜欢这个广告的人都和他们的三个朋友分享这个广告。随着广告接触到更多的人，喜欢的数量每天都在增加。这个函数的目标是输出在`n`天后有多少人喜欢这个广告。让我们做一个例子，统计一下这个广告在 4 天后有多少人喜欢。

我们知道，在第一天，广告与 5 个人分享。

第一天:

*   股份- 5
*   喜欢- 2
*   累计喜欢数- 2

一半的人喜欢这个广告，所以如果我们把答案向下取整，`5 / 2`就是 2。第一天，到目前为止我们已经有 2 个累计赞了。

第二天:

*   股份- 6
*   喜欢- 3
*   累计喜欢数- 5

第二天，前一天喜欢这个广告的两个人和他们的三个朋友`2 * 3 = 6`分享了这个广告，所以在第二天这个广告增长到了 6 个分享。这 6 个人中有一半喜欢广告`6 / 2 = 3`，给我们留下了 3 个喜欢。我们目前有 2 个累计喜欢，所以我们添加了第 1 天和第 2 天的喜欢，并更新了累计喜欢的数量。`cumulative likes = cumulative likes from current day + cumulative likes from previous day`。这种模式一直持续到第`n`天。

第三天:

*   股份- 9
*   喜欢- 4
*   累计喜欢数- 9

第 4 天

*   股份- 12
*   喜欢- 6
*   累计喜欢数- 15

在四天内，广告累计 15 个喜欢，所以该功能将输出 15。

让我们把这变成代码。

```
let shared = 5;
let cumulative = 0;
let liked = 0;
```

我们的`shared`变量从第一天分享广告的人数开始，即 5 人。

`cumulative`变量将携带广告在`n`天内收到的赞数。我们从 0 开始。该函数将输出此变量。

`liked`变量将携带每天的点赞数。我们从 0 开始。

我们使用 for 循环来遍历天数:

```
for (let i = 1; i <= n; i++){
    liked = Math.floor(shared/2);
    cumulative += liked;
    shared = liked * 3;
}
```

查看每条陈述，我们知道在第一天，我们与 5 个人分享了我们的广告。与你分享广告的人中有一半喜欢这个广告。我们使用`Math.floor()`向下舍入。

```
liked = Math.floor(shared/2);
```

在我们的累积变量中，我们使用加法运算符来增加我们每天累积的点赞数。

```
cumulative += liked;
```

每个喜欢这个广告的人，分享给他们的三个朋友。这将是我们第二天开始的股票数量。

```
shared = liked * 3;
```

一旦循环完成，我们返回`cumulative`变量，它将给出我们在`n`天内收到的赞数。

```
return cumulative;
```

下面是剩余的代码，

```
function viralAdvertising(n) {
  let shared = 5;
  let cumulative = 0;
  let liked = 0;

  for (let i = 1; i <= n; i++){
    liked = Math.floor(shared/2);
    cumulative += liked;
    shared = liked * 3;
  }

  return cumulative;
}
```

如果您觉得这个算法有帮助或有用，请查看我的其他 JavaScript 算法解决方案文章:

[](https://levelup.gitconnected.com/javascript-algorithm-time-conversion-71dc15dd13b8) [## JavaScript 算法:时间转换

### 对于今天的算法，我们将创建一个名为 timeConversion 的函数。这个函数将接受一个字符串作为…

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-time-conversion-71dc15dd13b8) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-migratory-birds-848ad6a99ac3) [## JavaScript 算法:候鸟

### 对于今天的算法，我们将编写一个名为 migratoryBirds 的函数，在这个函数中，我们将接受一个…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-migratory-birds-848ad6a99ac3) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85) [## JavaScript 算法:电子商店

### 对于今天的算法，我们将编写一个名为 getMoneySpent 的函数，它将接受三个输入:两个数组…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85)