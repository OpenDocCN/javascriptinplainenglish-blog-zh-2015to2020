# 如何用 JavaScript 在数组中搜索

> 原文：<https://javascript.plainenglish.io/how-to-search-in-an-array-in-javascript-ecbba894cf65?source=collection_archive---------4----------------------->

## 让我们来看看完成这项任务的几种方法

![](img/babef413ad26fc81e8a5428d0158989a.png)

Photo by [Eugene Zhyvchik](https://unsplash.com/@eugenezhyvchik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇文章中，我将向你介绍一些我最常用的搜索数组的方法。让我们开始吧！

# Array.indexOf()

**indexOf()** 使用严格比较来查找项目，然后返回该项目的索引。

例如:

```
let languages = [‘Java’, ‘Kotlin’, ‘Python’, ‘Ruby’, ‘JavaScript’];console.log(languages.indexOf(‘Ruby’)); // 3
console.log(languages.indexOf(‘Golang’)); // -1
```

如果在数组中没有找到该值，将返回 **-1** 。

**indexOf()** 返回从第一个位置开始找到的第一个项目的索引。如果您想从最后一个位置开始搜索，请使用 **lastIndexOf()**

```
let numbers = [1, 6, 3, 1, 5];console.log(numbers.lastIndexOf(1)); // 3
console.log(numbers.indexOf(1)); // 0
```

# Array.includes()

使用 **includes()** 来查找数组中的一个项目非常简单。你需要做的就是将一个值作为参数传递给函数，返回 true**或 false**。****

****例如:****

```
**let jobs = [‘Doctor’, ‘Programmer’, ‘Designer’, ‘Civil Engineer’, ‘Teacher’];let hasDoctor = jobs.includes(‘Doctor’); // true
let hasYoutuber = jobs.includes(‘Youtuber’); // false**
```

****在上例中，**医生**在列表**工作**中，所以返回的结果为**真**。反之， **jobs** 不包含 **Youtuber** ，则返回 **false** 。****

****你需要记住的一点是， **includes()** 的比较是严格的。为了使它更清楚，看一下下面的例子:****

```
**let stuff = [100, ‘banana’, ‘2020’];console.log(stuff.includes(‘100’)); // false
console.log(stuff.includes(100)); // true**
```

******100** 和**‘100’**是不同的类型，所以**stuff . includes(‘100’)**结果返回 **false** 。****

******includes()** 有第二个参数，表示应该从什么位置开始搜索。第二个参数的默认值是 0。让我们回到第一个例子，如果你想从索引 1 开始寻找**医生**会发生什么？****

```
**let hasDoctor = jobs.includes(‘Doctor’, 1);**
```

****正如你在列表**工作**中看到的，从索引 1 到列表末尾没有**博士**。因此，**假**将被返回。****

# ****Array.find()****

******find()** 并不是简单地传递需要搜索的值，而是需要一个回调函数作为参数。****

****例如:****

```
**let fruits = [‘mango’, ‘banana’, ‘strawberry’, ‘orange’];console.log(fruits.find(item => item === ‘banana’)); // banana
console.log(fruits.find(item => item === ‘kiwi’)); // undefined**
```

****结果是数组中满足所提供条件的第一项的值。****

****嗯，这很容易理解，但我认为没有人会在像例子这样简单的条件下使用 **find()** 。我的意思是，当你选择 **find()** 进行搜索时，你应该创建一个更复杂的条件来利用它。类似于:****

```
**fruits.find(item => item.length > 3 && item !== ‘mango’);**
```

# ****Array.filter()****

****有时，您需要满足特定条件的项目列表。你可以用**滤镜()**来完成任务。****

```
**let movies = [‘Spiderman’, ‘The Avengers’, ‘Flash’, ‘Wonder Woman’];console.log(movies.filter(item => item.length > 5)); // [‘Spiderman’, ‘The Avengers’, ‘Wonder Woman’]**
```

# ****额外收获:创建你自己的功能来满足你的需求****

****在使用内置 JavaScript 函数或外部库完成任务后，我倾向于只使用纯 JavaScript 编写代码。我觉得这样做很有趣，而且它帮助我磨练了我的编码技能。****

****所以对于这个搜索问题，我将使用**作为循环**:****

```
**let findIndex = function (list, value) {
  const length = list.length || 0;
  if (length === 0) {
    return -1;
  }

  for (let i = 0; i < length; i++) {
    if (list[i] === value) {
      return i;
    }
  } return -1;
};let simpleList = [‘I’, ‘am’, ‘28’];console.log(findIndex(simpleList, ‘am’)); // 1
console.log(findIndex(simpleList, 28)); // -1**
```

****这只是一个简单的原始类型项目的解决方案。为了处理对象，你需要做更多的工作。****

****关键是你需要更深入地研究你使用的每一个函数，以理解它是如何工作的，以及你是否可以开发你自己的函数。我认为这是你应该采取的编码思维方式。****

****以上是我在数组中搜索条目时使用的方法。不知道哪个用的最多。这取决于我的情况。例如，如果我想检查一个项目是否存在，我将选择 **includes()** 。当条件变得更复杂时，我将使用 **find()** 。****

****希望你喜欢这篇文章！****

****[](https://medium.com/javascript-in-plain-english/8-ways-to-solve-javascript-array-issues-you-need-to-know-about-de4fb3770e5a) [## 你需要知道的解决 JavaScript 数组问题的 8 种方法

### 你的生活会更轻松。

medium.com](https://medium.com/javascript-in-plain-english/8-ways-to-solve-javascript-array-issues-you-need-to-know-about-de4fb3770e5a)****