# JavaScript 分号是可选的吗？

> 原文：<https://javascript.plainenglish.io/are-javascript-semi-colons-optional-71495286f6f4?source=collection_archive---------3----------------------->

## 有时候。一个小分号的奇怪情况

![](img/b3cd0147e8c471a494fc58656239c89c.png)

Photo by Alan Emery on [Unsplash](https://unsplash.com)

在这篇文章中，我将快速演示省略分号会导致问题。

> 自动分号插入
> 
> **JavaScript** 解析器将**自动**添加一个**分号**当在源代码解析过程中，它发现这些特殊情况时:当下一行以中断当前行的代码开始时。

## 让我们打破它！

首先，**这段代码运行良好。**

```
// No semi-colonslet x=1function addOne(){
      x++
}
addOne()
console.log(x)  //Output 2
```

现在。让我们把函数 **addOne()** 写成一个[life，](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) *立即被调用的函数表达式。*

**连编译都不会！**

```
// No semi-colons
let x=1(function addOne(){
    x++
})()console.log(x)
```

**修正版。一个简单的分号**

```
let x=1; // Note the semi-colon(function addOne(){
    x++
})()console.log(x)  //Output 2
```

## 刚刚发生了什么？

谁能在不查看调试器消息的情况下发现问题？

解析器会将生命周围的左括号“(”，视为上面一行的函数的左括号。不管那条高于生活的线是什么。

```
(function addOne(){ 
^  TypeError: 1 is not a function
```

## 结论

如果你选择不使用分号，就要小心了。

感谢阅读。快乐编码。