# 缩短 JavaScript 中的条件语句

> 原文：<https://javascript.plainenglish.io/shorten-conditional-statements-in-javascript-aaa377716641?source=collection_archive---------11----------------------->

## 我们可以缩短 JavaScript 中的条件语句

![](img/c44c6e2e52d326692e267f52eb465d11.png)

Photo by [Greg Rakozy](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在使用 JavaScript 时，我每天仍然会写很多条件语句，我敢打赌你也是这样。这里有一些东西可以让你写出更好的条件句。

# 理解真理和谬误

JavaScript 中的每个值都有一个固有的布尔值，通常称为 truthy 或 falsy。

## 福尔西

这里有一个虚假礼貌价值观的快速列表。

*   `false`
*   `null`
*   `0`
*   `NaN`(不是数字)
*   `''`或`""`(空弦)
*   `undefined`

## 真理

其他一切都是真实的。这包括:

*   `'0'`(包含单个零的字符串)
*   `'false'`(包含文本“false”的字符串)
*   `[]`(一个空数组)
*   `{}`(一个空对象)
*   `function(){}`(“清空”功能)

请记住，数组和对象，甚至空数组和对象，总是真的。

# 等价性和同一性

## 等价

一个等价的值，如果它是相同的，但是类型不同，并且用`==`关键字检查。

```
10 == '10' // true
```

一个`empty string`等于`false`(但不相同)。

```
'' == false // true
```

是`falsy`。

```
if (!‘’) {
    return 'I am false.'
}
```

## 身份

相同的值意味着它们必须是相同的类型。

```
10 === '10' // false
10 === 10 // true
```

# 数组和对象

因为数组和对象总是真的，所以我们必须找到另一种方法来检查空性。长度或 Object.keys({})。长度，它会给你一个 0 或者一个很好的真值。

```
if (array && array.length) {
    // not empty
} 
```

# 缩短 JavaScript 中的条件语句

我们需要了解虚假价值和真实价值。它们很重要，因为你可以缩短很多冗长的表达。

比如:我们不需要知道是日期还是证书名。我们所需要知道的是，价值是存在的，并且那里有一些东西。

```
const​ student = {​ 
    name: ​'Bob’​,​ 
    equipmentTraining: ​''​,​ 
}if​(!student.equipmentTraining) {​
    return​ ​'Not authorized to operate machinery'​
}
```

## 无意中创建虚假值是很容易的。

最常见的问题发生在通过检查值的索引来测试数组的存在性时。比如:这里，0 也就是 falsy。

```
[“a”, “b”, “c”].indexOf(“a”)
<< 0 // which is falsy
```

如果使用如下表达式会怎样(这里存在“a”键)。

```
if (![“a”, “b”, “c”].indexOf(“a”)) {
    console.log(“a doesn’t exist”)
}
>> a doesn't exist
```

当您获得未定义或空值时，可能会出现错误值。

## 用三元运算符让你的代码更整洁

我总是试图通过使用三元运算符将表达式减少到尽可能少的字符。

例如，如果`age`大于`16`，那么允许该人驾驶可以编码如下。

```
var age = 19
var canDrive if (age > 16) { 
    canDrive = 'yes'
} 
if (age <= 16) { 
    canDrive = 'no’ 
}
```

但是我们可以用三元运算符作为捷径。代码看起来干净多了。

```
var canDrive = age > 16 ? 'yes' : 'no'
```

## 新变量的一些问题

如果你试图检查一个变量，而你正在使用一个块范围的变量，你将不能访问检查之外的变量。

比如说。

```
if​ (type === ​’bird’​) {​ ​
    const​ move = [​’walk’​, ​’fly’​]
} ​else​ {​ ​
const​ move = [​’walk’​]
}​ 
move // move is not defined​
<< undefined​
```

所以，我们需要做的作业是。

```
let​ move
​if​ (type === ​’bird’​) {​ 
    move = [​’walk’​, ​’fly’​]
} ​else​ {​ 
    move = [​’walk’​]
}
```

我们可以重写前面的代码来使用 const 和一个三元组，如下所示。

```
const​ move = type === ​’bird’​ ? [​’walk’​, ​’fly’​] : [​’walk’​]
```

## 什么是短路？

当 JavaScript 对 OR 表达式求值时，如果第一个操作数为真，JavaScript 会短路，甚至不看第二个操作数。为了清楚地理解什么是短路，让我们考虑下面的三元组，它非常适合。

```
var person = {
  name: 'Jack',
  age: 34
}
console.log(person.job || 'unemployed')
// 'unemployed'
```

这里的目标相当明确。如果一个人有一份工作(在这种情况下，这意味着它是已定义的并且不是空字符串)，那么您希望打印该工作。如果是`falsy`、`undefined`或`‘’`，那么你要用`default`。

很简单，对吧？

感谢阅读😘，再见👋，别忘了👏最多 50 次并跟随！

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****