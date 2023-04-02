# JavaScript 高阶函数:简单介绍和实现

> 原文：<https://javascript.plainenglish.io/javascript-higher-order-functions-249a045b8c1?source=collection_archive---------13----------------------->

![](img/9fbca259f68667ef9ce202fb20f7e18b.png)

Photo by [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

高阶函数已经存在很长时间了，JavaScript 开发人员也使用了很长时间(有意或无意)。

## 什么是高阶函数？

简单地说(通俗地说)，把一个函数作为输入或者返回一个函数的函数，可以归为高阶函数。

## **不知不觉使用高阶函数(HOF)**

有些情况下，我们在不知不觉中使用了 HOFs。
同样的一个基本例子是 DOM 元素事件监听器。

```
<html><head></head><body><button type="button">Click Me</button></body><script>*const* btn = document.querySelector('button')btn.addEventListener('click', fireClick)*function* fireClick(){*console*.log('Clicked');}</script></html>
```

这里创建了一个简单的函数，即 fireClick，并作为参数传递给 Click 事件侦听器，这主要是 HOF 的用法之一。

Hof 的其他常用示例有 map、reduce、filter、some、every，它们经常用于数组操作。

我们来看看其中一个经常使用的功能，即地图。

Map 用于迭代数组的每个元素，并根据需要返回修改后的数组。

在我们的例子中，我们将每个元素乘以 6(只是一个随机的数字，没什么特别的)并得到输出。

```
*const* arr = [1,2,3,4];*const* modifiedArray = (element,index , array) *=>* {return `${index} - ${element * 2}`}*let* newArr = arr.map(modifiedArray)*console*.log(newArr);  // Output --> ["0 - 2", "1 - 4", "2 - 6", "3 - 8"]
```

这里，modifiedArray 是传递给 map 函数的回调函数，它在每个元素上运行，并接受三个参数→ **当前元素、当前索引**和发生迭代的数组。

## 那么，我们可以创建自己的 Hof 吗？

答案是肯定的，我们将立即创建一个。我们将创建自己版本的地图功能。

```
*const* arr = [1,2,3,4];*const* duplicateMap = (arr,modifiedArray) *=>* {     // Step 1*let* result = [];                                  // Step 2for(*let* i=0 ; i < arr.length ; i++) {             // Step 3result.push(modifiedArray(arr[i],i))             //  Step 4}return result;                                   // Step 5}*const* modifiedArray = (element,index) *=>* {       // Step 4.1return `${index} - ${element * 6}`               // Step 4.2}*let* newArr = duplicateMap(arr,modifiedArray)*console*.log(newArr); // Output -->  ["0 - 6", "1 - 12", "2 - 18", "3 - 24"]
```

这里发生了什么？我们一步一步来。

步骤 1 → duplicateMap 是我们自定义的 Map 函数，它有两个参数:

1。输入数组

2.最终修改我们的数据的函数(这里命名为 modified array)
这个步骤使 duplicateMap 成为一个高阶函数，因为它接受一个函数作为输入。

步骤 2 →我们创建一个空数组，最终存储我们修改过的数组。

步骤 3 →数组上的简单迭代。

步骤 4 →在这里，我们对每个元素调用回调函数，该函数将当前元素和当前索引作为参数。

步骤 4.1 →我们在这里用当前元素和当前索引作为参数定义回调函数。

步骤 4.2 →这里是我们的实际逻辑，其中我们根据我们的要求修改每个元素并返回相同的内容。

步骤 4(重新访问)→将步骤 4.2 中返回的更改数据推入步骤 2 中创建的数组。

步骤 5 →返回结果数组。

这就是我们自定义映射函数的全部实现，它实际上是一个高阶函数。

这是高阶函数和自定义实现的基本介绍。希望你对此有所了解。

你可以在这里阅读我的其他文章[，另外，你可以订阅我的时事通讯以获得我发表的最新话题。](https://medium.com/@avinash.dev21987)

*更多内容看* [***说白了。报名参加我们的***](https://plainenglish.io/) **[***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)*和*[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*。加入我们的* [***社区***](https://discord.gg/GtDtUAvyhW) *。***