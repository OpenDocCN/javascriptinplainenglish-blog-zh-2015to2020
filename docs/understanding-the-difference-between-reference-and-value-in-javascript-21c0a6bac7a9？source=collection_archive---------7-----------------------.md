# 理解 JavaScript 中引用和值的区别

> 原文：<https://javascript.plainenglish.io/understanding-the-difference-between-reference-and-value-in-javascript-21c0a6bac7a9?source=collection_archive---------7----------------------->

![](img/ec9d86b41663af308fd18358f3507b32.png)

Photo by [davisco](https://unsplash.com/@codytdavis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

"对象通过引用传递，而不是通过值传递."

你以前听过这句话，但却很难理解它的意思吗？这是一个经常导致新开发人员在第一次学习 JavaScript 时犯错误的概念。

在本文中，我们将通过几个例子来更好地理解变量是如何被处理的，以及“引用”和“值”之间的区别。

# 传递原语

JavaScript 中的原始数据类型类似于`number`、`string`、`boolean`或`undefined`。还有其他原语，但这些是最常见的。

**原语通过值**传递。为了理解这意味着什么，让我们看一个简单的例子:

```
const myNumber = 10;const addOne = x => x + 1;const anotherNumber = addOne(myNumber);console.log(myNumber);
console.log(anotherNumber);
```

在这个例子中，我们有一个值为`10`的变量`myNumber`。我们有一个函数`addOne`，它接受一个参数并返回这个参数加上`1`。然后，我们使用`myNumber`变量作为参数调用`addOne`函数，并将结果保存到另一个名为`anotherNumber`的变量。最后，我们将两个变量的值记录到控制台。

所以，问题是:记录了什么？

如果你回答了`10`和`11`，你就答对了。因为数字是通过值传递的，`myNumber`的值被传递给函数，但是当数字增加时，`myNumber`变量不受影响。

# 比较原语

所以现在我们知道原语是通过值传递的。但是当他们被比较的时候呢？为了回答这个问题，我们来看另一个例子:

```
const x = 5;
const y = 5;console.log(x === y);
```

我们有两个变量，`x`和`y`，它们的值都是`5`。当我们在控制台记录一个严格相等的检查时，我们得到了什么？

如果你回答了`true`，你就答对了。这是因为**图元也通过值**进行比较，并且`5`等于`5`。

# 传递物体

那么，JavaScript 中非原语的数据类型呢？例如，`objects`不是原语，`arrays`也不是(它们实际上只是对象，秘密地)。

**对象通过引用传递**。为了理解这意味着什么，让我们看一个简单的例子:

```
const someNumbers = [1, 2, 3];const addNumberToArray = arr => {
  arr.push(100);
  return arr;
}const otherNumbers = addNumberToArray(someNumbers);console.log(someNumbers);
console.log(otherNumbers);
```

在这个例子中，我们有一个变量`someNumbers`，它是一个包含三个元素的数组。我们有一个函数`addNumberToArray`，它接受一个参数(一个数组)，将值`100`推入数组，然后返回数组。然后我们使用`someNumbers`变量作为参数调用`addNumberToArray`函数，并将结果保存到另一个名为`otherNumbers`的变量中。最后，我们将两个变量的值记录到控制台。

所以，问题是:记录了什么？

如果你回答了`[1, 2, 3, 100]`和`[1, 2, 3, 100]`，你就答对了。

哦不！我们无意中修改了传递给函数的输入数组！

因为对象是通过引用传递的，所以对`someNumbers`的引用被传递给函数。因此，当值`100`被推送到数组时，该值也被推送到`someNumbers`表示的同一个数组中。

如果您想确保不在这样的函数中修改原始数组，那么有必要使用`concat`方法或 ES6 `spread`操作符将值`100`推入输入数组的副本中。例如:

```
const someNumbers = [1, 2, 3];const addNumberToArray = arr => [...arr, 100];const otherNumbers = addNumberToArray(someNumbers);console.log(someNumbers);
console.log(otherNumbers);
```

现在，当我们将这两个变量记录到控制台时，我们将看到`[1, 2, 3]`和`[1, 2, 3, 100]`被记录。好多了。

# 比较对象

所以现在我们知道对象是通过引用传递的。但是当他们被比较的时候呢？为了回答这个问题，我们来看另一个例子:

```
const object1 = { someKey: 'someValue' }
const object2 = { someKey: 'someValue' }console.log(object1 === object2);
```

我们有两个变量，`object1`和`object2`，这两个变量都是只有一个属性的对象。键是`someKey`，值是`someValue`。当我们在控制台记录一个严格相等的检查时，我们得到了什么？

如果你回答了`false`，你就对了。这是因为**对象也通过引用**进行比较。即使这两个对象的值相同，它们也不是同一个对象。这是保存在两个独立变量中的两个独立对象，所以它们的引用是不同的。

如果您想要快速的完整性检查，您也可以检查每个对象是否等于它自己，就像这样:

```
console.log(object1 === object1);
console.log(object2 === object2);
```

控制台的这两个日志都是`true`,因为在每种情况下，你都将一个对象与其自身进行比较，这是同一个引用。

如果你真的想检查`object1`和`object2`是否有相同的键和值，你需要写一个实用方法来循环对象的键和值，并确保它们都是相同的。或者，你可以使用像`lodash`这样的库中的帮助方法，它为你实现了这个功能。

# 结论

原语通过值进行传递和比较。对象通过引用进行传递和比较。理解其中的区别将会为你省去很多调试代码的麻烦！

# 更新

我的心理模型是“原语通过值传递；对象通过引用传递”多年来一直为我服务，它有助于理解预期的行为，但似乎我一直在使用不正确的术语来解释实际发生的事情。

解释这一概念的更正确的方式是:

> *原语通过值传递。对象通过“引用的副本”传递。*
> 
> *或者，JavaScript 中的参数总是通过值传递。但一个对象的“值”才是参照物。*

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**