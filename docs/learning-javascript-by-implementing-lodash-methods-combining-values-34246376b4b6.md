# 通过实现 Lodash 方法学习 JavaScript 组合值

> 原文：<https://javascript.plainenglish.io/learning-javascript-by-implementing-lodash-methods-combining-values-34246376b4b6?source=collection_archive---------7----------------------->

![](img/74c755af6b548999c6931f3156e5492d.png)

Photo by [Edoardo Busti](https://unsplash.com/@phoedobus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将研究如何实现 Lodash 方法来处理集合中的组合项。

# `reduce`

Lodash `reduce`方法通过调用`iteratee`函数将集合中的值组合成一个值，该函数采用一个`accumulator`参数和到目前为止组合的值，第二个参数是要为数组版本组合的项。

对象版本接受具有累积值的`result`、具有对象值的`value`和具有正在处理的对象的键的`key`。

在运行`iteratee`之前，它还使用一个可选的`accumulator`对象和`accumulator`参数的初始值。

然后我们可以编写下面的代码:

```
const reduce = (collection, iteratee, accumulator) => {
  if (Array.isArray(collection)) {
    return collection.reduce(iteratee, accumulator)
  } else {
    let reduced = accumulator;
    for (const key of Object.keys(collection)) {
      iteratee(reduced, collection[key], key)
    }
    return reduced;
  }
}
```

在上面的代码中，我们检查`collection`是否是一个数组。如果是，那么我们在数组实例上调用`reduce`，用`iteratee`函数作为回调，用`accumulator`作为初始值。

否则，我们用`accumulator`作为初始值创建一个新的`reduced`变量。

然后我们用`iteratee`遍历`collection`对象的键，它将`reduced`值、给定键的值和键本身作为参数。

然后我们返回`reduced`对象，它有组合的结果。

那么我们可以这样称呼它:

```
const result = reduce({
  'a': 1,
  'b': 2,
  'c': 1
}, (result, value, key) => {
  (result[value] || (result[value] = [])).push(key);
  return result;
}, {});
```

在上面的代码中，我们用一个对象和一个带有`result`、`value`和`key`参数的`iteratee`函数调用了我们自己的`reduce`方法。然后在函数中，我们通过用`value`填充结果对象的键来组合这些值，并将这些键推入数组。

然后我们得到:

```
{
  "1": [
    "a",
    "c"
  ],
  "2": [
    "b"
  ]
}
```

作为`result`的值。

我们也可以用一个数组调用我们的`reduce`函数，如下所示:

```
const result = reduce([1, 2], (sum, n) => {
  return sum + n;
}, 0);
```

然后我们得到`result`是 3。

![](img/6a1f033970ee724612b31130385e9bcc.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `reduceRight`

`reduceRight`方法类似于`reduce`，除了从结束到开始组合数值。

我们可以用类似于`reduce`的方式实现它，如下所示:

```
const reduceRight = (collection, iteratee, accumulator) => {
  if (Array.isArray(collection)) {
    return collection.reverse().reduce(iteratee, accumulator)
  } else {
    let reduced = accumulator;
    const keys = Object.keys(collection)
    for (let i = keys.length - 1; i >= 0; i--) {
      iteratee(reduced, collection[keys[i]], keys[i])
    }
    return reduced;
  }
}
```

在上面的代码中，在数组的情况下，我们通过使用与`reduce`实现相同的参数调用`reduce`来反转数组。

否则，我们以相反的顺序遍历对象的键。在循环中，我们对每个项目调用`iteratee`。`iteraee`函数采用与我们传递给`reduce`的参数相同的参数。

我们可以如下调用`reduceRight`:

```
const result = reduceRight({
  'a': 1,
  'b': 2,
  'c': 1
}, (result, value, key) => {
  (result[value] || (result[value] = [])).push(key);
  return result;
}, {});
```

那么我们得到`result`是:

```
{
  "1": [
    "c",
    "a"
  ],
  "2": [
    "b"
  ]
}
```

因为我们以逆序遍历对象条目。

我们可以用一个数组调用`reduceRight`，如下所示:

```
const result = reduceRight([1, 2], (sum, n) => {
  return sum + n;
}, 0);
```

那么我们得到的结果和`reduce`一样，都是 3。

# 结论

`reduce`和`reducerRight`方法可以用数组的现有数组方法来完成。我们可以用`iteratee`函数和`accumulator`调用数组实例上的`reduce`。

对于对象来说，就更复杂了。我们必须通过获取键来遍历这些项，并将键和值传递给`iteratee`函数，该函数将`result`作为第一个参数。第二个参数是`value`，第三个是`key`。

## 来自简明英语团队的说明

简明英语刚刚推出了一个 YouTube 频道！我们希望您现在就通过 [**订阅来支持我们！**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)