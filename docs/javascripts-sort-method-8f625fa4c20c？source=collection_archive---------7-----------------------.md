# JavaScript 的排序方法

> 原文：<https://javascript.plainenglish.io/javascripts-sort-method-8f625fa4c20c?source=collection_archive---------7----------------------->

![](img/8bd2214965ffbac774c420ac3ee1d91e.png)

Farm-fresh beans sorted by type/color

在学 JavaScript 之前，我学过 Ruby。Ruby 有那种开箱即用的感觉。JavaScript 有这样一个*,指令在哪里？*感觉。Ruby 有非常方便的内置方法，这些方法使编码变得精彩，但也创造了一种情况，在这种情况下它们的功能可以被认为是理所当然的。最常见和最常用的内置方法之一(在许多语言中)是 sort 方法。自然如此。数据本质上是以集合的形式存储和组织的，通常需要以不同的方式对数据进行排序，以获得不同的输出。为了对数据数组进行排序，Ruby 有直观的`sort()`方法和`sort_by()`方法。Javascript 有:

```
Array.prototype.sort()
```

已经让人不安了，对吧？那么，让我们解决它。第一步是不要担心原型。它与从 ethereal JS plain 继承属性的数组对象本身有关，这个词通常不与方法调用一起使用，您可以在这里了解原型[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype)*。*

*sort 方法用于对[数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)进行排序。在 JavaScript 中，sort 方法最擅长的是按字母顺序(默认情况下是升序)对字符串数组进行排序。像这样:*

```
*const beansArray = ['yellow', 'purple', 'speckled', 'green']beansArray.sort()console.log(beansArray)=> ['green', 'purple', 'speckled', 'yellow']*
```

*这很好，但是为什么排序方法做得最好呢？为什么会有更多的欲望？sort 方法将数组中的元素转换成字符串(在前面的例子中领先一步并大获全胜)，然后根据它们的 [UTF-16](https://en.wikipedia.org/wiki/UTF-16) 字符代码值依次比较每对相邻的元素。在按字母顺序排列的情况下，字符‘a’的值小于‘b’，因此被移动到前面的位置，而‘b’被移动到后面的位置。*

*其他数据类型呢？让我们尝试对一组数字进行排序。*

```
*const numbers = [1, 300, 5, 20, 40]numbers.sort()console.log(numbers)=> [1, 20, 300, 40, 5]*
```

*这里我们很快揭示了排序的局限性。上一个示例的结果没有按照预期的升序排列。由于数组的元素在比较之前被转换为字符串，`300`中的`3`将`300`放置在`20`和`40`之间。当对字符串以外的数据类型进行排序时，这不是一个可靠的方法。虽然一开始看起来有点不方便，但是这个问题的解决方案是有力量的:*回调*。*

*回调是 JavaScript 不可或缺的。回调本质上是一个在执行时被另一个函数调用的函数。如何与`.sort()`一起使用是通过将一个定制的比较函数作为参数传递给排序方法，即回调。大概是这样的:*

```
*Array.sort(comparisonFunction(elementArg1, elementArg2))*
```

*让我们来探讨一下比较函数的作用以及它是如何支持排序方法的。假设我们要对一个对象数组进行排序，每个对象有两个键值对，分别代表类型和数量。要按类型排序:*

```
*const beansArray = [
  {type: 'yellow', qty: 200}, 
  {type: 'purple', qty: 30}, 
  {type: 'speckled', qty: 100}, 
  {type: 'green', qty: 4000}
]function compareType(a, b) {
  if (a.type > b.type) {
    return 1
  } else if (a.type < b.type) {
    return -1
  } else {
    return 0
  }
}beansArray.sort(compareType)console.log(beansArray)=> [
  {type: 'green', qty: 4000},
  {type: 'purple', qty: 30}, 
  {type: 'speckled', qty: 100}, 
  {type: 'yellow', qty: 200}
]*
```

*有用多了。由于回调函数`compareType`，这个数组的对象成功地按照每个对象的类型值按字母升序排序。假设理解了数组的语法、它包含的对象和基本的排序方法，让我们来理解一下`compareType`函数。当`.sort()`调用`compareType`时，它从`beansArray`获取元素对的顺序比较，并将它们作为两个参数传递。本案:`(a, b)`。在`compareType`函数中完成的工作是通过一个条件语句传递这些元素对。在这种情况下:一个`if/else if/else`语句。由于`a`和`b`代表对象，在这种情况下，要比较的查找值可以分别由`a.type`和`b.type`访问。根据对每个条件的评估，`compareType`将返回一个`-1`、`0`或`1`，并将其传递回排序方法。*

*   *当返回`-1`时(较小的值):`a`在`b`之前。*
*   *当`0`返回(相等值)时:`a`和`b`保持命令。*
*   *当返回`1`时(较大值):`b`在`a`之前。*

*此时，`.sort()`拿起滚轮，根据`compareType`函数的返回值对数组进行排序。有一种速记方法可以代替`if/else if/else statement`:*

*`string.prototype.localeCompare()`*

*[语言环境比较方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)比较一对字符串，并通过上面列出的逻辑确定它们的排序顺序。利用`.localeCompare()`和一些箭头函数简写，前面例子中带有比较回调的排序方法也可以写成这样:*

```
*const beansArray = [
  {type: 'yellow', qty: 200}, 
  {type: 'purple', qty: 30}, 
  {type: 'speckled', qty: 100}, 
  {type: 'green', qty: 4000}
]beansArray.sort((a, b) => a.type.localeCompare(b.type))console.log(beansArray)=> [
  {type: 'green', qty: 4000},
  {type: 'purple', qty: 30}, 
  {type: 'speckled', qty: 100}, 
  {type: 'yellow', qty: 200}
]//Cool beans!*
```

***请注意，虽然像* `*if/else*` *语句这样的条件语句允许改变排序顺序(例如，降序而不是升序)，但语言环境比较方法通常会生成升序排序顺序。* `*.localeCompare()*` *可以采取附加的、可选的自变量来确定其行为。或者，附加方法如* `[*.reverse()*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)` *可以与* `*.localeCompare()*` *链接。**

*由于`.sort()`将值转换为字符串以便进行比较，所有 J [avaScript 数据类型](https://javascript.info/types)都可以使用(有些有用，有些没用)。借助回调函数的力量，`.sort()`可以用来对对象数组、数组、嵌套对象、嵌套数组等进行排序。*

*当按数值排序时，sort 方法的回调函数可以通过从第一个元素中减去比较对中的第二个元素来产生它的返回。这有效地复制了较小、相等或较大值标记的返回。让我们按数量对上一个示例中的数组进行排序:*

```
*const beansArray = [
  {type: 'yellow', qty: 200}, 
  {type: 'purple', qty: 30}, 
  {type: 'speckled', qty: 100}, 
  {type: 'green', qty: 4000}
]beansArray.sort((a, b) => a.qty - b.qty) //ascending sort by default//  sort sequence://  200 - 30 = 170 | positive (return 1) | b precedes a 
//  => 30, 200//  200 - 100 = 100 | positive (return 1) | b precedes a
//  => 30, 100, 200//  200 - 4000 = -3800 | negative (return -1) | a precedes b
//  => 30, 100, 200, 4000console.log(beansArray)=> [
  {type: 'purple', qty: 30}, 
  {type: 'speckled', qty: 100}, 
  {type: 'yellow', qty: 200},
  {type: 'green', qty: 4000}
]*
```

*虽然可以对字符串或数字数组执行简单的排序，例如*

*`['c', 'a', 'b'].sort() returns ['a', 'b', 'c']`*

*`[2, 1, 3].sort((a, b) => a — b) returns [1, 2, 3]`*

*回调允许许多条件逻辑，这些条件逻辑可以通过多个属性对复杂对象的数组进行排序。最后一个例子，让我们假设前面例子中的`beansArray`有相同数量的不同类型。对象可以先按数量分类，然后再按类型分类:*

```
*const beansArray = [
  {type: 'yellow', qty: 200},
  {type: 'purple', qty: 200},
  {type: 'speckled', qty: 100},
  {type: 'green', qty: 4000}
]function compareQtyAndType(a, b) {
  if (a.qty > b.qty) return 1
  if (a.qty < b.qty) return -1
  if (a.type > b.type) return 1
  if (a.type < b.type) return -1
}
//note use of shorthand if statementsbeansArray.sort(compareQtyAndType)console.log(beansArray)const beansArray = [
  {type: 'speckled', qty: 100},  
  {type: 'purple', qty: 200},
  {type: 'yellow', qty: 200},
  {type: 'green', qty: 4000}
]*
```

*`compareQtyAndType`函数首先评估代表数量的值的比较。每个对象的排序位置根据返回的`1`或`-1`进行移动。如果值相等并且评估为返回`0`，则对象不会基于数量移动排序位置，而是基于表示类型的值进行比较评估，然后按字母顺序排序。*

*我希望这篇文章 *sort* 能让你更好地理解 JavaScript 的排序方法，并激发你对其可能性的好奇心，或者至少让你更喜欢 Ruby。*

*[github.com/dangrammer](https://github.com/dangrammer)T8[linked.com/in/danieljromans](https://www.linkedin.com/in/danieljromans/)danromans.com*