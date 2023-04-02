# 更多 JavaScript 数组技巧——移除虚假值、随机数组条目和反转数组

> 原文：<https://javascript.plainenglish.io/more-javascript-array-tips-removing-falsy-values-get-random-array-entry-and-reversing-arrays-759473ba25a4?source=collection_archive---------2----------------------->

![](img/061bd8beea270cda7b7fc59a9f0f078e.png)

Photo by [The Lucky Neko](https://unsplash.com/@theluckyneko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 和其他编程语言一样，有许多方便的技巧，让我们更容易编写程序。在这篇文章中，我们将看看如何做不同的事情，涉及到数组，如删除 falsy 值，从一个数组中获得随机值，以及反转数组。

# 删除虚假值

在 JavaScript 中，falsy 值包括 0、空字符串、`null`、`undefined`、`false`和`NaN`。我们在处理数组时经常会遇到这些值，它们可能会导致错误。

像`null`、`undefined`和`NaN`这样的值经常是意想不到的，如果我们在对它们做任何事情之前从一个数组中消除它们会更容易。

我们可以用一些方法来实现这一点，包括使用数组可用的`filter`方法。`filter`方法接受一个回调函数。

回调函数有两个参数，第一个是数组条目，第二个是被`filter`方法迭代的数组的索引。`filter`方法过滤器让我们定义我们想要保留的内容，并返回一个新数组，根据我们返回的布尔表达式保存条目。

任何被定义为至少接受值并返回布尔值的函数都可以被用作回调函数，所以我们可以使用`filter`方法过滤掉的一种方式是使用`Boolean`工厂函数，该函数在其第一个参数中接受任何对象，如果是 true，则返回`true`，如果是 falsy，则返回`false`。如果`true`从回调函数返回，那么该项目将被保留。

否则，它将丢弃它返回的数组中的值。例如，我们可以通过直接传入`filter`方法来使用`Boolean`函数，就像我们在下面的代码中做的那样:

```
let arr = [0, 'yellow', '', NaN, 1, true, undefined, 'orange', false];
arr = arr.filter(Boolean);
console.log(arr);
```

然后我们从`console.log`得到以下输出:

```
["yellow", 1, true, "orange"]
```

我们还可以使用感叹号运算符两次，将 the 值强制转换为`true`，将 falsy 值强制转换为`false`。为此，我们只需在 value 前面加两次感叹号，这样我们就可以使用类似下面代码中的操作符来过滤掉 falsy 值:

```
let arr = [0, 'yellow', '', NaN, 1, true, undefined, 'orange', false];
arr = arr.filter(a => !!a);
console.log(arr);
```

如果我们运行上面的代码，我们应该得到和以前一样的东西。同样，我们可以传递一个从`Boolean`函数返回值的函数，而不是将`Boolean`函数直接传递给`filter`方法，以使表达式更加清晰:

```
let arr = [0, 'yellow', '', NaN, 1, true, undefined, 'orange', false];
arr = arr.filter(a => Boolean(a));
console.log(arr);
```

如果我们运行上面的代码，应该会得到和以前一样的结果。

# 从数组中随机获取一个值

为了从一个数组中获取一个随机值，我们可以使用`Math.random`方法来获取一个随机索引以及一些其他代码。

我们比仅仅调用`Math.random`方法需要更多的代码，因为它只返回一个介于 0 和 1 之间的随机数，这意味着它返回一个介于 0 和 1 之间的十进制数，这是我们不想要的。

这意味着它必须乘以数组的长度才能得到一个介于 0 和数组长度之间的值。然后，我们需要对该结果调用`Math.floor`，将其向下舍入到最接近的整数。

我们可以像下面的代码一样使用`Math.random`方法:

```
let arr = ['yellow', 'orange', 'blue', 'purple', 'green'];
const chosenValue = arr[(Math.floor(Math.random() * (arr.length)))]
console.log(chosenValue);
```

上面的代码通过运行`Math.random() * (arr.length)`生成索引，其范围从 0 到数组的长度。然后，我们使用`Math.floor`方法将其向下舍入，该方法采用我们想要向下舍入的数字，这样我们可以将该数字向下舍入到最接近的整数，因为我们不想要非整数。

然后我们可以把它传递到括号中，得到生成的索引。

![](img/e8f86f74bf1e67c1553adca848a61c44.png)

Photo by [Roberto Nickson](https://unsplash.com/@rpnickson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 反转数组

我们可以通过使用 JavaScript 数组对象附带的`reverse`方法来反转数组。它不接受任何参数，就地反转一个数组。例如，我们可以在下面的代码中使用它:

```
let arr = ['yellow', 'orange', 'blue', 'purple', 'green'];
arr.reverse();
console.log(arr);
```

然后我们得到:

```
["green", "purple", "blue", "orange", "yellow"]
```

从上面的`console.log`输出。除了对象之外，`reverse`方法还可以处理任何基本对象。例如，如果我们有:

```
let arr = [{
  a: 1
}, {
  b: 2
}, {
  c: 3
}];
arr.reverse();
console.log(arr);
```

然后我们从`console.log`得到以下输出:

```
[
  {
    "c": 3
  },
  {
    "b": 2
  },
  {
    "a": 1
  }
]
```

`reverse`方法也适用于任何有数字键的对象。例如，如果我们有:

```
let arrLikeObj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
}
Array.prototype.reverse.call(arrLikeObj);
console.log(arrLikeObj);
```

然后我们从`console.log`得到以下输出:

```
{
  "0": "c",
  "1": "b",
  "2": "a",
  "length": 3
}
```

只要对象有从 0 开始并从 0 开始递增 1 的数字键和一个`length`属性，上面的代码就能工作。我们也可以使用`apply`方法来做同样的事情，如下面的代码所示:

```
let arrLikeObj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
}
Array.prototype.reverse.apply(arrLikeObj);
console.log(JSON.stringify(arrLikeObj, (key, value) => value, 2));
```

然后，我们再次从`console.log`获得以下输出:

```
{
  "0": "c",
  "1": "b",
  "2": "a",
  "length": 3
}
```

# 结论

在 JavaScript 中，falsy 值包括 0、空字符串、`null`、`undefined`、`false`和`NaN`。我们在处理数组时经常会遇到这些值，它们可能会导致错误。像`null`、`undefined`和`NaN`这样的值经常是意想不到的，如果我们在对它们做任何事情之前从一个数组中消除它们会更容易。

我们可以通过一些方法来实现这一点，包括使用数组可用的`filter`方法。

我们可以使用`Boolean`函数或者应用感叹号两次来将一个对象转换成一个布尔值。要反转一个数组，我们可以使用调用数组上的`reverse`方法来反转它。

`reverse`方法也适用于任何具有从 0 开始递增 1 的数字键和一个`length`属性的对象。我们可以通过使用`Math.random`乘以数组的长度和`Math.floor`方法从数组中获得一个随机条目。