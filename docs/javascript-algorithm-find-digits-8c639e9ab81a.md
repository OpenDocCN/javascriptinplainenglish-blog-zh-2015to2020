# JavaScript 算法:查找数字

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-find-digits-8c639e9ab81a?source=collection_archive---------3----------------------->

![](img/723f6f7983bc62ed64bd716989b56f45.png)

对于今天的算法，我们将编写一个名为`findDigits`的函数，它将接受一个整数`n`作为输入。

在 JavaScript 中，当找到一个数是另一个数的除数时，使用模运算符。模运算符返回除法余数。如果某物能被整除，那么余数将是 0。给你一个整数`n`，这个函数的目标是取这个整数中的每个数字，并确定这个数字是否是`n`的除数。返回`n`的除数。我们来举个例子。

```
let n = 426;
```

我们的变量`n`是 426，组成数字的位数是 **4** 、 **2** 和 **6** 。

让我们用 426 除以这些数字，看看哪个数字是 426 的除数。

```
426 / 4 = 106.5 // not a whole number, therefore not a divisor
426 / 2 = 213
426 / 6 = 71
```

我们看到两个数字， **2** 和 **6** 是 426 的约数，因此，该函数将输出 2。

让我们把这变成代码。我们创造变量:

```
let digitArray = (n + "").split('');
let divisors = [];
```

我们的`digitArray`变量携带我们的输入，并将其分割成单个数字，然后放入一个数组中。我们首先将数字连接成一个空字符串，从而将它转换成一个字符串。然后我们使用`split()`方法，同时使用`''`作为分隔符，这样我们就可以将字符串分割成单独的字符。

我们的`divisors`变量将携带一个数组中所有的`n`的约数。它目前是空的。

现在我们遍历我们的`digitArray`数组:

```
for(let i = 0; i < digitArray.length; i++){
    let num = digitArray[i]*1;

    if(n % num == 0){
      divisors.push(num)
    }
  }
```

数字数组中的字符是字符串，所以我们通过将它们乘以 1 来快速将它们转换成整数。我们将该值赋给 num 变量，以便在 if 语句条件中使用。

我们检查单个数字是否是`n`的除数。我们使用模运算符。如果余数是 0，那么它是一个除数。我们将该数字放入我们的`divisors`数组中。

最后，我们通过返回`divisors`数组的长度来返回`n`中作为`n`的除数的位数。

```
return divisors.length;
```

以下是完整的代码:

```
function findDigits(n) {
  let digitArray = (n + "").split('');
  let divisors = [];

  for(let i = 0; i < digitArray.length; i++){
    let num = digitArray[i]*1;

    if(n % num == 0){
      divisors.push(num)
    }
  }

  return divisors.length;
}
```

如果您觉得这个算法有帮助或有用，请查看我的其他 JavaScript 算法解决方案文章:

[](https://medium.com/javascript-in-plain-english/javascript-algorithm-grading-students-210a89c5496f) [## JavaScript 算法:给学生评分

### 对于今天的算法，我们将编写一个名为 gradingStudents 的函数，在这个函数中，我们将…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-grading-students-210a89c5496f) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-bon-app%C3%A9tit-209f59a85715) [## JavaScript 算法:Bon Appé tit

### 对于今天的算法，我们将编写一个名为 bonAppetit 的函数，这个函数将接受三个输入…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-bon-app%C3%A9tit-209f59a85715) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85) [## JavaScript 算法:电子商店

### 对于今天的算法，我们将编写一个名为 getMoneySpent 的函数，它将接受三个输入:两个数组…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85)