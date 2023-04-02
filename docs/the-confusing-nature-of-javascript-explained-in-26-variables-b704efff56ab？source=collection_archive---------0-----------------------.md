# 你知道 JavaScript 吗？

> 原文：<https://javascript.plainenglish.io/the-confusing-nature-of-javascript-explained-in-26-variables-b704efff56ab?source=collection_archive---------0----------------------->

![](img/fed520de57fca873881e0d28fa67b76a.png)

## 我认为有兴趣知道的一系列事情

## **1。Null 具有“对象”类型**

```
const val = null;typeof val; 
// ‘object’
```

## 2.数组的类型为“object”

```
const arr = [];typeof arr; 
// ‘object’
```

## 3.您可以直接在字符串上执行函数。

```
const str = "Sunil".toUpperCase();console.log(str) 
// 'SUNIL'
```

## 4.当与三重相等运算符比较时，相等的原始值将始终返回 true

```
const num1 = 5
const num2 = 5num1 === num2
// true
```

## 5.即使你这样设置…

```
const sun1 = "sun" + "il";
const sun2 = "su" + "n" + "il";sun1 === sun2;
 // true
```

## 6.这适用于所有原始值，除了 NaN

```
const momma = NaN;
const poppa = NaN;momma === poppa;
 // false
```

## 7.非基元值(如数组和对象)不会产生不完全相等的值

```
const x = { name: "Sunil Sandhu" };
const y = { name: "Sunil Sandhu" };x === y; 
// false
```

## 8.但是，如果你比较里面的属性…

```
const x = { name: "Sunil Sandhu" };
const y = { name: "Sunil Sandhu" };x.name === y.name; 
// true
```

## 9.函数不能改变原语

```
const func = (number) => {
  number = number + 1;
}let val = 1;func(val);val; 
// 1
```

## 10.但是它们可以改变非原语内部的属性

```
const func = (number) => {
  number.value = number.value + 1;
}let newNumber = {value: 1};func(newNumber);newNumber; 
// {value: 2}
```

## 11.那 5 + 5 = 10，但 5+ 5 + [] = "10 "

这是因为对数组使用`+`会触发字符串连接，因为数组没有继承数值。因此，`[].toString`被隐式调用。`Array.prototype.toString`自动调用`Array.prototype.join`将所有的数组值合并成一个字符串。`[].join()`会加入一个空数组，所以结果是`""`，所以`[].toString()`也是`""`。

`5 + ""`会产生字符串`"5"`因为一个空字符串被加到一个数字上，所以这个数字也被转换成一个字符串。

## 12.更糟糕的是…

`[] + 3 + 3 === “33”`

# 我们做到了！

一些我认为值得一提的东西。当我遇到更多有趣的东西时，我可能会把它作为一个中心，所以一定要把它收藏起来，并不时地回来查看。同样，如果你注意到 JavaScript 中有什么有趣的东西，一定要在评论中留言，也许我们会把你的添加进去！

**来源**

11、12: [*来源*](https://www.reddit.com/r/learnjavascript/comments/bsgapc/why_does_3_3_6_and_not_6/?utm_medium=android_app&utm_source=share)