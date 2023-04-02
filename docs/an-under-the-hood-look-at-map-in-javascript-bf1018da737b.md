# 深入了解 JavaScript 中的 map()函数

> 原文：<https://javascript.plainenglish.io/an-under-the-hood-look-at-map-in-javascript-bf1018da737b?source=collection_archive---------10----------------------->

## 关于如何从头开始构建 map()函数的小演练的第一部分

![](img/c23b07166f8d97ee69c853232899bb1a.png)

Photo by [Cookie the Pom](https://unsplash.com/@cookiethepom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

不久前，一个朋友向我挑战，要我建造。map( ) native 方法，并更进一步，白板演示当涉及到全局执行上下文、全局内存和调用堆栈时会发生什么。作为一个彻头彻尾的编码游戏新手，我看了一眼这个挑战，然后离开了舞台。

回过头来看，这是一项艰巨的任务，尤其是对于刚刚学会如何用 *let* 和 *const 声明变量的人来说。*但我发现学习如何建造。map()帮助我更好地理解了 JavaScript 是如何工作的。

这是由两部分组成的系列的第一部分:第一部分将简单地构建. map()函数，第二部分将展示 JavaScript 如何工作，解释全局执行上下文、全局内存和执行线程。所以，对于所有正在阅读这篇文章的人，我终于征服了曾经是我的珠穆朗玛峰。

## 首先，什么是。map()？

根据 MDN web docs，map 方法“ ***创建一个新的数组*** *，用调用数组中每个元素上提供的函数的结果填充*

尽管我对 JavaScript 文档有着特殊的兴趣，但对于第一次阅读这篇文章的人来说，当你意识到并非所有这些都有意义时，你可能会听到脑海中响起悲伤的小提琴声。所以这里有一个更快捷的解释方法:

1.  。map()是 JavaScript 中存在的数组方法。
2.  它接受另一个函数作为参数，这就是所谓的回调函数——根据定义，回调函数是传递给另一个函数的函数。
3.  它将这个传入的函数(回调)应用于数组中被调用的每个元素。
4.  最后，它返回一个包含计算值的新数组。

```
array.map(callback)  =>Syntax 
```

## 为什么我们要用？map()？

当你第一次学习编码时，作为一般的经验法则，你经常被告知不要改变原始数据。太好了！不要改变数据。如果你和我一样，你需要一个具体的理由，比如为什么你不应该做某件事，否则你的思想会徘徊在如果你做了会发生什么的可能性上。所以对于所有像我一样的人来说，这里有一个很小的列表，列出了突变不好的原因。

1.  如果您改变了代码中的一部分数据，可能会导致代码中的另一部分发生变化，这意味着出现错误，除非您喜欢浪费时间来修复错误，否则这不是对时间的有效利用。
2.  当涉及到修复由突变引起的 bug 时，很难精确定位代码崩溃的确切位置。
3.  从更大的角度来看——比如说，你在一家大型公司工作，这家公司将数据托管在远程服务器上，或者使用云计算。作为在公司工作的人，你需要在本地访问数据，这样你就可以运行一些代码(你想出的解决方案)。通过不改变你电脑上的数据，你可以更大规模地高效工作。

简而言之，您不希望更改您的数据，而是希望以某种方式使用它，同时保持原始数据不变。这就是。map()进来，它不会改变原始数组。相反，在 map 方法中，一旦对原始数组中的每个元素进行了回调计算，它就分配一个新的空数组(在幕后)并返回这个新数组。如果这没有意义，请不要担心，我将在下一节和下一篇博文中展示这是如何实现的。这里的关键是原始数组保持不变。

把它想象成隔离期间的 Instagram 帖子，你可能会打扮好，化妆，可能会使用滤镜，然后拍出惊人的照片。事实是，当你脱下所有你已经应用的，跳回你舒适的睡衣，你保持不变(你是原来的数组)。图片直播，在 Instagram 上。在本例中，图片是新的数组，所有在幕后完成的工作是。map()方法。

## 正在建设。地图( )

对于这个具体的例子，我们将从声明一个我们将要处理的数组开始。

```
let originalArray= [1,2,3,4]
```

其次，让我们构建回调函数，我们将把它传递给我们的 map 函数。对于这个回调函数，我希望传入的数字(参数)在返回时加上 2。

```
**Regular function declaration:**function addTwo(num){
    return num + 2;
}**Arrow function:**let addTwo=(num)=>{num +2}// a number will be passed in and the return value will be the number+2 
```

第三，我们将构建带有两个参数的 map 函数，一个数组和一个回调。正如每个程序员都会建议的那样，我也将在下一节中这样做，最好是从伪代码开始，然后构建函数。

```
//map function-takes in array, and callback function,//declare a new variable and assign it to an empty array,//initiate a for loop that will iterate through the array and invoke the callback function with the values of array[i] being passed to it within the for loop// push that evaluated result into the new array,//return the new array,
```

在伪代码之后:

```
function map(array,callback){
    let newArray=[]; for (let i=0; i<array.length; i++){
         newArray.push(callback(array[i]))
    }; return newArray
}
```

上面的 map 函数就是 native 的方法。地图正在引擎盖下做。下面的代码片段将向您展示如何调用 map 函数、我们的特定代码片段的返回值，以及如何使用本机 map 方法语法。

```
//to invoke function**map(originalArray, addTwo)**//will return [3,4,5,6]//to use the native map method syntax**originalArray.map(addTwo)** or
**originalArray.map(num=>num+2)**
```

## 结论

在下一个系列中，我们将使用相同的 map 函数来研究 JavaScript 如何在代码行中运行，它在代码行中存储变量、函数和对象的值。以及 JavaScript 中的执行上下文。但是现在，这里是关键的要点:

*   不要改变你的数据！
*   。map()在数组上被调用，
*   。map()接受一个回调函数并返回一个新数组，
*   。map()不会改变调用它的原始数组。

## 参考

*   [https://alistapart.com/article/why-mutation-can-be-scary/](https://alistapart.com/article/why-mutation-can-be-scary/)
*   [https://www.geeksforgeeks.org/javascript-array-map-method/](https://www.geeksforgeeks.org/javascript-array-map-method/)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Array/map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)