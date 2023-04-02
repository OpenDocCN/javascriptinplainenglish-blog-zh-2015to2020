# 有用的 Lodash 数组函数—提取和相交

> 原文：<https://javascript.plainenglish.io/useful-lodash-array-functions-extraction-and-intersection-106569b7238?source=collection_archive---------2----------------------->

![](img/093690353cde2b0e3f83df9911902873.png)

Photo by [Bec Brown](https://unsplash.com/@bec_brown?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个实用程序库，有很多操作对象的方法。它有我们一直在使用的东西，也有我们不常使用或不想使用的东西。

在本文中，我们将研究更有用的 Lodash 数组方法，包括`head`、`indexOf`、`initial`、`intersection`、`intersectionBy`和`intersectionWith`方法。

# 头

方法获取数组的第一个元素并返回它。`first`是`head`的别名。

它将一个数组作为唯一的参数。

我们可以如下使用它:

```
import * as _ from "lodash";
const array = [1, 2, 3];
const result = _.head(array);
console.log(result);
```

那么我们对`result`得到 1。`head`返回`undefined`传入的数组是否为空。

# `indexOf`

`indexOf`获取传入参数的数组中第一个匹配项的索引并返回。

最多需要 3 个参数。第一个是要搜索的数组。第二个参数是要查找的值。第三个是索引开始搜索的可选参数。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const array = [1, 2, 3];
const result = _.indexOf(array, 2, 1);
console.log(result);
```

然后我们得到 1，因为 2 在第二个位置，我们从开始搜索。

# `initial`

`initial`方法获取数组中除最后一个元素之外的所有元素并返回它。

它将一个数组作为唯一的参数。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const array = [1, 2, 3];
const result = _.initial(array);
console.log(result);
```

然后我们得到`result`的`[1, 2]`。

# `intersection`

`intersection`方法返回包含在所有给定数组中的值的数组。相等比较是用 SameValueZero 比较完成的，除了`NaN`被认为等于自身之外，它与`===`相同。

它需要一个逗号分隔的数组列表来返回交集。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const arr1 = [1, 2, 3];
const arr2 = [3, 4, 5];
const result = _.intersection(arr1, arr2);
console.log(result);
```

然后我们得到`[3]`，因为在`arr1`和`arr2`中只有 3 个。

# `intersectionBy`

`intersectionBy`与`intersection`相似，只是它采用了一个为每个数组的每个元素调用的函数来生成比较项目的标准。第一个数组确定结果的顺序和引用。

它以逗号分隔的数组列表作为参数，以一个函数作为要比较的标准来查找交集，或者以一个字符串作为要比较的属性名。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const arr1 = [
  { name: "Joe", age: 10 },
  { name: "Mary", age: 12 },
  { name: "Jane", age: 13 }
];
const arr2 = [
  { name: "Joe", age: 10 },
  { name: "Jerry", age: 12 },
  { name: "Amy", age: 13 }
];
const result = _.intersectionBy(arr1, arr2, a => a.name);
console.log(result);
```

然后我们得到:

```
[
  {
    "name": "Joe",
    "age": 10
  }
]
```

对于`result`。

我们可以用`'name'`代替`a => a.name`，如下所示:

```
import * as _ from "lodash";
const arr1 = [
  { name: "Joe", age: 10 },
  { name: "Mary", age: 12 },
  { name: "Jane", age: 13 }
];
const arr2 = [
  { name: "Joe", age: 10 },
  { name: "Jerry", age: 12 },
  { name: "Amy", age: 13 }
];
const result = _.intersectionBy(arr1, arr2, "name");
console.log(result);
```

然后我们得到同样的东西。

![](img/98be024421b9fb5a4b05f8828c650ce4.png)

Photo by [Sean DuBois](https://unsplash.com/@seandubois?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `intersectionWith`

`intersectionWith`类似于`intersection`，只是它将一个比较器函数作为最后一个参数。该函数的参数是数组中要比较的 2 个条目。

它以逗号分隔的数组列表作为参数，比较器函数作为最后一个参数。

例如，我们可以如下使用它:

```
import * as _ from "lodash";
const arr1 = [
  { name: "Joe", age: 10 },
  { name: "Mary", age: 12 },
  { name: "Jane", age: 13 }
];
const arr2 = [
  { name: "Joe", age: 10 },
  { name: "Jerry", age: 12 },
  { name: "Amy", age: 13 }
];
const result = _.intersectionWith(arr1, arr2, (a, b) => a.name === b.name);
console.log(result);
```

然后我们得到:

```
[
  {
    "name": "Joe",
    "age": 10
  }
]
```

对于`result`。

`head`方法获取数组的第一个元素并返回它。`first`是`head`的别名。

`indexOf`获取传入参数的数组中第一个出现的项目的索引并返回。它可以从默认为 0 的起始索引开始搜索。

`initial`获取数组中除最后一个元素之外的所有元素并返回。

为了找到所有数组中的数组条目，我们可以使用`intersection`、`intersectionBy`和`intersectionWith`方法。它们的不同之处在于，它们可以分别采用标准函数进行比较，或者采用比较器方法进行相等性比较。