# 神奇的 JavaScript 行为 II

> 原文：<https://javascript.plainenglish.io/magical-javascript-behaviors-ii-8ca9beff77b4?source=collection_archive---------11----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/43f4a88882615da9aacf7aeea12e9df4.png)

A man throwing dice

# 第二部分

JavaScript 有很多奇怪的地方。在“神奇的 JavScript 行为”的第二部分中，我将分享我最喜欢的列表。如果你想要更多，你可以在“[神奇的 JavaScript 行为 I](https://medium.com/javascript-in-plain-english/magical-javascript-behaviors-4b5c4d151663) ”中找到更多的例子
为了让这篇文章更容易理解，我把重点放在看起来最容易理解的案例上。

# 案例

*   香蕉
*   真实但不真实
*   添加数组
*   未定义和编号
*   未定义的可以定义
*   未定义始终是未定义的
*   词典排序
*   null 是一个对象，但 undefined 是未定义的

# 香蕉

```
'b' + 'a' + **+ 'a'** + 'a'; 
//'BaNaNa'('b' + 'a' + + 'a' + 'a').toLowerCase()
//banana
```

解释:

表达式的计算结果为' ba' + ( **+'a'** )，将+'a '转换为非数字。然后表达式 NaN+'a '求值为 NaN **a** ，最后结果是香蕉

# 真实但不真实

注意:的！！运算符将对象转换为布尔值。

Null 是一个 false 值，但它不等于 false。

```
!!null; 
//false
null == false; 
//false
```

解释:

在比较相等性之前，不能将 null(和 undefined)转换为任何其他值，因此 null == false 等于 false。

一个数组和一个空数组总是真值，但不等于真值。

```
!![]       
//true
[] == true 
//false
```

解释:

True 转换为 1，[]转换为 0，因此[] ==true (0 == 1)等于 false。

# 添加数组

```
[9, 8, 7] + [6, 5, 4] 
//"9,8,**76**,5,4"
```

出现这种结果是因为当您对两个集合求和时，在每个数组中都调用了“toString”，因此[9，8，7]被转换为“9，8，7”，而[6，5，4]被转换为“6，5，4”，然后发生串联。

# 未定义和编号

如果不向 Number 构造函数传递任何参数，则得到 0。当没有参数时，你得到 undefined，当有空值时，你得到 0。但是对于 undefined，你得到的是 NaN。

```
Number(true)
//1
Number(false)
//0
Number(null)
//0Number()
//0
Number(undefined)
//NaN
```

解释:

如果没有参数传递给函数调用，则得到 0。
否则，你得到 ToNumber(值)。在未定义的情况下，ToNumber(未定义)返回 NaN。

# 未定义始终是未定义的

在下一个例子中，没有什么奇怪的事情发生:

```
let team;
console.log(team == undefined); 
//true
```

但是这里发生了什么？

```
undefined = "A Team";
let team;
console.log(team == undefined); 
//true
console.log(undefined);
//undefined
```

解释:

Undefined 是 JavaScript 中的保留字，不能给它赋值。

# 词典排序

可以用 sort 方法对数组的元素进行排序。

```
[’z’, 'a’, 'b’].sort()
//[ "a", "b", "z" ]
```

这里发生了什么？

```
[1,2,11].sort()
//[ 1, 11, 2 ]
```

解释:

在 JavaScript 中，默认情况下，数值是按字典顺序排序的，这可能会产生奇怪的结果。

您可以使用比较函数来覆盖此行为:

```
[1,2,11].sort( (a, b) => a - b )
//[ 1, 2, 11]
```

# null 是一个对象，但 undefined 是未定义的

JavaScript 既有 null 也有 undefined，虽然它们看起来很相似，但却有很大的不同。

```
typeof(undefined)
"undefined"typeof(null)
"object"
```

说明

这种行为是 JavaScript 第一次实现的副作用。值表示为类型标记加上值。但是，null 是用一个空指针提供的。对象的类型标记为 0。因此，null 也被解释为一个对象。

# 结论

虽然这些案例反映了有趣的行为，但如果你想深入理解这种语言并避免在开发中出错，理解它们是至关重要的。

我希望你喜欢这篇文章。你有更多的例子在 [**神奇的 JavaScript 行为我**](https://medium.com/javascript-in-plain-english/magical-javascript-behaviors-4b5c4d151663) **。**

# 参考

这些例子中的大部分都受到了 Brian Leroux 在 2012 年的演讲[的启发。](https://www.youtube.com/watch?v=et8xNAc2ic8)

[摄影爱好者的照片](https://www.pexels.com/es-es/@fotografierende?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)。