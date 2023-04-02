# 有用的 Lodash 方法——寻找独特之处

> 原文：<https://javascript.plainenglish.io/useful-lodash-methods-finding-uniques-bc9f39bf79dc?source=collection_archive---------5----------------------->

![](img/262b4182d0349b9487aa2e54e7007ae2.png)

Photo by [Grant Durr](https://unsplash.com/@blizzard88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个实用程序库，有很多操作对象的方法。它有我们一直在使用的东西，也有我们不常使用或不想使用的东西。

在本文中，我们将了解如何使用 Lodash 方法创建具有唯一条目的数据结构。

# _.联合([数组])

`union`方法接受一个逗号分隔的数组列表，并返回一个包含所有数组中唯一条目的数组。结果从值出现的第一个数组中选择。

例如，我们可以如下使用它:

```
import * as _ from "lodash";const arr = _.union([1, 2, 3], [2, 3, 4]);
console.log(arr);
```

那么`arr`就是`[1, 2, 3, 4]`，因为我们有 1、2、3 和 4 作为一个或两个数组的条目。

# _.union by([数组]，[迭代=_。身份])

`unionBy`方法将一个数组列表作为第一个参数，然后是一个函数，当值被迭代时调用这个函数。该函数确定哪些值被认为是唯一的。结果从值出现的第一个数组中选择。

它返回一个新的组合值数组。

例如，我们可以如下使用它:

```
import * as _ from "lodash";const arr = _.unionBy([1, 2.1, 2.7, 3], [2.8, 3.1, 4], Math.round);
console.log(arr);
```

然后我们得到:

```
[1, 2.1, 2.7, 4]
```

作为`arr`的值，因为我们在每个上调用了`Math.round`，然后比较它们，看哪些在用`Math.round`舍入后是相同的。

还有一种通过对象的属性找到唯一值的简单方法。例如，我们可以写:

```
import * as _ from "lodash";const arr = _.unionBy([{ x: 2 }], [{ x: 2 }, { x: 1 }], "x");
console.log(arr);
```

然后我们得到:

```
[
  {
    "x": 2
  },
  {
    "x": 1
  }
]
```

作为`arr`的值。

# _.union with([数组]，[比较器])

我们可以使用`unionWith`再取一个数组和一个比较器函数来比较每一项的值，看看哪些是相等的。它从被认为相同的项目列表中取第一个。

然后它将这些值放入数组并返回它。例如，我们可以如下使用它:

```
import * as _ from "lodash";const arr1 = [{ x: 1, y: 3 }, { x: 2, y: 1 }];
const arr2 = [{ x: 1, y: 1 }, { x: 1, y: 3 }];
const arr = _.unionWith(arr1, arr2, _.isEqual);
```

然后我们写有:

```
[
  {
    "x": 1,
    "y": 3
  },
  {
    "x": 2,
    "y": 1
  },
  {
    "x": 1,
    "y": 1
  }
]
```

作为`arr`的值，因为它获取数组中唯一的第一个值，并将它们放入返回的数组中。

![](img/80ecdd58a8cb9f4d66dc93acb6c23d87.png)

Photo by [Daniel McCarthy @themccarthy](https://unsplash.com/@danielmccarthy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# _.唯一(数组)

`uniq`方法获取一个数组，然后返回一个消除了重复项的新数组。

例如，我们可以如下使用它:

```
import * as _ from "lodash";const arr = _.uniq([1, 2, 3, 4, 4, 5]);
```

然后我们得到:

```
[
  1,
  2,
  3,
  4,
  5
]
```

作为`arr`的值。

# 结论

Lodash 有返回数组的方法，方法是从一个或多个数组中获取唯一值，然后在一个新的数组中返回它们。有些还支持通过我们的标准来确定值是否唯一的不同方法，如`unionBy`方法。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****