# 有用的 JavaScript 技巧——变量和数组

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-variables-and-arrays-75422bb6ec5e?source=collection_archive---------11----------------------->

![](img/3f0f7d243e7d1c2d17c1f7f0388a19fb.png)

Photo by [Hector Argüello Canals](https://unsplash.com/@harguello?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 赋值运算符

我们可以使用复合赋值操作符来简化数值变量的更新。

有`+=`、`-=`、`*=`、`/=`和`%=`操作符，我们可以使用它们将数值变量更新为基于当前值的任意值。

例如，我们可以写:

```
x += 2;
```

然后，我们通过将当前值增加 2 来更新`x`的值。

它与以下内容相同:

```
x = x + 2;
```

同样，我们可以对其余部分做同样的事情。

`-=`是减法。

`*=`是乘法。

`/=`是为师。

而`%=`是基于当前值得到余数。

所以:

```
x %= 2;
```

与以下内容相同:

```
x = x % 2;
```

然后，我们基于`x`的当前值将`x`的余数除以 2，并对其重新赋值。

# 将真值或假值转换为布尔值

我们可以使用`!!`操作符将任何东西转换成布尔值。

例如，我们可以写:

```
!!"" 
!!0 
!!null 
!!undefined 
!!NaN 
```

都是假的，所以都回`false`。

其他操作数将返回`true`。例如，如果我们有:

```
!!"hi"
```

那么它将返回`true`。

# Currying

Curry 是将一个接受多个参数的函数转换成一系列接受单个参数的函数的操作。

这对于创建应用了一些参数的函数并重用它们很有用。

例如，如果我们有一个`add`函数:

```
const add = (x, y) => {
  return x + y;
}
```

然后我们可以写下这个函数:

```
const curriedAdd = (x) => {
  return (y) => {
    return x + y;
  }
}
```

那么我们可以这样称呼它:

```
curriedAdd(1)(2)
```

得到 3。

# 部分应用

我们可以通过编写以下内容将参数部分应用到函数中:

```
const plus10 = (y) => {
  return 10 + y;
}
```

然后我们可以写:

```
plus10(3);
```

然后我们得到 13。

概括起来，我们可以这样写:

```
const partApply = (f, x) => {
  return (y) => {
    return f(x, y);
  }
}
```

然后我们可以通过为`f`传入一个函数和为`x`传入一个值来调用`partApply`。

例如，我们可以写:

```
const plus10 = partApply(add, 10);
```

然后，如果我们调用`plus10(3)`，我们得到 13，因为 10 作为第一个参数被应用。

# 过滤和排序字符串列表

我们可以用`filter`方法对过滤字符串进行排序。

我们可以得到这样的结果，我们返回一个数组，其中的单词长度与它的索引长度相同，写为:

```
const filtered = words
  .filter((word, index) => {
    return word.length > 2
  })
```

所以如果我们有:

```
const words = ['do', 'if', 'foo', 'bar'];
```

然后我们得到:

```
["foo", "bar"]
```

然后我们可以在没有任何回调的情况下调用`sort`来对字符串进行排序，因为这就是`sort`在没有回调的情况下对数组项进行排序的方式。

所以我们可以通过写下来链接它们:

```
const filtered = words
  .filter((word, index) => {
    return word.length > 2
  })
  .sort();
```

然后我们得到`filtered`的`[“bar”, “foo”]`。

# 使用立即调用的函数表达式(IIFEs)

我们可以创建并使用函数来封装函数中的变量，使它们保持私有。

例如，我们可以写:

```
(() => {
  let x = 1;
  //...
})()
```

然后`x`留在函数内部。

这是在函数中创建私有变量的一种方式。

![](img/c6eb13ce2d3226456356d59523a701c7.png)

Photo by [Luiz Eduardo Alves da Silva](https://unsplash.com/@luizduardin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 快速转换数字的方法

为了方便地转换数字，我们可以使用`+`操作符将任何东西转换成数字。

例如，我们可以写:

```
const one = +'1';
```

那么`one`就是数字 1，而不是字符串`'1'`，因为我们对它使用了`+`操作符。

# 打乱数组

我们可以通过使用`sort`方法打乱一个数组，并返回一个可以是正数或负数的随机数，这样我们就可以打乱它们。

例如，我们可以写:

```
const arr = [1, 2, 3];
arr.sort(() => -1 + (Math.random() * 2));
```

`sort`对一个数组进行排序，所以我们得到了它的一个不同的值。

返回`-1 + (Math.random() * 2)`会让回调返回东西-1 和 1。

# 结论

我们可以使用各种方法来过滤和排序数组。

此外，我们可以使用赋值操作符来缩短赋值语句。

## **简单英语的 JavaScript**

你知道我们有四种出版物吗？通过[**plain English . io**](https://plainenglish.io/)找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**