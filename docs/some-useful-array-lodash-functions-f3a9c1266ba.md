# 一些有用的数组 Lodash 函数

> 原文：<https://javascript.plainenglish.io/some-useful-array-lodash-functions-f3a9c1266ba?source=collection_archive---------8----------------------->

![](img/5a016f80b407fe25e9a3359958ded7fd.png)

Photo by [Grant Durr](https://unsplash.com/@blizzard88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个实用程序库，有很多操作对象的方法。它有我们一直在使用的东西，也有我们不常使用或不想使用的东西。

在本文中，我们将看看 Lodash 的一些数组方法，包括`chunk`、`compact`、`concat`、`difference`、`differenceBy`和`differenceWith`方法。

# 矮胖的人或物

`chunk`方法让我们将数组条目分成给定大小的数组，并返回它。如果条目不能被平均分割，那么最后的块将是剩余的元素。

它有两个参数，即要分割的数组和分割数组中每个条目的块大小。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const array = [1, 2, 3, 4, 5, 6, 7];
const result = _.chunk(array, 2);
```

然后我们得到:

```
[
  [
    1,
    2
  ],
  [
    3,
    4
  ],
  [
    5,
    6
  ],
  [
    7
  ]
]
```

分配给`result`。

# `compact`

`compact`方法创建一个移除了所有 falsy 值的数组并返回它。值`false`、`null`、`0`、`""`、`undefined`和`NaN`为假值。

它需要一个参数，也就是要删除这些值的数组。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const array = [0, 1, 2, 3, null, false, undefined];
const result = _.compact(array);
```

然后我们得到:

```
[
  1,
  2,
  3
]
```

分配给`result`。

# `concat`

`concat`方法创建一个添加了附加值的新数组并返回它。它需要两个或更多的参数。第一个是要添加值的数组，其余的是我们要添加到数组中的值。

我们可以如下使用它:

```
import * as _ from "lodash";
const array = [1, 2, 3];
const result = _.concat(array, 2, [3], [[4]]);
```

然后我们得到:

```
[
  1,
  2,
  3,
  2,
  3,
  [
    4
  ]
]
```

分配给`result`。

# `difference`

`difference`从原始数组的副本中删除 values 数组中给定的值，并返回没有指定值的新数组。

使用`SameValueZero`比较，与`===`运算符相同，除了`NaN`与自身相同，`-0`与`+0`被认为相同。

它有两个参数，第一个参数是要从中删除项目的数组，第二个参数是要从第一个参数中删除的数组值

例如，我们可以这样使用它:

```
import * as _ from "lodash";
const array = [1, 2, 3];
const result = _.concat(array, 2, [3], [[4]]);
```

然后我们得到:

```
[
  1
]
```

分配给`result`。

![](img/34a391ae9c1be3bac3c7163a383e6285.png)

Photo by [Bogdan Todoran](https://unsplash.com/@todoranb_26?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `differenceBy`

我们可以使用`differenceBy`方法从数组中移除元素，并返回移除了元素的新数组。

它需要 3 个参数，这是要从中移除项目的数组。第二个参数包含要从第一个数组中排除的值。最后一个参数是一个函数，我们可以用它来比较项目或一个带有要比较的值的属性名的字符串。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const result = _.differenceBy(
  [{ name: "Joe" }, { name: "Jane" }],
  [{ name: "Joe" }],
  "name"
);
```

然后我们回来了:

```
[
  {
    "name": "Jane"
  }
]
```

哪个分配给`result`。

我们还可以传入一个比较器函数，而不是我们希望作为第三个参数检查的属性值的字符串键:

```
import * as _ from "lodash";
const fn = a => a.id.toString();
const result = _.differenceBy([{ id: 1 }, { id: 2 }], [{ id: 1 }], fn);
```

`fn`函数有`a`参数，该参数有在第一个数组中循环的对象，所以我们可以用我们喜欢的方式操作它，将它映射到新的值。

然后我们回来了:

```
[
  {
    "id": 2
  }
]
```

并被分配到`result`。

# `differenceWith`

像`differenceBy`一样，`differenceWith`做同样的事情，但是它只接受一个`comparator`函数作为第三个参数，而不是同时接受`comparator`或一个包含我们想要比较的键名的字符串。

`comparator`有两个参数，分别是自变量中第一个和第二个数组的元素。

如果与第一个数组中的项匹配，它还返回一个新数组，其中第二个数组中的项已被删除。

例如，我们可以这样写:

```
import * as _ from "lodash";
const comparator = (a, b) => a.id.toString() === b.id.toString();
const result = _.differenceWith(
  [{ id: 1 }, { id: 2 }],
  [{ id: 1 }],
  comparator
);
```

上面的代码将比较第一个数组中的项目和第二个数组中的项目，并根据从`comparator`函数返回的条件，将不在第二个数组中的项目返回到新数组中。

然后我们回来了:

```
[
  {
    "id": 2
  }
]
```

并分配给`result`。

Lodash 中有一些有用的数组方法可以使数组操作变得更容易。

`chunk`方法让我们将数组条目分成给定大小的数组，并返回它。

`compact`方法创建一个删除了所有 falsy 值的数组并返回它。

`concat`方法创建一个添加了附加值的新数组并返回它。

`difference`从原始数组中删除第二个参数的值数组中给出的值。类似地，`differenceBy`和`differenceWith`让我们以不同的方式比较数组，并以不同的方式操作它们，以从原始数组中删除 values 数组中给出的元素。