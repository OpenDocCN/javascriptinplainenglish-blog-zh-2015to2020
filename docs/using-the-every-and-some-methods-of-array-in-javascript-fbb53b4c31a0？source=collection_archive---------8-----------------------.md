# 在 JavaScript 中使用数组的 every()和 some()方法

> 原文：<https://javascript.plainenglish.io/using-the-every-and-some-methods-of-array-in-javascript-fbb53b4c31a0?source=collection_archive---------8----------------------->

学习如何使用 JavaScript 中的各种方法。

![](img/47ca5d3f71d9718cd134afe74a2791d2.png)

*   `every` →检查数组的所有元素是否满足提供的回调函数
*   `some` →检查数组中的任何一个元素是否满足提供的回调函数

# 每个

every 方法将一个测试函数作为参数，数组的所有元素都根据测试函数进行测试。

如果测试函数为数组的所有元素返回真值(任何真值)，那么 every 方法返回 true。

如果测试函数为任何一个元素返回 false，那么 every 方法将返回 false，并且不测试其余的元素。

对于空数组，测试方法返回 true。

```
function isEven(item, index, sourceArray) { **return (item % 2 === 0);** } var array = [0,2,4,6]; **array.every(isEven); // true**
```

上面的例子可以用箭头函数简化

```
var array = [0,2,4,6]; 

array.every( **item => (item % 2 === 0)** ); // true
```

我们还可以添加和删除源数组的元素

*   如果我们将元素添加到源数组中，那么测试函数不会对添加的元素进行测试

*   如果我们删除源数组的元素，那么被删除的元素就不会被测试

如果数组中现有的未被访问的元素被回调改变，那么访问测试函数时该元素的值将是该元素修改后的值。

注意事项

注 1:下面的方法将返回 false，因为`""`是一个 false 值。

```
var array = [1]; array.every( item=> "" ); // **false**
```

注意 2:下面的方法将返回 true，因为`non-empty string`是一个真值。

```
var array = [1]; array.every( item=> "I am truthy value" ); // true
```

注 3:如果我们对一个空数组测试每个方法，那么它返回真

```
[].every(item => item); // true
```

注 4:如果我们想检查数组中的所有元素是否都为真，那么我们可以像

```
var array = [1,"test", function(){}, "true"];  
array.every(item=> item); //true **array.push(0);  
array.every(item=> item); // false**
```

# 一些

`some`方法通过将数组传递给`callback function(testing function) .`来测试数组的每个元素

如果任何元素返回`truthy`值，那么`some`函数将返回`true`，并且剩余的**元素不被测试。**

如果数组中没有元素返回`true`，那么`some`方法将返回`false`。

这个方法**为空数组返回 false。**

```
var array = [false, 0 , 1]; array.some(item => item); // true
```

使用`some`检查数组中是否有值

```
function isPresent( **arr, itemToFind** ) { return arr.some( **item => item === itemToFind** );}const fruits = ['🍏 ', '🍌 ', '🥭 ', '🍉 '];isPresent(fruits, '🍏 '); // trueisPresent(fruits, '🍓'); // false
```

被删除的元素不会被测试。

在调用`some()`开始后添加到数组中的元素将不会被回调测试。

如果数组中现有的未被访问的元素被回调改变，那么访问测试函数时该元素的值将是该元素修改后的值。

注 1:对于空数组，`some`方法返回 false

```
var arr = [] ;arr.some();
```

**不要忘记传递回调函数**

> 对于`some`和`every`,如果我们忘记将回调函数传递给测试，方法将抛出异常

**打破条条框框**

> 如果你想中断一个`some`方法，那么我们可以在`some` 方法中返回`true`，如果你想中断一个`every`方法，那么在`every`方法中返回`false`。

感谢阅读📖。我希望你喜欢这篇文章。如果你发现任何错别字或错误，给我发一封私信📝谢谢🙏 😊。

关注我 [JavaScript Jeep🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----98efbae5e8aa----------------------)。

**请捐款** [**此处**](https://www.paypal.com/paypalme2/jagathishSaravanan) **。你捐款的 98%都捐给了需要食物的人🥘。提前感谢。**

[](https://levelup.gitconnected.com/creating-a-pdf-viewer-in-javascript-9b988b5ec163) [## 用 JavaScript 创建 PDF 查看器

### 了解如何使用 PDF.js 在 Javascript 中创建 PDF 打开器

levelup.gitconnected.com](https://levelup.gitconnected.com/creating-a-pdf-viewer-in-javascript-9b988b5ec163) [](https://medium.com/better-programming/creating-dice-in-flexbox-in-css-a02a5d85e516) [## 在 CSS 中的 Flexbox 中创建骰子

### 了解如何使用 flexbox 在 CSS 中创建骰子

medium.com](https://medium.com/better-programming/creating-dice-in-flexbox-in-css-a02a5d85e516)