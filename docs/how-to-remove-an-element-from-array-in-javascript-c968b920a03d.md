# 如何在 JavaScript 中从数组中移除元素

> 原文：<https://javascript.plainenglish.io/how-to-remove-an-element-from-array-in-javascript-c968b920a03d?source=collection_archive---------0----------------------->

![](img/4fd9b9e42b7c8a0ddcec3abd0aa7192e.png)

T 在过去的某个时候，你很有可能在谷歌或 Stack Overflow 搜索栏中输入过上述短语。这个问题太普遍了，以至于成为 [**栈**](https://stackoverflow.com/questions/5767325/how-can-i-remove-a-specific-item-from-an-array) 上 *javascript* 标签下**投票最多的问题。如果它如此受欢迎，为什么不仔细看看这个问题呢？**

> 从数组中移除元素的问题似乎无处不在，因此也很重要。

在 JavaScript 中，数组本身有很多方法，其他标准对象中也有。在这些方法中，有不少可以帮助我们解决特定的问题。这意味着从题目来看，问题实际上有多个答案。让我们回顾一下最受欢迎的。

为了使解决方案更具可比性，我们将使用三个属性来描述每个解决方案:

*   **就地** —该方法是否修改原始数组？如果没有，则返回修改后的数组(没有删除元素的数组)
*   **删除重复项** —这个方法是删除指定元素的所有出现项还是只删除第一个？
*   **按值/索引** —该方法是否需要提供数组中元素的值或其索引？

为了简化示例，我们假设所有数组都包含原始值，比如数字。

# 一般情况

在这一节中，我们将评估一般情况下解决问题的最常见方法。一般来说，我的意思是删除一个在数组中没有特殊位置的元素。它不是最后的，第一个或唯一的。它只是数组元素中的常规 Joe。

## 使用`.splice()`按值删除元素

拼接法可以用来去掉一个已知索引处的元素。如果您只知道值，首先您必须确定该项目的索引。然后使用索引作为开始元素，只删除一个元素。

Remove element using splice method

解决方案的属性:

Properties of splice method solution

## 使用移除数组元素。filter()方法

通过提供过滤功能，可以从数组中 ***过滤出*** 的具体元素。然后为数组中的每个元素调用这个函数。

Remove element using filter method

解决方案的属性:

Properties of filter method solution

## 通过扩展 Array.prototype 移除数组元素

数组的原型可以用其他方法扩展。这样的方法将可用于创建的数组。
**注意:**从 JS 的标准库(比如 Array)扩展对象的原型被一些人认为是反模式。

Remove element by extending Array prototype

解决方案的属性:

Properties of extending prototype solution

## 使用删除运算符删除数组元素

使用删除运算符不会影响长度属性。也不会影响后续元素的索引。删除的项目不会被移除，但会变得未定义。delete 操作符用于从 JavaScript 对象中删除属性，这些对象是数组。

Remove element using delete operator

解决方案的属性:

Properties of delete operator solution

## 使用`Object`实用程序(> = ES10)移除数组元素

ES10 引入了 *Object.fromEntries* ，可用于从任何类似数组的对象创建所需的数组，并在此过程中过滤掉不需要的元素。

Remove element using Object.fromEntries

解决方案的属性:

Properties of fromEntries solution

# 特殊情况

在这一节中，我们将根据指定元素在数组中的位置来评估三种特殊情况:在开头、在结尾或者两种情况都有，即它是数组中唯一的元素。

## 如果元素在数组的末尾，则删除它..

**..通过改变数组*长度* :**

通过将 length 属性设置为小于当前值的值，可以从数组末尾删除 JavaScript 数组元素。索引大于或等于新长度的任何元素都将被移除。

Remove last by changing length property

解决方案的属性:

Properties of changing length solution

**..通过使用*。pop()* 方法:**

pop 方法移除数组的最后一个元素，返回该元素，并更新 length 属性。pop 方法修改调用它的数组，这意味着与使用 delete 不同，最后一个元素被完全删除，数组长度减少。

Remove last by using pop method

解决方案的属性:

Properties of pop method solution

## 如果元素在数组的开头，则删除它

*。shift()* 方法的工作方式与 pop 方法非常相似，只是它移除的是 JavaScript 数组的第一个元素，而不是最后一个元素。当元素被移除时，剩余的元素向下移动。

Remove first by using shift method

解决方案的属性:

Properties of shift method solution

## 如果元素是数组中唯一的元素，则移除该元素

最快的方法是将数组变量设置为空数组。

Remove the only element by creating new array

解决方案的属性:

Properties of new array creation solution

或者，可以通过将 length 设置为 0 来使用前面一个示例中的技术。

# 结论

JavaScript 变得越来越对开发者友好。每个版本都添加了新的工具。其结果是有很多方法来处理简单的任务。从数组中移除元素的问题似乎无处不在，因此也很重要。我希望你通过阅读这篇文章学到了一些新的东西。我也希望它能鼓励你学习 JS 中简单的东西，正如某位智者曾经说过的:

> 看在上帝的份上，先从小事做起，然后再向更大的目标前进。— [爱比克泰德](http://en.wikipedia.org/wiki/Epictetus)

*更多内容看* [***说白了. io***](https://plainenglish.io/)