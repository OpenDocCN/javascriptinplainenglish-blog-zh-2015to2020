# JavaScript 算法:对一个范围内的所有数字求和

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-sum-all-numbers-in-a-range-49f8d37a3fec?source=collection_archive---------2----------------------->

## 我们编写一个函数，返回两个整数范围内所有数字的和。

![](img/ca36cde2207f4dd25f5e3cce465c4aba.png)

我们将编写一个名为`sumAll`的函数，它接受一个数组(`arr`)作为参数。

给你一个包含两个整数的数组。该函数的目标是返回这两个数字之间(包括这两个数字)所有数字的总和。

示例:

```
let arr = [1, 4];// output: 1 + 2 + 3 + 4 = 10
```

对于我们的例子，如果我们把从`1`到`4`的每个数字相加，总和将是`10`。

我们来写函数吧。

我们首先创建三个变量:

```
let fullArr = [];
let sum = 0;
const reducer = (accumulator, currentValue) => accumulator + currentValue;
```

`fullArr`变量将在一个数组中保存从`arr[0]`到`arr[1]`范围内的所有数字。

`sum`变量将保存来自`fullArr`的所有数字的总和。

我们将通过使用`reduce()`方法得到`sum`。`reducer`函数是 reduce 方法将调用的函数，用于对来自`fullArr`的所有数字求和。

接下来，我们将对数组进行排序。即使我们的数组输入只有两个数字，它也不会总是从最小到最大排序。我们对数组进行排序以防万一。

```
arr.sort(function(a,b){return a-b});
```

接下来，我们将使用 for 循环。这将获取从`arr[0]`到`arr[1]`的所有数字，并将它们推入我们的`fullArr`数组。

```
for (let i = arr[0]; i <= arr[1]; i++) {
    fullArr.push(i);
}
```

接下来，我们使用带有`reducer`函数的`reduce()`方法对`fullArr`数组中的所有数字求和。我们将总和赋给我们的`sum`变量。

```
sum = fullArr.reduce(reducer);
```

最后，我们返回`sum`。

```
return sum;
```

下面是该函数的其余部分:

如果您发现这个算法很有帮助，请查看我的其他 JavaScript 算法解决方案文章:

[](https://medium.com/@endubueze00/javascript-algorithm-chunky-monkey-337991491b24) [## JavaScript 算法:矮胖猴子

### 我们编写一个函数，通过将一个一维数组分割成子数组组来创建一个二维数组…

medium.com](https://medium.com/@endubueze00/javascript-algorithm-chunky-monkey-337991491b24) [](https://medium.com/@endubueze00/javascript-algorithm-splice-and-flatten-52bb0f192d59) [## JavaScript 算法:拼接和展平

### 我们编写一个函数，从给定的索引开始，将一个数组中的每个元素插入到另一个数组中。

medium.com](https://medium.com/@endubueze00/javascript-algorithm-splice-and-flatten-52bb0f192d59) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-falsy-bouncer-4fc92250dc65) [## JavaScript 算法:Falsy Bouncer

### 我们写了一个函数，从一个数组中删除所有的假值。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-falsy-bouncer-4fc92250dc65) 

## **用简单英语写的 JavaScript 的注释:**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**