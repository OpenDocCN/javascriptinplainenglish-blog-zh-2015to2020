# 神奇的 JavaScript 行为 I

> 原文：<https://javascript.plainenglish.io/magical-javascript-behaviors-4b5c4d151663?source=collection_archive---------4----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/16978ba3bcbfe2660846969768791a93.png)

A man throwing dice

# 第一部分

在我想写的关于这个主题的第一篇文章中，我收集了一系列 JavaScript 特有的行为。我用一些简单的例子来解释它们，并展示如何避免它们，因为它们会导致在我们的代码处于生产阶段时很难检测到的错误。

# 案例

*   真等于假
*   最小值大于零
*   南不是南
*   神奇地求和
*   数字的比较
*   Try-catch-finally 上的意外返回值
*   箭头功能
*   神秘回归
*   关系运算符为空
*   蝙蝠侠！

# 真等于假

```
!!"false"==!!"true";  //true
!!"false"===!!"true"; //true
```

解释:

```
//true is 'truthy' and represented 
//by the number 1,
//'true' in string form is NaN.
//false (1 == NaN)
true == "true";   
false == "false"; 
//false//'false' is not the 
//empty string,
//so it's a truthy value
!!"false"; //true
!!"true";  //true
```

## 最小值大于零

号码。MIN_VALUE 是最小的数字，在 JavaScript 中似乎大于零:

```
Number.MIN_VALUE > 0; 
//true
```

解释:

号码。MIN_VALUE 是 5e-324，即在 float precision 范围内可以表示的最小正数，也就是说，这是最接近零的数字。它定义了浮点数给你的最佳分辨率。

# 南不是南

第一，南是什么？

“NaN”是“Not a Number”的首字母缩写，当我们试图将一个数字转换为一种非数值类型的数据时，就会出现此错误，因此会引发异常“NaN”。IEEE 754 浮点标准在 1985 年引入了 NaN 的使用。

```
NaN === NaN; 
//false
NaN == NaN;  
//false
```

解释:

相关的 IEEE 标准定义了一个数值常数 NaN(不是一个数字)。它规定 NaN 应该不等于自身进行比较，以避免由于当时计算机的十进制精度而导致的错误。

如果想知道某个东西是不是 NaN，可以使用 ES6 feature Object.is()。运行 Object.is(NaN，NaN)返回 true。

# 神奇地求和

```
9999999999999999;       
//10000000000000000 wtf!
10000000000000000 + 1;   
//10000000000000000 wtf!
10000000000000000 + 1.1;
//10000000000000002 wtf!
10000000000000000 + 10; 
//10000000000000010
```

解释:

这些事情是由 IEEE 754–2008 浮点运算标准引起的。它舍入到最接近的偶数。

```
10  - 2 
 //8
 10  + 2 
 //12
'10' -2  
 //8
'10' +2  
 //102
'10' -'2'
 //8
'10' +'2'
//102
```

解释:

数字-数字:减法
数字+数字:加法

数字-字符串:减法
数字+字符串:串联

字符串-字符串:减法
字符串+字符串:串联

# 数字的比较

```
1 < 2 < 3; 
//true
3 > 2 > 1; 
//false
```

解释:

问题出在表达式的第一部分。您可以使用大于或等于运算符来修复它。

```
1 < 2 < 3; 
//(1 < 2) -> true -> 1
true (1) < 3;
1 < 3;     
// true3 > 2 > 1; 
//(3 > 2) -> true -> 1
true > 1; 
1 > 1; 
// -> false3 > 2 >= 1; 
// (3 > 2) -> true -> 1
true > 1; 
1 >= 1; // -> true
```

# Try-catch-finally 上的意外返回值

下一个 IIEF(立即调用的函数表达式)的结果是什么？

返回值为假。这是因为“Finally 块”总是会执行。

```
(() => {
 try {
   return true;
 } finally {
   return false;
 }
})();
//false
```

您可以像下一个示例那样修复它:

```
(() => {
 let result = false;
 try {
   result =  true;
 } finally {
   return result;
 }
})();
//true
```

# 箭头功能

考虑下面的例子:

```
let foo =() => {console.log("Hello!)};
foo();
//Hellolet foo2= () => {};
foo2(); 
//undefined
```

解释:

您获得 undefined，因为花括号是箭头函数语法的一部分。

如果将返回值用括号括起来，可以解决这个问题并获得{}对象，如下例所示:

```
let foo3 = () => ({});
foo3();
//{}
```

## 神秘回归

考虑下面的 IIEF 函数

```
(function() {
  return
  {
    greeting: "Hello";
  }
})();
//undefined
```

解释:

这个返回是因为自动分号插入，它自动在大多数换行符后插入分号。

结果函数类似于:

```
(function() {
  return**;//!!**
  {
    greeting: "Hello";
  }
})();//undefined
```

您可以像下面的例子一样解决这个问题:

```
(function() {
  return { greeting: "Hello" }
})();
//{greeting: "Hello"}
```

# 关系运算符为空

接下来的比较行为很奇怪。根据这个例子，最后一个结果表明“null >=0”，所以在上面的一个比较中，它必须为真。

```
null > 0
//false
null == 0
//false
null >= 0 
//true
```

解释:

原因是比较将 null 转换为数字 0。这就是为什么 null >= 0 为真，null > 0 为假。但是 null 和 undefined 的等式 check ==是在没有任何转换的情况下定义的。这就是为什么 null== 0 是假的。

# 蝙蝠侠！

为了完成这个 JavaScript“神奇事物”的小列表，我们现在要做一些有趣的事情。

```
Array(16).join('robin' — 1) + ' Batman!';
//"NaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaN Batman!"
```

解释:

NaN 是计算' robin'-1 的结果(它是 NaN，因为你不能从一个字符串中减去 1)。NaN 被转换成一个字符串并连接 16 次，然后在末尾追加“Batman”。

# 结论

有了 JavaScript，很明显你永远不会感到无聊，但是你必须明白为什么会出现这些情况，以避免将来的编码错误。我的想法是写更多关于这些“特殊”案例的文章，在那之前，我希望你喜欢这篇文章，你可以在[神奇的 JavaScript 行为 II](https://medium.com/@kesk/magical-javascript-behaviors-ii-8ca9beff77b4) 中继续看更多的例子

# 参考

这些例子大部分都是受 Brian Leroux 在 2012 年的[演讲](https://www.youtube.com/watch?v=et8xNAc2ic8)的启发。

[摄影爱好者的照片](https://www.pexels.com/es-es/@fotografierende?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)。