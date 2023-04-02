# 函数式 JavaScript —部分应用和组合

> 原文：<https://javascript.plainenglish.io/functional-javascript-partial-application-and-composition-d4accd96d8b6?source=collection_archive---------8----------------------->

![](img/0ba1a18d58cf9ec09a6433777154a4a8.png)

Photo by [Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将了解如何使用 JavaScript 来部分应用和组合函数。

# 部分应用

部分应用是指我们只应用函数中预期的一些参数。

例如，如果我们有一个带有 3 个参数的函数:

```
const add = (x, y, z) => x + y + z;
```

然后，我们可以创建一个函数，该函数的`add`函数通过编写以下内容来部分应用参数:

```
const add1 = (y, z) => add(1, y, z);
```

然后我们可以通过写来使用它:

```
const sum = add1(2, 3);
```

我们得到 6。

概括起来，我们可以这样写:

```
const partial = function(fn, ...partialArgs) {
  let args = partialArgs;
  return function(...fullArguments) {
    return fn(...[...partialArgs, ...fullArguments]);
  };
};
```

我们通过将`partialArgs`和`fullArguments`作为参数传入`fn`函数来返回一个函数，从而创建自己的`partial`函数。

然后我们可以通过写来使用它:

```
const sum = partial(add, 1)(2, 3);
```

而`sum`又是 6。

# Currying 与部分应用

每当我们需要将接受多个参数的函数转换为接受一个参数的多个函数时，Currying 就很有用。

我们需要将一个接受多个参数的函数转换成一个只接受一个参数的回调函数`map`的情况就是一个例子。

当我们需要将一个或多个参数应用于一个函数并返回一个应用了参数的函数时，函数的部分应用非常有用。

# 作文

组合是我们链接多个函数调用来返回我们想要的结果。

我们有很多做一件事的函数，我们把它们组合成一个，这样就能得到我们想要的结果。

例如，我们可以通过编写以下代码来组合数组`map`和`filter`方法:

```
const arr = [1, 2, 3]
  .filter(a => a % 2 === 1)
  .map(a => a ** 2);
```

而`arr`就是`[1, 9]`。

我们调用`filter`来返回一个只有奇数的数组。

然后我们调用`map`对每个奇数求平方。

# 撰写功能

我们可以将组合函数概括为:

```
const compose = (fn1, fn2) =>
  (c) => fn1(fn2(c))
```

我们的函数将两个函数作为参数，然后我们返回一个函数，一个接一个地调用。

首先调用`fn2`，然后对`fn2`返回的结果调用`fn1`。

然后我们可以通过写来使用它:

```
let number = compose(Math.round, parseFloat)('10.1')
```

我们用`Math.round`和`parseFloat`调用`compose`。

首先用`'10.1'`调用`parseFloat`，然后对返回的结果调用`Math.round`。

那么`number`就是 10。

# 组合许多功能

我们可以通过使用数组`reduce`方法创建一个通用版本的`compose`函数。

例如，我们可以写:

```
const compose = (...fns) =>
  (value) =>
  fns.reverse().reduce((acc, fn) => fn(acc), value)
```

我们创建了一个函数，它将函数数组`fns`作为参数。

然后我们返回一个函数，它取一个值作为初始值，并对它调用`reduce`，用返回的结果调用数组中的每个函数。

`acc`是目前调用函数返回的结果，`fn`是函数。

然后我们可以通过书写来使用它:

```
let splitIntoSpaces = (str) => str.split(" ");
let count = (array) => array.length;
const countWords = compose(count, splitIntoSpaces)('foo bar baz');
```

我们用`splitIntoSpaces`函数按空格分割字符串。

并且我们用`count`函数得到分裂字符串数组的长度。

然后我们用`compose`把它们组合在一起。

一旦我们用一个字符串调用返回的函数，我们就会得到用空格分隔的数字单词。

所以`countWords`是 3。

![](img/929618820aa52d0c6b933f211e8759aa.png)

Photo by [Finn Gerkens](https://unsplash.com/@finnsn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 JavaScript 部分应用和组合函数。

部分应用允许我们调用应用了一些参数的函数。

组合让我们可以在一个链中调用多个函数。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**