# JavaScript 中有 7 件奇怪的事情

> 原文：<https://javascript.plainenglish.io/here-are-7-weird-things-in-javascript-42da32e7b50?source=collection_archive---------2----------------------->

## 是[] ==！[] ?

![](img/751faa9879396bca8cdcb573129c4f0e.png)

Photo by [Bruce Warrington](https://unsplash.com/@brucebmax?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/weird?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 是一种伟大而有趣的语言，有着微妙而怪异的东西。所以今天我要用代码例子来回顾这 7 个奇怪的东西。

# `[]`等于`![]`

数组等于非数组:

```
[] == ![]; // -> true
+[] == +![]; // -> true
```

等号运算符将两边转换成数字进行比较，两边因为不同的原因变成数字`0`。

```
[] == ![]; // -> 0 == 0 -> true
```

# `true`不等于`![]`，但也不等于`[]`

数组不等于`true`，但非数组也不等于`true`，数组等于`false`，非数组也等于`false`:

```
true == []; // -> false
true == ![]; // -> false

false == []; // -> true
false == ![]; // -> true
```

等号运算符将两边都转换成数字，所以我们有了`1`和`0`数字:

```
true == ![]; // -> false
Number(true); // -> 1
Number([]); // -> 0
1 == 0; // -> false
```

# 真就是假

```
!!"false" == !!"true"; // -> true
!!"false" === !!"true"; // -> true// true is 'truthy' and represented by value 1 (number), 'true' in string form is NaN.
true == "true"; // -> false
false == "false"; // -> false// 'false' is not the empty string, so it's a truthy value
!!"false"; // -> true
!!"true"; // -> true
```

# 比较`null`和`0`

```
null == 0; // -> false
null > 0; // -> false
null >= 0; // -> true
```

相等运算符将`null`转换为`0`数字。所以`null == 0`和`null > 0`是假的。但是`null >= 0`呢？`null`有一个特殊的规则，它不等于其他任何东西。

# 三个数的比较

```
1 < 2 < 3; // -> true
3 > 2 > 1; // -> false
```

它是如何工作的:

```
1 < 2 < 3; // 1 < 2 -> true
true < 3; // true -> 1
1 < 3; // -> true

3 > 2 > 1; // 3 > 2 -> true
true > 1; // true -> 1
1 > 1; // -> false
```

# `0.1 + 0.2`的精度

```
0.1 + 0.2 == 0.3 // -> false
0.1 + 0.2 // -> 0.30000000000000004
```

【StackOverflow 的回答:

> 程序中的常数`0.2`和`0.3`也将是它们真实值的近似值。碰巧离`0.2`最近的`double`比有理数`0.2`大，而离`0.3`最近的`double`比有理数`0.3`小。`0.1`和`0.2`之和大于有理数`0.3`，因此与代码中的常数不一致。

# 默认行为 Array.prototype.sort()

```
[10, 1, 3].sort() // -> [1, 10, 3]
```

JavaScript 将元素转换成字符串，然后比较它们的 UTF-16 代码序列。我们应该这样排序:

```
[10, 1, 3].sort((a, b) => a - b) // -> [1, 3, 10]
```

# 结论

感谢阅读，希望这篇文章对你有用。编码快乐！

# **用简单英语写的 JavaScript 笔记**

我们推出了三种新的出版物！通过以下方式表达对我们新出版物的热爱:[](https://medium.com/ai-in-plain-english)**[**【UX】**](https://medium.com/ux-in-plain-english)[**【Python】**](https://medium.com/python-in-plain-english)**——谢谢您，继续学习！****

****我们也一直对帮助推广高质量内容感兴趣。如果您有文章想提交给我们的任何出版物，请用您的中用户名在[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**发邮件给我们，我们会将您添加为作者。另外，请告诉我们您想添加到哪个出版物中。******

# ******资源******

******[](https://github.com/denysdovhan/wtfjs) [## denysdovhan/wtfjs

### 有趣且棘手的 JavaScript 示例列表 JavaScript 是一种很棒的语言。它有一个简单的语法，大的生态系统…

github.com](https://github.com/denysdovhan/wtfjs) [](https://xtrp.io/blog/2019/12/15/strange-things-in-js/) [## 只有在 JavaScript 中才会发生的 5 件奇怪而有趣的事情|弗雷德·亚当斯(xtp)-完整堆栈…

### JavaScript 是一种复杂的语言，就其代码如何被解析和运行而言，可能非常令人困惑。这是一个…

xtrp.io](https://xtrp.io/blog/2019/12/15/strange-things-in-js/)******