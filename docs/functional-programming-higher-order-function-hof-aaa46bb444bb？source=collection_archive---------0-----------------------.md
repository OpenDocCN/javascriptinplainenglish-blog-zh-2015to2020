# 函数式编程——高阶函数

> 原文：<https://javascript.plainenglish.io/functional-programming-higher-order-function-hof-aaa46bb444bb?source=collection_archive---------0----------------------->

## 什么是 HOF，如何使用它？

![](img/92cfec0c47bc1de77e1c57022b05c752.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在函数式程序设计中，HOF 使用非常广泛，被认为是一个重要的因素。但是你可能没有意识到这个概念，尽管你可能每天都在使用它。在这篇文章中，我将谈谈 HOF 是什么以及如何使用它。

# 在 JavaScript 中，函数是一级对象

看看这个例子。

```
function fruit(sweetness) {
  return sweetness * 3;
}const pear = fruit(1);
const strawberry = fruit(5);
const watermelon = fruit(3);
```

函数`fruit`能够被分配给变量。这是可能的，因为函数在 JavaScript 中是一个值。如果你不熟悉函数没有价值的语言，你会很困惑，但是不要担心。在 JavaScript 中，它们是一个值。

这意味着函数也可以分配给一个数组或一个对象。

```
const fruitsArr = [fruit(1), fruit(3), fruit(5)];
const fruitsObj = {
  pear: fruit(1),
  strawberry: fruit(5),
  watermelon: fruit(3)
};
```

或者可以将它们传递给另一个函数。

```
function strawberry(fruit) {
  return fruit(5); 
}strawberry(fruit);
```

因为 JavaScript 函数被认为是一个变量，所以它被称为一级对象。一级对象最强大的特性之一是一个函数也可以返回另一个函数。

```
function f(x) {
  return function(y) { ... }
}
```

# 那 HOF 是什么？

尽管 HOF 听起来像是一种新技术，但它不是。HOF，高阶函数，是以函数为自变量，返回函数的函数。

```
// HOF
function f(x) {
  // HOF
  return function(y) {
    return function(x) {}
  }
}
```

看起来很简单，不是吗？是的，确实如此。但这会带你去一个你一生中从未见过的魔法世界。在实际情况中，使用 HOF 最常见的情况可能是一些数组。

```
const numbers = [1, 2, 3, 4, 5, 6, 7];
const isGreaterThan = x => x > 4;numbers.filter(isGreaterThan); // [5, 6, 7]
```

`Array.prototype.filter`采用一个函数，该函数选取一些满足过滤条件的值。如果没有霍夫，你不得不这样做。

```
const filtered = [];
for (let i = 0; i < numbers.length; i += 1) {
  if (numbers[i] > 4) {
    filtered.push(numbers[i]);
  }
}
```

使用 HOF 看起来更好更干净！

# HOF 的真正力量——函数合成

为什么 HOF 在函数式编程中很重要，因为它允许你将函数组合在一起。既然 HOF 返回一个函数，这个函数可以返回一个函数，这个函数可以返回一个函数…，那么你可以做一个管道。

在很多函数式 JavaScript 库中，有一个函数叫做“compose”。名称可能与您使用的库不同，但核心概念是相同的。

```
f: ((w → x), (x → y), (y → z), …)
```

组合函数采用一个或多个函数。这个 compose 函数执行从左到右的函数合成。最左边的函数可以采用任何 arity。一旦最左边的函数返回值，下一个最左边的函数就获取结果并返回它的返回值，这个过程一直持续到最右边的函数返回结果。

```
const compose = (fn, ...fns) => (...v) => 
  fns.reduce((r, f) => f(r), fn.apply(this, v));
```

当然，真正的 compose 函数可能看起来更加复杂和困难，而且组织得也很好。我在代码块中附加的`compose`来自 [ramda.js](https://ramdajs.com/) ，这是一个很棒的函数式编程库。我已经写了一些硬代码。

```
const classyGreeting = (firstName, lastName) => 
  `The name's ${lastName}, ${firstName} ${lastName}`;
const toUpper = name => name.toUpperCase();const yellGreeting = compose(classyGreeting, toUpper);
yellGreeting('james', 'bond');
// THE NAMW'S BOND, JAMES BOND
```

Compose 需要两个函数，`classyGreeting`和`toUpper`。`classyGreeting`将两个参数组合成一个字符串，`toUpper`将所有字母转换成大写字母。

一旦函数被传递给 compose，第一个函数将被绑定到`fn`，其他函数将被绑定到`...fns`。如果只有一个函数被传递给 compose like `compose(toUpper)`，那么`fn`将是`toUpper`而`fns`将是一个空数组(`[]`)。一旦用空数组调用 reduce，就会抛出一个错误。

```
Uncaught TypeError: Reduce of empty array with no initial value
```

但是，如果您按如下方式设置初始值，它不会抛出错误。

```
fns.reduce(..., fn.apply(this, v));
```

那么，这个 reduce 函数的初始值就是 fn 的结果。如果你不明白`reduce`是如何工作的，请查阅[文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)。

但是这个看起来不是很像管子吗？函数式编程中的管道函数也接受函数和返回值，它像示例中的 compose 一样执行从左到右的合成。在函数式编程中，通常管道执行从左到右的函数组合，而 compose 执行从右到左的函数组合，这基本上是相同的。

# 咖喱和部分应用也是一种形式

FP 里还有一个概念叫库里。curry 函数是一个只接受一个参数的函数，它返回一个只接受一个参数的函数，并且返回一个只接受一个参数的函数…

假设你想把一个学生的分数加起来。

```
function sum(math, eng, geo) {
  return math + eng + geo;
}
```

但是，你可以这样写这个函数。

```
// ES5
function curriedSum(math) {
  return function(eng) {
    return function(geo) {
      return math + eng + geo;
    }
  }
}// ES6
const curriedSum = math => eng => geo => math + eng + geo;
```

这是部分应用。

```
// ES5
function partialSum(math) {
  return function(eng, geo) {
    return math + eng + geo;
  }
}// ES6
const partialSum = math => (eng, geo) => math + eng + geo;
```

这些函数给了你懒惰的优势——使用闭包计算值。

# 结论

HOF 是 JavaScript 中一个非常特殊的特性，它支持函数式编程，允许你在以后需要的时候计算值。在 React 中，这个概念被广泛用作一个组件返回一个新组件的更高阶组件的名称。

好了，伙计们，现在你们需要做的是在你们的项目中找到 HOF，并思考它为什么是 HOF。这将是一个很好的练习！

# 你可能想看看我在 FP 的其他帖子

*   [JavaScript 中的函数式编程和管道函数](https://medium.com/better-programming/functional-programming-and-the-pipe-function-in-javascript-c92833052057)
*   [函数式编程:curry vs . Partial Application](https://medium.com/better-programming/functional-programming-currying-vs-partial-application-53b8b05c73e3?source=your_stories_page---------------------------)

# 资源

*   [撰写— Ramda.js](https://ramdajs.com/docs/#compose)
*   [array . prototype . reduce—MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)