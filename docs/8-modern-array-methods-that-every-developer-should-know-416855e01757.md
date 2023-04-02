# 每个开发人员都应该知道的 8 种现代数组方法

> 原文：<https://javascript.plainenglish.io/8-modern-array-methods-that-every-developer-should-know-416855e01757?source=collection_archive---------8----------------------->

## 非常有用的方法让你的生活更轻松

![](img/4d544b49dd6888a823b29ad3a5aea716.png)

Photo by [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在用很长的代码行执行数组操作时，您是否曾经停下来想知道是否有更简单的方法？

在 JavaScript 中，有各种非常有用的数组方法可供我们使用。如果使用正确，它们可以帮助您用几行可读的代码完成伟大的事情。

我们将看到 8 种阵列方法可以大大减少您的工作量，并为您带来好处。

让我们开始吧。

## 1.地图

Map 方法允许您使用指定的操作将现有数组转换为新数组。

```
var numbers = [4, 9, 16, 25];var x = numbers.map(*v*=> 2*v)console.log(x)console.log(numbers)
```

`map`方法将返回一个新数组，并将其存储在变量‘x’中。原始数组“数字”将保持不变。

上述代码的预期输出将是:

```
[8,18,32,50] // x[4,9,16,25]  // numbers
```

## 2.发现

这是另一个有用的。

`find`方法的作用是让我们在数组中找到一个特定的对象。

语法类似于`map`方法，除了我们必须基于一些特定的检查返回 true 或 false。

一旦第一次返回 true，该方法就停止对数组的迭代。

值得注意的是，它返回数组中与我们的搜索查询匹配的第一次。

```
var data = [
  {item:'Coffee', price:10},
  {item:'Tea',price:12},
  {item:'Shirt',price:25},
  {item:'Pen',price:6},
  {item:'Shirt', price:10}
];
var searchEle=data.find(v => v.item=='Shirt')
console.log(searchEle)
```

输出:

```
{
  item:"Shirt",
  price:25
}
```

如您所见，我们的数组“data”中有两个对象，值为“item”和“Shirt ”,然而，`.find()`方法只返回了第一个符合我们条件的对象。

## 3.过滤器

顾名思义，这个方法允许我们过滤我们的数组。

和上面两种方法一样，这种方法也不改变原始数组。

我们将使用前一个例子中的相同的“数据”数组，我们将过滤掉价格低于 10 英镑的元素。

```
var data = [
  {item:'Coffee', price:10},
  {item:'Tea',price:12},
  {item:'Shirt',price:25},
  {item:'Pen',price:6},
  {item:'Shirt', price:10}
];
var filteredArr=data.filter(v => v.price>=10)
console.log(filteredArr)
```

如果你看一下`filter` 方法中的函数，你会发现我们正在检查当前对象的“价格”属性的值是否大于等于 10。

如果是这样，元素被添加到我们的“filteredArr”数组中。

上面代码片段的输出:

```
[
  {
    "item": "Coffee",
    "price": 10
  },
  {
    "item": "Tea",
    "price": 12
  },
  {
    "item": "Shirt",
    "price": 25
  },
  {
    "item": "Shirt",
    "price": 10
  }
]
```

您可以做的另一件有趣的事情是，您可以使用这个方法实现`find()`方法的功能。然而，有一点不同。

当使用`find` 方法时，我们只获得匹配我们的搜索查询的第一个元素，而使用`filter` 方法，我们将获得匹配我们的查询的所有元素。

如果我们使用与我们用来展示`find` 方法的例子相同的例子，会更好地展示这一点，只是这次我们将使用`filter` 方法来达到相同的目的。

```
var data = [
  {item:'Coffee', price:10},
  {item:'Tea',price:12},
  {item:'Shirt',price:25},
  {item:'Pen',price:6},
  {item:'Shirt', price:10}
];var searchEle=data.filter(v => v.item=='Shirt')console.log(searchEle)
```

我们只需将“find”关键字替换为“filter ”,其余代码保持不变。但是，输出看起来会有所不同。

```
[
  {
    "item": "Shirt",
    "price": 25
  },
  {
    "item": "Shirt",
    "price": 10
  }
]
```

如前所述，与`find` 方法不同，过滤器将返回函数返回 true 的每个对象，而不是第一次就停止。

## 4.为每一个

此方法是 for 循环的替代方法。

我们可以使用这种方法来实现相同的目的，而不是编写整个循环语句。

然而，这个方法不返回一个数组，只是简单地循环遍历它们。

```
var numbers = [4, 9, 16, 25];numbers.forEach(*v*=> console.log(v))
```

当您想简单地循环数组中的元素时，这是一个好方法。

但是，这并不妨碍您执行其他操作，例如根据某些条件将数组的值赋给其他数组。

```
var numbers = [4, 9, 16, 25];
var s=[];
numbers.forEach(
  v=> v%2==0 ? //checking for even numbers
  s.push(v*v): //adding their square to another array
  null)
console.log(s)
```

但是这些操作最好由本文前面提到的`filter`方法来执行。

我只是想展示这种可能性。

## 5.每个

这个方法检查数组中的每一项是否满足特定的标准。

这个方法返回一个布尔值而不是一个数组。

当您必须确保每个元素都有特定的属性或值时，这很有用。

```
var data = [{name:'Science', results:'passed'},{name:'Maths',results:'failed'},{name:'Computer',results:'passed'},];var finalResult=data.every(*v* => v.results=='passed')console.log(finalResult) // => false
```

例如，在我们上面的代码片段中，学生的考试结果存储在数组“data”中，为了通过期末考试，他必须通过每一门科目。

因此，我们使用了`every()`方法来检查他是否通过了所有的科目。

您甚至可以检查特定值是否存在，如下所示:

```
var data = [{name:'Science', results:'passed'},{name:'Maths',}, //here results is missing{name:'Computer',results:'passed'},];var everyEle=data.every(*v* => v.results)console.log(everyEle) // => false
```

## 6.一些

如果我们想检查某些元素是否满足我们的条件，而不是每个元素，该怎么办？

这就是`some` 方法的由来。

像`every` 方法一样，这个方法也返回一个布尔值。但是，只要至少有一个元素满足条件，它就返回 true。

```
var data = [{name:'Science', results:'passed'},{name:'Maths',results:'failed'},{name:'Computer',results:'passed'},];var finalResult=data.every(*v* => v.results=='passed')
console.log(finalResult) // => true
```

## 7.包含

这是一个非常简单的方法，它检查一个特定的元素是否包含在数组中。

值得一提的是，`includes`方法也适用于 String，并提供相同的功能。

```
var numbers = [4, 9, 16, 25];
var s=numbers.includes(4)
console.log(s) // => true
```

此方法还返回 true 或 false。

此外，您可以提供一个搜索开始位置以及第二个参数。

```
var numbers = [4, 9, 16, 25];var s=numbers.includes(4,2)//(element,starting index)console.log(s) // => false
```

## 8.减少

与我们到目前为止讨论的方法相比，这种方法相对复杂。

我们已经使用`reduce` 方法计算了“数据”数组中元素的总价。

请注意，我们没有声明一个全局变量来保存总数，而是使用了方法本身提供的变量。

```
var data = [
  {item:'Coffee', price:10},
  {item:'Tea',price:12},
  {item:'Shirt',price:25},
  {item:'Pen',price:6},
  {item:'Shirt', price:10}
];
var totalPrice=data.reduce((total,current) => {
  return current.price+total
},0)
console.log(totalPrice)
```

首先，这个方法有两个参数，而不是一个。

第一个参数是函数最后一次迭代的返回值，而第二个参数是数组中实际的当前元素。

因此，“总计”(第一个参数)包含“当前价格”(即当前对象的价格值)和总计的总和。

这个“total”值由函数返回，并构成函数下一次迭代的第一个参数。

另外，请注意，我们在函数后传递了另一个参数。这是默认值。我们希望“total”从 0 开始，因此在函数后传递 0 作为第二个参数。

## 最后的想法

JavaScript 是占主导地位的客户端脚本语言，尽管它是一种初学者友好的语言，但它被广泛用于不同的领域。

JavaScript 应用程序正变得越来越复杂，这种语言不断增加功能并自我更新以满足需求。

当务之急是学习[最佳和现代的实践](https://medium.com/javascript-in-plain-english/5-modern-practices-of-javascript-that-every-developer-should-know-1a61dc9a6ee0)来减少工作量，并用可读和简单的代码构建重要的应用程序。

JavaScript 提供了各种数组方法，旨在大大简化与数组相关的操作。但是，了解何时使用以及用于什么目的是很重要的。

使用 JavaScript 提供的最佳特性并了解何时使用它们也可以在技术面试中提供优势。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**