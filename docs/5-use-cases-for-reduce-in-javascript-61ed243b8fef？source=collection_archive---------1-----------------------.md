# JavaScript 中 reduce()的 5 个用例

> 原文：<https://javascript.plainenglish.io/5-use-cases-for-reduce-in-javascript-61ed243b8fef?source=collection_archive---------1----------------------->

## JavaScript 数组 reduce()及其用例简介。

![](img/cb9aa29a90db763ce3d25ebc249fdb63.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> *reduce()方法对数组的每个元素执行一个 reduce 函数(由您提供)，得到一个输出值。* [*源*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

`reduce()`方法将数组中的所有元素简化为一个输出值。输出值可以是数字、对象或字符串。`reduce()`方法有两个论点。第一个是**回调函数**，第二个是**初始值**。

## **回调函数**

回调函数在数组的每个元素上执行。回调函数的返回值是累积的结果，并在回调函数的下一次调用中作为参数提供。回调函数接受四个参数。

`***Accumulator***`
累加器累加回调函数的返回值。

`***Current Value***`
处理数组的当前元素。

`***Current Index***`处理数组当前元素的索引。

`***Source Array***`
召唤阵 `reduce()`。

`**Current Index**` 和`**Source Array**` 可选。

## **初始值**

如果指定了初始值，则累加器被设置为初始元素`initialValue`。否则，累加器被设置为数组的第一个元素作为初始元素。

```
arr.reduce(callback(accumulator, currentValue[,index[,array]])[, initialValue])
```

在下面的代码片段中，首先`accumulator`被赋予初始值 0。`currentValue`是正在处理的`numbersArr`数组的元素。在此，在`accumulator`上增加`currentValue`。返回值在回调函数的下一次调用中作为参数提供。

Figure 1\. (numbersArr array reduce to single value total)

输出:

```
accumulator is 0 current value is 67
accumulator is 67 current value is 90
accumulator is 157 current value is 100
accumulator is 257 current value is 37
accumulator is 294 current value is 60
total : 354
```

# **JavaScript 用例减少**

## **1 .将一个数组的所有值相加**

在下面的代码中，`studentResult` 数组有 5 个数字。使用`reduce()`方法将数组简化为一个值，这个值是`studentResult`数组所有值的和，结果被分配给`total`。

Figure 2\. (sum all the values of the studentResult array)

输出:

```
354
```

## **2。对象数组中值的总和**

通常，我们从后端获取数据作为对象数组。因此，`reduce()`方法有助于管理我们的前端逻辑。在下面的代码中，`studentResult` object 数组有三个主题，它们的标记是 object。这里，`currentValue.marks`取`studentResult`对象数组中每个主体的标记。

Figure 3\. (sum of the marks in studentResult object array)

输出:

```
251
```

## **3。展平数组的数组**

“展平数组”是指将多维数组转换为一维数组。在下面的代码中，`twoDArr` 2D 数组被转换为`oneDArr`一维数组。这里，第一个[1，2]数组被分配给`accumulator`，然后`twoDArr` 数组的其余元素被连接到`accumulator`。

Figure 4\. (flatten the twoDArr array)

输出:

```
[ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]
```

## **4。按属性对对象进行分组**

基于对象的属性，我们可以使用`reduce()`方法将对象数组分成几组。您可以通过下面的代码片段清楚地理解这个想法。这里，`result`对象数组有五个对象，每个对象都有`subject`和`marks`属性。如果分数大于或等于 50，则该科目通过。否则该科目不及格。`reduce()`用于将结果分为通过和失败。首先将`initialValue`赋值给累加器，然后`push()`方法在检查条件后将`current`对象添加到`pass`和`fail`属性中作为对象数组。

Figure 5\. (grouping objects by property)

输出:

```
{
 pass: [
  { subject: ‘Chemistry’, marks: 59 },
  { subject: ‘Applied Maths’, marks: 90 },
  { subject: ‘English’, marks: 64 }
 ],
 fail: [
  { subject: ‘Physics’, marks: 41 },
  { subject: ‘Pure Maths’, marks: 36 }
 ]
}
```

## **5。移除数组中的重复项**

在下面的代码片段中，删除了`duplicatedArr`数组中的重复项。首先，给`accumulator`分配一个空数组作为初始值。`accumulator.includes()`检查`duplicatedArr`阵列的每个元素是否已经在`accumulator`中可用。如果`currentValue`在`accumulator`中不可用，则使用`push()`添加。

Figure 6\. (Remove duplicates in the duplicatedArr array)

输出:

```
[ 1, 5, 6, 7, 8, 9 ]
```

## **结论**

在本文中，我们讨论了数组约简()方法。首先，介绍了`reduce()`法。然后，用一个简单的例子讨论了它的行为。最后，结合实例讨论了`reduce()`方法最常见的五种用例。如果您是 JavaScript 的初学者，本文将对您有所帮助。

## **参考**

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) [## Array.prototype.reduce()

### reduce()方法在数组的每个元素上执行一个 reduce 函数(您提供的),得到单个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)