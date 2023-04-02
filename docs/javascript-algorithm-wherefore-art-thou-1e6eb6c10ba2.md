# JavaScript 算法:你为什么存在

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-wherefore-art-thou-1e6eb6c10ba2?source=collection_archive---------7----------------------->

## 我们编写了一个函数，该函数返回一个数组中的所有对象，该数组包含来自源对象的所有名称/值对。

![](img/dd604d051acf6fa639f0e6a557f80b93.png)

我们将编写一个名为`whatIsInAName`的函数，它接受一个数组(`collection`)和一个对象(`source`)。

你会得到一个对象数组。该函数的目标是查看对象的`collection`数组，并返回具有匹配名称和值对的所有对象的数组作为`source`对象。

**从`collection`开始，对象中必须存在`source`对象的每个**名称和值对。

示例:

```
let collection = [
{ "apple": 1, "bat": 2 }, 
{ "bat": 2 }, 
{ "apple": 1, "bat": 2, "cookie": 2 }
];let source = { "apple": 1, "bat": 2 };
```

如果我们看看我们的例子，唯一包含来自`source`的所有名称和值对的对象是第一个和第三个对象。该函数将这两个对象放入一个数组并返回:

`[{ “apple”: 1, “bat”: 2 }, { “apple”: 1, “bat”: 2, “cookie”: 2 }]`

让我们编写函数。

首先，我们创建三个变量:

```
let arr = [];
let hasAllEntries = 0;
const sourceObject = Object.entries(source);
```

`arr`变量是用于保存`collection`数组中所有对象的变量，该数组中所有匹配的名称-值对都作为`source`。

对于下一个变量，我费了好大劲才找到方法来确保`collection`对象中的每个对象都有**匹配`source`对象中的每个名称-值对。《财富》美国 500 强排名第 14 位的公司将继续关注此事。**

`sourceObject`变量使用`Object`对象将对象转换为数组。[这使得迭代变得容易。](https://zellwk.com/blog/looping-through-js-objects/)

`Object.entries`创建数组的数组。每个子阵列有两个项目。第一个是属性，第二个是值。此变量构成了`source`对象参数的数组。

接下来，我们循环通过`collection`数组。然后我们添加一个额外的嵌套 for 循环，以循环通过`sourceObject`数组。

对于我们在`collection`数组中循环通过的每个对象，我们也循环通过`sourceObject`数组中的每个键值对。

当遍历`sourceObject`时，由于我们处理的是一个二维数组，我们将数组析构为它的键(`sourceKey`)和值(`sourceValue`)。

在我们的条件中，如果`collection`数组中的对象包含与`sourceKey`匹配的属性，并且如果该对象属性的值与`sourceValue`匹配，我们就增加`hasAllEntries`。

如果您阅读过前面的内容，那么为我们从`collection`中的每个对象获得的每个名称-值对计数`hasAllEntries`变量是跟踪和查看该对象是否拥有来自`source`对象的所有名称-值对的唯一方法。

许多其他解决方案中，我只在`arr`数组中填充了只有*一个*名称-值对匹配的对象。该函数应该返回匹配每一对的对象。

为了回到函数，我们写了一个条件。根据`sourceObject`的长度，如果`hasAllEntries`等于`sourceObject`数组的长度，这意味着来自`collection`的特定对象拥有来自`source`的每个名称-值对。如果是这种情况，我们将该对象推入`arr`数组。

在我们进入下一个对象之前，我们将`hasAllEntries`重置回 0。

```
if (hasAllEntries === sourceObject.length) {
    arr.push(object);
}hasAllEntries = 0;
```

循环完成后，我们返回`arr`数组。

```
return arr;
```

我们的代码到此结束。它并不完美，但下面是函数的其余部分:

如果你觉得这个算法有帮助，看看我的其他 JavaScript 算法解决方案:

[](https://codeburst.io/javascript-algorithm-return-largest-numbers-in-arrays-476914b717a7) [## JavaScript 算法:返回数组中最大的数字

### 我们编写了一个函数，从数组参数中返回一个数组，该数组包含每个子数组中最大的数字

codeburst.io](https://codeburst.io/javascript-algorithm-return-largest-numbers-in-arrays-476914b717a7) [](https://levelup.gitconnected.com/javascript-algorithm-remove-the-time-de34cfb909cd) [## JavaScript 算法:去掉时间

### 我们将编写一个函数，输出以某种格式编写的时间戳。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-remove-the-time-de34cfb909cd) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-minimum-distances-65dfde1ca83f) [## JavaScript 算法:最小距离

### 对于今天的算法，我们要写一个名为 minimumDistances 的函数，它将接受一个数组 a 作为…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-minimum-distances-65dfde1ca83f) 

```
hasAllEntries = 0;
}
```

## **用简单的英语写的 JavaScript 的注释:**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**