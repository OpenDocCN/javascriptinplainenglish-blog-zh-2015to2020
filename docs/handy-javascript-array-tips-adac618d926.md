# 方便的 JavaScript 数组技巧

> 原文：<https://javascript.plainenglish.io/handy-javascript-array-tips-adac618d926?source=collection_archive---------3----------------------->

![](img/f4d7ef6e630501f981e77ffa3927ba85.png)

Photo by [Pierre Bamin](https://unsplash.com/@bamin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 和其他编程语言一样，有许多方便的技巧，让我们可以更容易地编写程序。在这篇文章中，我们将看看如何做不同的涉及数组的事情，比如检查一个对象是否是一个数组和组合数组。

# 检查对象是否是数组

有多种方法可以检查 JavaScript 对象或原始值是否是数组。最新的检查方法是使用`Array.isArray`方法。它有一个参数，可以是任何对象或原始值，我们要检查它是否是一个数组。如果是数组，它返回`true`，否则返回`false`。但是对于`TypedArray` 对象，如`Int8Array`、`Uint8Array`、`Uint8ClampedArray`、`Int16Array`、`Uint16Array`、`Int32Array`、`Uint32Array`、`Float32Array`、`Float64Array`、`BigInt64Array`、`BigUint64Array`。它总是返回`false`。例如，如果我们编写以下代码:

```
console.log(Array.isArray([1, 2, 3]));
console.log(Array.isArray(0));
console.log(Array.isArray(null));
console.log(Array.isArray({
  a: 1
}));
console.log(Array.isArray(undefined));
console.log(Array.isArray(Infinity));
console.log(Array.isArray(new Uint8Array()));
```

我们从`console.log`语句中得到以下输出:

```
true
false
false
false
false
false
false
```

这是确定一个对象是否是数组的一个非常方便的方法。检查对象是否为数组的另一种方法是使用`instanceof`操作符。它通过检查`Array.prototype`是否在一个对象的原型链上来工作。使用`Array.isArray`时唯一失败但有效的情况是`instanceof`在跨帧检查对象时会失败，因为对象实例可能与使用`instanceof`操作符进行数组测试时使用的不同。我们可以在下面的代码中使用它:

```
console.log([1, 2, 3] instanceof Array);
console.log(0 instanceof Array);
console.log(null instanceof Array);
console.log({
    a: 1
  }
  instanceof Array);
console.log(undefined instanceof Array);
console.log(Infinity instanceof Array);
console.log(new Uint8Array() instanceof Array);
```

`console.log`的输出应该和之前的完全一样，因为我们没有在一个帧中放置任何对象。`Array.isArray`是最简单和最健壮的解决方案，因为大多数现代浏览器都内置了这种方法，而且它可以跨框架工作。

![](img/768cb7606ce84bf2e94eddea98e0740c.png)

Photo by [Edoardo Busti](https://unsplash.com/@phoedobus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 组合数组

使用现代 JavaScript，将数组组合成一个数组比以往任何时候都容易。数组对象有一个在数组上被调用的`concat`方法，它接受一个或多个参数，我们希望将哪一个或多个数组或对象组合到被调用的数组中。请注意，它也可以接受其他非数组值，我们希望将这些值添加到调用它的数组中。它用新的数组值返回一个新的数组实例，这样我们就可以将一系列对`concat`方法的调用链接起来，将更多的数组组合成一个数组。例如，我们可以为最基本的用例编写以下代码:

```
const arr1 = [1, 2, 3];
const arr2 = ['a', 'b', 'c'];
const combinedArr = arr1.concat(arr2);
console.log(combinedArr);
```

那么我们得到`combinedArr`的值将是`[1, 2, 3, “a”, “b”, “c”]`。我们还可以传入多个参数，每个参数都是进入`concat`方法的数组，如下面的代码所示:

```
const arr1 = [1, 2, 3];
const arr2 = ['a', 'b', 'c'];
const arr3 = ['1', '2', '3'];
const arr4 = ['d', 'e', 'f'];
const arr5 = ['g', 'h', 'i'];
const arr6 = [null, Infinity, NaN];
const arr7 = [10, 11, 12];
const arr8 = [{}, {
  a: 1
}, {
  b: 2
}];
const combinedArr = arr1.concat(arr2, arr3, arr4, arr5, arr6, arr7, arr8);
console.log(combinedArr);
```

然后我们从`console.log`输出得到以下输出:

```
[
  1,
  2,
  3,
  "a",
  "b",
  "c",
  "1",
  "2",
  "3",
  "d",
  "e",
  "f",
  "g",
  "h",
  "i",
  null,
  null,
  null,
  10,
  11,
  12,
  {},
  {
    "a": 1
  },
  {
    "b": 2
  }
]
```

正如我们所见，`concat`方法足够聪明，可以将多个数组合并到第一个数组中，并且我们将所有数组中的所有条目放在一个新数组中。从`concat`方法返回的数组没有引用原始数组。此外，它可以处理多种类型的数据。我们在里面传递什么都不重要，应该还是管用的。例如，如果我们有以下代码:

```
const arr1 = [1, 2, 3];
const arr2 = ['a', 'b', 'c'];
const arr3 = ['1', '2', '3'];
const arr4 = ['d', 'e', 'f'];
const arr5 = ['g', 'h', 'i'];
const arr6 = [null, Infinity, NaN];
const arr7 = [10, 11, 12];
const arr8 = [{}, {
  a: 1
}, {
  b: 2
}];
const combinedArr = arr1.concat(arr2, arr3, arr4, arr5, arr6, arr7, arr8, 1, 'a', {
  c: 3
});
```

然后，如果我们在`combinedArr`上运行`console.log`，我们会得到以下输出:

```
[
  1,
  2,
  3,
  "a",
  "b",
  "c",
  "1",
  "2",
  "3",
  "d",
  "e",
  "f",
  "g",
  "h",
  "i",
  null,
  null,
  null,
  10,
  11,
  12,
  {},
  {
    "a": 1
  },
  {
    "b": 2
  },
  1,
  "a",
  {
    "c": 3
  }
]
```

这非常有用，因为不必担心我们传入的对象的数据类型，也不必担心我们向`concat`方法传递了多少参数。我们传入的参数越多越好。由于`concat`方法返回一个新的数组，其中的值组合成一个数组，我们还可以链接`concat`方法的调用，将多个内容组合成一个数组，如下面的代码所示:

```
const arr1 = [1, 2, 3];
const arr2 = ['a', 'b', 'c'];
const arr3 = ['1', '2', '3'];
const arr4 = ['d', 'e', 'f'];
const arr5 = ['g', 'h', 'i'];
const arr6 = [null, Infinity, NaN];
const arr7 = [10, 11, 12];
const arr8 = [{}, {
  a: 1
}, {
  b: 2
}];
const combinedArr = arr1
  .concat(arr2)
  .concat(arr3)
  .concat(arr4)
  .concat(arr5)
  .concat(arr6)
  .concat(arr7)
  .concat(arr8)
  .concat(1)
  .concat('a', {
    c: 3
  });
```

然后，当我们在`combinedArr`上运行`console.log`时，我们应该得到以下结果:

```
[
  1,
  2,
  3,
  "a",
  "b",
  "c",
  "1",
  "2",
  "3",
  "d",
  "e",
  "f",
  "g",
  "h",
  "i",
  null,
  null,
  null,
  10,
  11,
  12,
  {},
  {
    "a": 1
  },
  {
    "b": 2
  },
  1,
  "a",
  {
    "c": 3
  }
]
```

在 ES6 中，我们有 spread 运算符，通过将数组的值分布到另一个数组中，我们可以使用它将数组合并为一个数组，并且我们可以在每个数组分布后对一个数组中的所有数组进行同样的操作，数组之间用逗号分隔。spread 还适用于所有类似数组的对象，如`arguments`、集合、地图或任何具有`Symbol.iterator`方法的对象，这些方法返回一个生成器，这样我们就可以用`for...of`循环遍历对象中的项目。要使用 spread 操作符将数组和对象组合成一个数组，我们可以编写如下代码:

```
const arr1 = [1, 2, 3];
const arr2 = ['a', 'b', 'c'];
const arr3 = ['1', '2', '3'];
const combinedArr = [...arr1, ...arr2, ...arr3];
```

然后我们得到:

```
[
  1,
  2,
  3,
  "a",
  "b",
  "c",
  "1",
  "2",
  "3"
]
```

当我们在`combinedArr`上运行`console.log`时。它也像`concat`方法一样处理不同数组中的重叠值。例如，我们可以编写以下代码:

```
const arr1 = [1, 2, 3];
const arr2 = ['a', 'b', 'c'];
const arr3 = [1, '2', '3'];
const combinedArr = [...arr1, ...arr2, ...arr3];
```

得到`combinedArr`的值是:

```
[
  1,
  2,
  3,
  "a",
  "b",
  "c",
  1,
  "2",
  "3"
]
```

正如我们所看到的，我们在`arr1`和`arr3`中都有值`1`，但是它们都变成了`combinedArr`，这很好。我们还可以在 spread 操作之前、之后或之间将对象放入新数组，如以下代码所示:

```
const arr1 = [1, 2, 3];
const arr2 = ['a', 'b', 'c'];
const arr3 = [1, '2', '3'];
const combinedArr = ['c', ...arr1, 'c', ...arr2, 'c', ...arr3];
```

然后我们得到`combinedArr`的如下值:

```
[
  "c",
  1,
  2,
  3,
  "c",
  "a",
  "b",
  "c",
  "c",
  1,
  "2",
  "3"
]
```

这意味着`concat`方法的特性可以很容易地被 spread 操作符复制，我们不必传入一长串参数或者将`concat`方法的多个调用链接在一起，将数组和其他类型的对象组合成一个数组。

有多种方法可以检查 JavaScript 对象或原始值是否是数组。最新的检查方法是使用`Array.isArray`方法，但是我们也可以使用旧的`instanceof`操作符来检查一个对象是否是一个数组。`Array.isArray`跨帧工作，因此比`instanceof`操作符更健壮。使用现代 JavaScript，将数组组合成一个数组比以往任何时候都容易。数组对象有一个`concat`方法，它在数组上被调用，并接受一个或多个参数，我们希望将哪一个或多个数组或对象组合到它被调用的数组中。请注意，它也可以接受其他非数组值，我们希望将这些值添加到调用它的数组中。它返回一个新的数组实例，其中包含新数组中的所有组合值。