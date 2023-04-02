# JavaScript 算法:发现者拥有者

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-finders-keepers-87c132354c53?source=collection_archive---------4----------------------->

## 我们编写一个函数，它将返回数组中通过另一个函数的真值测试的第一个数字。

![](img/3c18210b0a36f5eb97a46398af79a9bd.png)

我们将编写一个名为`findElement`的函数，它将接受一个数组(`arr`)和一个函数(`func`)作为参数。

给你一个包含整数的数组。您还会得到一个返回布尔值的函数。该函数的目标是将数组中的每个数字输入到该函数中。从我们的第二个参数返回的第一个数字是函数将返回的数字。

如果第二个参数没有为数组中的任何数字返回 true，则返回`undefined`。

这里有一个例子:

```
let arr = [1, 3, 5, 8, 9, 10];
function(num) { return num % 2 === 0; }) // the second argument
```

对于上面的例子，函数将返回`8`。如果我们的数能被`2`整除，我们的第二个参数将返回`true`。由于所有偶数都可以被 2 整除，所以我们数组输入中的第一个偶数是`8`。

我们做的第一件事是创建一个 for 循环来遍历数组。

```
for (let i = 0; i < arr.length; i++) {
    if (func(arr[i])) {
        return arr[i];
    }
}
```

我们如何将数字输入到一个自变量函数中？您可以像对待任何函数一样对待它。只要函数参数接受参数，就使用相同的函数语法。函数参数的名称是`func`，因此您可以通过参数名称调用函数输入。

我们编写一个条件来检查给定的函数对于数组中的当前数字是否返回 true。如果是，则返回该数字并结束循环。

如果循环到达末尾，这意味着数组中没有一个数字从我们给定的函数返回`true`。我们返回`undefined`。

```
return undefined;
```

代码到此结束。下面是该函数的其余部分:

如果你觉得这个算法有帮助，看看我的其他 JavaScript 算法解决方案:

[](https://medium.com/javascript-in-plain-english/javascript-algorithm-repeat-a-string-repeat-a-string-696dbc73882b) [## JavaScript 算法:重复一个字符串

### 我们编写了一个函数，它将返回一个字符串 x 次，而不使用循环或 repeat 方法。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-repeat-a-string-repeat-a-string-696dbc73882b) [](https://medium.com/@endubueze00/javascript-algorithm-confirm-the-ending-1ea3aa1d6010) [## JavaScript 算法:确认结局

### 我们编写一个函数，如果字符串的末尾包含目标子字符串，它将返回 true 或 false

medium.com](https://medium.com/@endubueze00/javascript-algorithm-confirm-the-ending-1ea3aa1d6010) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-zero-fuel-7e73be4fe91d) [## JavaScript 算法:零燃料

### 我们写了一个函数，它将决定你减少的燃料量是否足以让你到达最近的加油站…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-zero-fuel-7e73be4fe91d) 

【JavaScript 用简单的英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)