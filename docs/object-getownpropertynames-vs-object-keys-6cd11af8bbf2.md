# Object.getOwnPropertyNames()与 Object.keys()

> 原文：<https://javascript.plainenglish.io/object-getownpropertynames-vs-object-keys-6cd11af8bbf2?source=collection_archive---------2----------------------->

![](img/0bf84e50e2cf73f3f220de98e71ca39c.png)

## **object . getownpropertymanames()快速介绍**

`Object.getOwnPropertyNames()`方法返回在给定对象中直接找到的所有属性(包括不可枚举的属性)的数组

**语法**

```
Object.getOwnPropertyNames(obj)
```

**示例**

`Object.getOwnPropertyNames()`返回对象的所有键

```
[ '0', '1', '2' ]
string
```

## Object.keys()快速介绍

`Object.keys()`方法返回一个给定对象自己的可枚举属性**名称**的数组，迭代顺序与普通循环相同

**语法**

```
Object.keys(obj)
```

**例子**

`Object.keys`返回与`Object.getOwnPropertyNames()`相同的输出，因为`obj`中的每个元素都是可枚举的

```
[ '0', '1', '2' ]
string
```

## 最后，Object.getOwnPropertyNames()与 Object.keys()

`Object.getOwnPropertyNames(obj)`返回*对象的所有属性*。`Object.keys(obj)`返回*所有可枚举属性。除非您将`enumerable: false`设置为任何属性，否则它们会提供相同的结果*

让我们再定义一个名为`resolution`的属性，但它被设置为`enumerable: false`

`resolution`属性则不会出现在`Object.keys`列表中

## 结论

```
key: {enumerable: false | true,},
```

`Object.getOwnPropertyNames(obj)`

→所有属性(包括不可枚举的属性)

`Object.keys(obj)`

→仅可枚举的属性

![](img/697ee8277ef3908f9839ae615a67b7a6.png)

Photo by [Lauren York](https://unsplash.com/@lauren_317?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)