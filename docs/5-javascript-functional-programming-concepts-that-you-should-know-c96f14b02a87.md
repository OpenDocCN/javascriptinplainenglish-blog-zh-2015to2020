# 你应该知道的 5 个 JavaScript 函数式编程概念

> 原文：<https://javascript.plainenglish.io/5-javascript-functional-programming-concepts-that-you-should-know-c96f14b02a87?source=collection_archive---------5----------------------->

## 带有实际例子的函数式编程概念

![](img/7078a52f6a366a02a410970fd5d99d2a.png)

Photo by [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是函数式编程？

函数式编程是一种基于在程序中使用函数的编码风格。它是一种基于功能评估的软件开发方法。它将程序分成小的、可测试的部分。你可以用多种方式组合基本功能来构建越来越复杂的程序。

函数式编程遵循几个核心原则，其中一个原则是函数独立于程序的状态或全局变量。它们依赖于传递给它们的参数。函数式编程的另一个原则是函数在程序中的副作用最小。除此之外，他们试图限制对程序状态的任何更改，避免对保存数据的全局对象的更改。

在本文中，我们将探索 JavaScript 中一些有用的函数式编程概念。让我们开始吧。

![](img/a66bcd345461e3a0c372dba7d49395c0.png)

Image Created With❤️️ By Mehdi Aoussiad.

# 1.纯函数

纯函数是这样一种函数，它对于输入返回相同的输出，而不修改超出其范围的任何内容。看看下面的例子:

**第一版:**

```
let age = 19

function getMyAge() {
  console.log(`I'm ${age} years old.`)
}

getMyAge(age) //Returns: I'm 19 years old.
age = 20
getMyAge(age) //Returns: I'm 20 years old.
```

**第二版:**

```
function getMyAge(age) {
  return `I'm ${age} years old.`
}

getMyAge(19) //Returns: I'm 19 years old.
getMyAge(20) //Returns: I'm 20 years old.
```

在第一个版本中，函数在你的范围之外寻找一个变量，以某种方式改变世界。在这种情况下，输出，理想的是只返回值，如果我们调用函数，用相同的参数(甚至没有参数)，我们得到一个不同的值。在一个纯函数中，这种情况不会发生，因为它只依赖于它的输入和参数。

# 2.副作用

副作用是在计算过程中发生的与外界的任何相互作用，这在纯函数中是不会发生的。我们的代码可以更加可预测，因为结果只取决于他们的输入。如果我们知道我们的函数看起来如何，以及它接收哪些输入，我们就可以预测结果。看看下面的例子:

```
var double = function(value){
    return value * 2;
}var initialValue = 30;
double(initialValue); // 60initialValue; // Still 30.
```

# 3.易变性

突变是关于事物是可变的。在函数式编程中，不鼓励可变性。当我们有不可变的数据时，它的状态在你创建它之后不能改变。如果你需要改变什么，你就必须创造新的价值。

**易变的例子:**

```
function changeFirstElem(array) {
  array[0] = 'Lose yourself to dance'
}

const daftPunkPopSongs = ['Instant Crush', 'Get Lucky', 'One More Time'];
changeFirstElem(daftPunkPopSongs);
console.log(daftPunkPopSongs); // ["Lose yourself to dance", "Get Lucky", "One More Time"].
```

**不可变的例子:**

```
function changeFirstElem(array) {
  const modifiedArray = ['Lose yourself to dance', ...array];
  return modifiedArray;
}

const daftPunkPopSongs = ['Instant Crush', 'Get Lucky', 'One More Time'];
const modifiedArray = changeFirstElem(daftPunkPopSongs);Console.log(daftPunkPopSongs); // Returns: ['Instant Crush', 'Get Lucky', 'One More Time'].
```

这太棒了。在第二个例子中，我们让事情变得更安全，更难在代码中找到 bug。如果输出是错误的，我们肯定问题出在我们的函数上，而不是因为随机的交互。这将使调试我们的代码更加容易。

# 4.高阶函数

任何以另一个函数为自变量的函数都称为**高阶函数。JavaScript 提供了一些有用的高阶函数来以一种简单的方式操作数据。**

以下是一些你应该知道的重要功能:

**map** 方法有助于传递一个函数来转换数组中的每一项。

```
const numbers = [1, 2, 3];
const doubles = numbers.map(num => num * 2) //[2, 4, 6]
```

**过滤器**接收一个数据集合，您可以传递一个返回集合子集的条件函数。

```
const numbers = [1, 2, 3];
const isGreaterThanOne = numbers.filter(num => num > 1) //[2, 3]
```

**reduce** 方法采用一个带有两个参数(累加器和项)的函数。我们还可以使用 reduce 方法返回所有数组项的总数，如下例所示。

```
const numbers = [1, 2, 3];
const mySum = numbers.reduce((accumulator, num) => accumulator + num) // returns: 6.
```

# 5.固化和局部应用

Currying 一个函数意味着把它从一个有几个参数的函数转换成几个只有一个参数的函数。看看下面的例子:

```
//Un-curried function
function unCurried(x, y) {
  return x + y;
}

//Curried function
function curried(x) {
  return function(y) {
    return x + y;
  }
}
//Alternative using ES6
const curried = x => y => x + y

curried(1)(2) // Returns 3
```

如果你不能一次给一个函数提供所有的参数，Currying 在你的程序中是有用的。

现在我们来看一下**部分应用**，可以描述为一次对一个函数应用几个参数，返回另一个应用到更多参数的函数。这里有一个例子:

```
//Impartial function
function impartial(x, y, z) {
  return x + y + z;
}
var partialFn = impartial.bind(this, 1, 2);
partialFn(10); // Returns 13
```

# 结论

函数式编程是一个好习惯。它使您的代码易于管理，并使您免于偷偷摸摸的错误。当使用 JavaScript 或其他编程语言时，这是一种强大的方法。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-fun-apis-for-your-next-javascript-projects-1834626864c) [## 为您的下一个 JavaScript 项目准备的 5 个有趣的 API

### 您可以在 JavaScript 项目中使用的 5 个有用的 API

medium.com](https://medium.com/javascript-in-plain-english/5-fun-apis-for-your-next-javascript-projects-1834626864c)