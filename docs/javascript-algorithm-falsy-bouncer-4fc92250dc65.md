# JavaScript 算法:Falsy Bouncer

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-falsy-bouncer-4fc92250dc65?source=collection_archive---------8----------------------->

## 我们写了一个函数，从一个数组中删除所有的假值。

![](img/a5dec45054cc4c58c5b4ae57851b5191.png)

我们将编写一个名为`bouncer`的函数，它将接受一个数组(`arr`)作为参数。

我们得到了一个包含不同类型值的数组。该函数的目标是删除所有的 falsy 值并返回结果数组。JavaScript 中的 Falsy 值有`false`、`null`、`0`、`""`、`undefined`和`NaN`。

示例:

```
let arr = [7, "ate", "", false, 9];//output: [7, "ate", 9];
```

在上面的例子中，只有`""`和`false`是假值。这两个值被删除，函数输出数组。

对于这个算法，我们将使用`filter()`方法过滤掉给定数组中的所有 falsy 值。filter 方法将用回调函数中返回 true 的所有元素创建一个新数组。

在这种情况下，我们只希望数组中的元素是真值。我们将这个新数组赋给一个名为`arr2`的变量。

```
let arr2 = arr.filter(function(currentValue) {
    if (currentValue) {
        return currentValue;
    }
});
```

我们的条件将查看每个值，并评估该值是假值还是真值。如果数组中的任何元素等于这些值中的任何一个:`false`、`null`、`0`、`""`、`undefined`和`NaN`，条件将返回`false`，不会进入`arr2`数组。

我们返回`arr2`数组。

```
return arr2;
```

下面是完整的函数:

如果你觉得这个算法有帮助，看看我的其他 JavaScript 算法解决方案:

[](https://levelup.gitconnected.com/javascript-algorithm-remove-the-final-exclamation-mark-8a7c3c8cd3a2) [## JavaScript 算法:去掉最后一个感叹号

### 我们要写一个函数来删除字符串末尾的感叹号

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-remove-the-final-exclamation-mark-8a7c3c8cd3a2) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-age-appropriate-drinks-1912d2a09d9f) [## JavaScript 算法:适合年龄的饮料

### 我们编写了一个函数，根据一个人的年龄返回他应该喝什么样的饮料。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-age-appropriate-drinks-1912d2a09d9f) [](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [## JavaScript 算法:Pangrams

### 对于今天的算法，我们要写一个叫做 pangrams 的函数，它接受一个字符串 s 作为输入。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) 

【JavaScript 用简单的英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)