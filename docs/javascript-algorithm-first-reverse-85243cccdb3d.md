# JavaScript 算法:先反向

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-first-reverse-85243cccdb3d?source=collection_archive---------10----------------------->

![](img/e42c24c14983f6dd19522dc9e3239eaf.png)

对于今天的简短算法，我们将编写一个名为`FirstReverse`的函数，它将一个字符串`str`作为输入。

给你一个字符串。该函数的目标是以相反的顺序返回该字符串。这里有一个例子:

```
let str = "Oatmeal is amazing";
// output: gnizama si laemtaO
```

上面的例子字符串反转时，函数将返回`"gnizama si laemtaO"`。

不管怎样，让我们把它变成代码。

为了反转字符串，我们必须使用`reverse()`方法，但是反转方法是一个数组对象，不能在字符串上工作。为了使用它，我们将字符串转换成数组。我们使用`split()`方法将字符串分割成一个字符数组。

现在我们的字符串是一个数组，我们可以使用 reverse 方法，反转数组并用`join()`方法将数组转换回字符串。

```
return str.split("").reverse().join("");
```

我们返回我们的结果，就是这样。以下是剩余的代码:

```
function FirstReverse(str) {
   return str.split("").reverse().join("");
}
```

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://medium.com/javascript-in-plain-english/javascript-algorithm-longest-word-da608ff20244) [## JavaScript 算法:最长的单词

### 我们要写一个函数，输出一个句子中最长的单词。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-longest-word-da608ff20244) [](https://codeburst.io/javascript-algorithm-maximum-toys-7a337e9a5b1f) [## JavaScript 算法:最大玩具

### 对于今天的算法，我们要写一个函数来找出在不超出预算的情况下我们可以买多少玩具。

codeburst.io](https://codeburst.io/javascript-algorithm-maximum-toys-7a337e9a5b1f) [](https://levelup.gitconnected.com/javascript-algorithm-lonely-integer-4397cd8b6ffc) [## JavaScript 算法:孤独的整数

### 我们要写一个函数，输出数组中唯一没有重复的数字。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-lonely-integer-4397cd8b6ffc)