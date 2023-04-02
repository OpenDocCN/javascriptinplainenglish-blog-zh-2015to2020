# JavaScript 中的一些和所有方法

> 原文：<https://javascript.plainenglish.io/the-methods-some-and-every-in-javascript-8b303a36eaae?source=collection_archive---------9----------------------->

## 了解一些高阶函数

![](img/477d39ed9ebde7a650adea0460502e68.png)

Photo by [Adeolu Eletu](https://unsplash.com/@adeolueletu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

JavaScript 引入了许多方法，允许开发人员用这种语言完成许多任务。其中一些是经常使用的，如 forEach、map 和 filter。然而，有几个似乎使用频率较低，如*一些*和*每*一次。这些方法被称为高阶函数，因为它们接受函数作为参数。

在本文中，我们将通过实例来学习高阶函数*的一些*和*的每一个*。让我们开始吧。

# 一些方法

方法`some()`是一个高阶函数，这意味着它接受一个回调函数作为参数。有时，您希望检查数组中是否至少有一个元素满足指定的条件。

嗯，方法`some`测试**数组中是否至少有**一个元素通过了由提供的回调函数实现的给定条件。它返回一个布尔值(`true`或`false`)。

看看下面的例子:

```
let marks = [ 4, 5, 7, 9, 10, 3 ];// Check if at least one elemnt is less than 5.
lessThanFive = marks.**some**(function(element) {     
  return element < 5; 
});
console.log(lessThanFive);
// true.// Check if at least one elemnt is less than 3.
lessThanThree = marks.**some**(function(element) {     
  return element < 3; 
});
console.log(lessThanThree);
// false.
```

如果需要，也可以使用 ES6 语法:

```
let marks = [ 4, 5, 7, 9, 10, 3 ];lessThanFive = marks.**some**(element => element < 5);
console.log(lessThanFive);
// true.lessThanThree = marks.**some**(element => element < 3);
console.log(lessThanThree);
// false.
```

正如你在上面看到的，在这种情况下，方法`some()`返回`true`。该方法有一个回调函数作为参数，用于检查数组`marks`中是否至少有一个元素小于 5(true)。我们还使用相同的例子来检查数组`marks`中是否至少有一个元素小于 3(false)。

注意，回调函数在这种情况下只有一个参数，即数组中正在处理的当前元素。

回调可以接受其他可选参数，比如数组中正在处理的当前元素的索引。它还可以接受第三个参数，即调用`some()`的数组。

# 该方法每

方法`every()`也是高阶函数。就像方法`some()`，它有一个接受三个参数的回调函数:一个强制参数和两个可选参数。它还返回一个布尔值。

唯一的区别是方法`every()`测试数组中的所有**元素是否通过由提供的函数实现的给定条件。它主要检查数组中的每个元素是否都符合给定的条件。**

以下示例使用方法`every()`检查 numbers 数组的每个元素是否都大于零:

```
let numbers = [1, 3, 5];
let result = numbers.**every**(function (e) {     
  return e > 0; 
});console.log(result);
// true.
```

现在让我们使用带有 ES6 语法的方法`every()`来检查数字数组中的每个元素是否都大于 2:

```
let numbers = [1, 3, 5];
let result = numbers.**every**( e  => e > 2);console.log(result);
// false.
```

如您所见，第一个示例返回`true`，因为 numbers 数组中的所有元素都大于 0。但是在第二个例子中，它返回`false`，因为 1 不大于 2。

# 结论

如果您使用数组，并且想要检查这些数组是否满足某些条件，那么`some()`和`every()`方法可能是非常有用的高阶函数。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**

## 更多阅读

[](https://medium.com/javascript-in-plain-english/5-extremely-useful-apis-for-your-next-projects-43647920a3e4) [## 5 个对你下一个项目非常有用的 API

### 令人惊叹的 API 会激发你的下一个项目

medium.com](https://medium.com/javascript-in-plain-english/5-extremely-useful-apis-for-your-next-projects-43647920a3e4)