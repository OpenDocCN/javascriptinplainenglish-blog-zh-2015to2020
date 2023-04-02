# JavaScript 算法:重复一个字符串

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-repeat-a-string-repeat-a-string-696dbc73882b?source=collection_archive---------9----------------------->

## 我们编写了一个函数，它将返回一个字符串 x 次，而不使用循环或 repeat 方法。

![](img/5576ef641752dc7fc0d0d3cac204fc10.png)

我们将编写一个名为`repeatStringNumTimes`的函数，它将接受一个字符串(`str`)和一个整数(`num`)作为参数。

该函数的目标是重复给定的字符串`num`的次数并返回结果。如果`num`小于 1，则返回空字符串。

示例:

```
let str = "*";
let num = 3;// result: "***"
```

对于这个函数，我们将尝试不使用 For 循环、while 循环或`repeat()`方法来解决这个问题。

我们检查`num`是否大于零。如果是，那么我们将使用`Array`构造函数创建一个新的数组，数组中有`num`个空槽。

我们有一个长度为`num`的数组，所以我们使用`fill()`方法用给定的字符串填充每个空槽。

因为我们想返回一个字符串而不是一个数组，所以我们使用`join()`方法将所有元素连接成一个字符串。

结果如下:

```
return num > 0 ? Array(num).fill(str).join("") : "";
```

当然，如果 num 等于或小于 0，我们将返回一个空字符串。

下面是该函数的其余部分:

```
function repeatStringNumTimes(str, num) {
    return num > 0 ? Array(num).fill(str).join("") : "";
}
```

你可以使用循环或重复的方法来解决这个问题，因为这可能是一个初学者更容易的路线。这是另一种创造性的方法，在不使用任何一种方法的情况下，将一个字符串重复一定的次数。

如果你觉得这个算法有帮助，看看我的其他 JavaScript 算法解决方案:

[](https://medium.com/javascript-in-plain-english/javascript-algorithm-soccer-goal-totals-93223792b67f) [## JavaScript 算法:足球进球总数

### 我们将解决并研究在 JavaScript 中，在一个函数中添加多个数字总和的许多方法。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-soccer-goal-totals-93223792b67f) [](https://levelup.gitconnected.com/javascript-algorithm-can-we-divide-it-c86842cb5657) [## JavaScript 算法:可以分吗？

### 我们要写一个函数，如果参数“数”能被 a 和 b 整除，则返回 true 或 false。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-can-we-divide-it-c86842cb5657) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85) [## JavaScript 算法:电子商店

### 对于今天的算法，我们将编写一个名为 getMoneySpent 的函数，它将接受三个输入:两个数组…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85) 

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。