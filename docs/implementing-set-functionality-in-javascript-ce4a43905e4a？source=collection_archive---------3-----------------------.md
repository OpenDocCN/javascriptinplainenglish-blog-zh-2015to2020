# 在 JavaScript 中实现 Set 功能

> 原文：<https://javascript.plainenglish.io/implementing-set-functionality-in-javascript-ce4a43905e4a?source=collection_archive---------3----------------------->

学习如何用 JavaScript 实现`union`、`intersection`、`difference`、`symmetricDifference` 。

![](img/ba9bd3714ffb8334c9bd35c4fc685c90.png)

Image from [Javier Quesada](https://unsplash.com/@quesada179?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

`**Set**`对象允许您存储任何类型的唯一值，无论是原始值还是对象引用。

例子

```
var array = [1,2,3,4,4,5];var set = new Set(array);set; // **Set(5) {1, 2, 3, 4, 5} duplicates are removed**
```

此处提供了对 Set 的详细介绍[。](https://levelup.gitconnected.com/set-data-structure-in-javascript-62e65908a0e6)

设置我们将要实现的操作

*   Union →返回一个新的`Set` ，它包含两个集合中的所有元素。
*   交集→返回一个新的`Set`，它在两个集合中都有公共元素。
*   Difference → `Set A` — `Set B`将从`Set A` 返回不在`Set B`中的元素。
*   对称差→返回一个新的集合，该集合由`Set A`的元素和`Set B`的元素创建，前者不在`Set B`中，后者不在`Set A`中。

# 1.联盟

union 方法返回来自`SetA`和`SetB`的所有元素，

```
function union(setA, setB) { **return new Set([...setA, ...setB]);**
}var setA = new Set([1,2,3,4,5]);var setB = new Set([3,4,5,6]);union(setA, setB); Set(6) {1, 2, 3, 4, 5, 6}
```

# 2.交集

`Intersection`操作返回一个新的`Set`，它在两个集合中有共同的元素。我们可以通过以下方式找到

*   循环通过`setA`
*   如果`setA`的元素出现在`setB`中，则推送至新的集合

我们可以在 Set 对象中使用`has`方法检查一个集合是否有元素。

```
function intersect(setA, setB) {
   var commonElements = new Set();

   for (var elem of setB) { if (**setA.has(elem)**) {
            commonElements.add(elem);
        } }

    return commonElements;
}var setA = new Set([1,2,3,4,5]);var setB = new Set([3,4,5,6]);intersect(setA, setB); // Set(3) {3, 4, 5}
```

# 3 .差异

`Set A` — `Set B`将从`Set A` 返回不在`Set B`中的元素，如果我们执行`Set B` — `Set A` ，那么该函数将从`Set B`返回不在`Set A` 中的元素。我们可以通过以下方式实现这一目标

*   使用 Set 对象中的`delete`方法，遍历`Set B`并删除`Set A`中`Set B`的所有元素

```
function difference(setA, setB) { var diff = new Set(setA); **for (var elem of setB) {** **diff.delete(elem);** **}** return diff;
}var setA = new Set([1,2,3,4,5]);var setB = new Set([3,4,5,6]);// SET A - SET B**difference(setA, setB); // Set(3) {1,2}**// SET B- SET Adifference(setB, setA); // Set(3) {6}
```

# 4.对称差

这个方法返回一个新的集合，它是由`Set B`中没有的`Set A`元素和`Set A`中没有的`Set B`元素创建的。我们可以通过以下方式实现这一目标

*   从`Set A`创建一组新的`Result Set`

对于`Set B`的每个元素

*   如果`Result Set`中存在`Set B`元素，则将其移除。
*   如果该元素不存在，则添加到`Result Set`

```
function symmetricDifference(setA, setB) { var resultSet = new Set(setA); for (var elem of setB) { if (**resultSet.has(elem)**) { **resultSet.delete(elem);**
        } else { **resultSet.add(elem);**
        }
    }
    return resultSet;
}var setA = new Set([1,2,3,4,5]);var setB = new Set([3,4,5,6]);symmetricDifference(setA, setB); // Set(3) {1, 2, 6}
```

# 5.发行人

这是一个实用函数，它将检查`Set` 中存在的`subset` 上的所有元素。我们可以通过以下方式实现这一目标

对于子集的每个元素

*   如果`set`中没有任何元素，返回 false。

```
function isSuperset(set, subset) { for (var elem of subset) { if (**!set.has(elem)**) {
            return false;
        } } return true;
}var set = new Set([1,2,3,4]);var subset = new Set([3,4]);isSuperset(set, subset); // truesubset = new Set([3,4,6]);isSuperset(set, subset); // false
```

感谢阅读📖。希望你喜欢这个。如果你发现任何错别字或错误给我一个私人说明📝谢谢🙏 😊。

关注我 [JavaScript Jeep🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----98efbae5e8aa----------------------)。

**请捐款** [**此处**](https://www.paypal.com/paypalme2/jagathishSaravanan) **。你捐款的 80%捐给了需要食物的人🥘。提前感谢。**