# 如何在 JavaScript 中检测对象的属性

> 原文：<https://javascript.plainenglish.io/how-to-detect-properties-of-an-object-in-javascript-754cae0dbe97?source=collection_archive---------5----------------------->

## 新的开发者经常错误地检测一个属性是否存在

![](img/b74544c5800042d54cfeddbca219d369.png)

Photo by [Max Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

参考[我之前的帖子](https://medium.com/javascript-in-plain-english/the-3-ways-to-create-a-javascript-object-33406794e9e5)，因为属性和方法可以随时添加，所以有时候需要检查对象中是否存在属性或方法。

## 错误的方式

有些开发商就像下面的例子一样去检查一个楼盘是否存在，这是一种不可靠的方式。

```
// check if age property exists
if (person.age) {
    // do something with age
}
```

这种方法的问题是我们不知道类型如何影响结果，因为“if 条件”可能计算为 true、false、truthy(对象、非空字符串、非零数字或 true)、false (null、未定义、0、false、NaN 或空字符串)。

因此，对象属性可能是这些假值中的一个，“if 条件”可能会产生假阴性。在这种情况下，如果(person.age)为 0，即使该属性存在，也不会满足 If 条件。

```
person.age = 0// There is nothing done, always falseif (person.age) {
    // do something with age}
```

## 更可靠的方法

我们可以使用 in 操作符来测试一个属性的存在。in 运算符在特定对象中查找具有给定名称的属性，如果找到，则返回 true。

> 请记住，in 操作符检查自己的属性和原型属性。

例如，当它用于检查 person 对象中的某些属性时，会发生以下情况。

*   查看*姓名*属性

```
console.log("name" in person)
// << true
```

*   检查*年龄*属性(在本例中，*年龄*属性不存在)

```
console.log (“age” in person) // << false
```

## 其他方式

在大多数情况下，In 运算符是最好的方法。但在某些情况下，您可能希望仅在某个属性是自己的属性时才检查该属性是否存在。

使用 *hasOwnProperty()* 方法，它返回一个布尔值，表明对象是否将指定的属性作为自己的属性。

```
console.log(person.hasOwnProperty("name"))
<< true
```

通过 toString()方法，是所有对象上都存在的原型属性。

*   in 运算符返回 true

```
console.log(“toString” in person)<< true
```

*   hasOwnProperty()方法返回 false

```
console.log(person.hasOwnProperty(“toString”))<< false
```

## 如何在 JavaScript 中测试一个方法的存在？

这太容易了！请记住，方法只是引用函数的属性，所以您可以用同样的方式来做。比如看到 go 方法存在，输出为真。

> 请记住，方法只是属性。

```
console.log("go" in person)
// true
```

它是关于如何在 JavaScript 中检测一个对象的属性。感谢阅读！

不要忘记👏跟着走。