# JavaScript 中使用 filter()的 4 个实际用例

> 原文：<https://javascript.plainenglish.io/4-practical-use-cases-of-using-filter-in-javascript-db46e2ec83b2?source=collection_archive---------4----------------------->

## 一些你以前可能不知道的方法

![](img/b82a924b63758178d7917ddefbf6a6b7.png)

Photo by [Marc Babin](https://unsplash.com/@marcbabin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

创建包含给定数组元素子集的新数组是 JavaScript 中最常见的任务之一。

除了使用循环语句，我们可以使用`filter()`以更短、更简洁的方式完成任务。

以下是方法。

# 1.删除重复值

这个有点棘手。我们将利用`indexOf()`函数。它是一个内置函数，返回数组中给定元素的第一个索引。

我们可以使用它来形成一个条件，以删除数组中的重复值，如下所示。

```
const numbers = [3, 12, 54, 12, 4, 4, 3, 12, 16];const filteredNumbers = numbers.filter((number, index) => numbers.indexOf(number) === index);console.log(filteredNumbers); // [3, 12, 54, 4, 16]
```

我们使用回调函数的第二个参数，它是当前元素的索引。

这里的想法是，我们将' indexOf()'函数返回的索引与当前元素的实际索引进行比较。如果它们不同，则当前元素是重复值。

以上面代码片段中的数组为例。当实际指标为`3`时，对应元素的值为`12`。但是，如果我们对该元素使用`indexOf()`，则返回的索引是`1`，因为元素`12`第一次出现在索引`1`处。因此`12`是重复值之一。

# 2.删除无效值

无效值被认为是会导致错误和意外行为的值。

以年龄为例。如果年龄是一个确定的数字，它就是有效的。

现在的要求是过滤所有有效年龄的人。为了完成这个任务，我们可以使用下面的代码片段。

```
const people = [
  { name: ‘Amy’, gender: ‘female’, age: ‘28’ },
  { name: ‘James’, gender: ‘male’, age: 13 },
  { name: ‘Victor’, gender: ‘male’, age: null },
  { name: ‘David’, gender: ‘male’, age: 28 },
  { name: ‘Simon’, gender: ‘male’, age: undefined },
  { name: ‘Anna’, gender: ‘female’, age: 21 },
  { name: ‘Jane’, gender: ‘female’, age: NaN }
];const filteredPeople = people.filter(person => person.age !== undefined && typeof person.age === ‘number’ && !isNaN(person.age));console.log(filteredPeople); // [{ name: ‘James’, gender: ‘male’, age: 13 }, { name: ‘David’, gender: ‘male’, age: 28 }, { name: ‘Anna’, gender: ‘female’, age: 21 }]
```

# 3.过滤数字数组

这个是最简单的用法。

假设你有一个数字数组，你只需要从数组中提取奇数。

```
const numbers = [23, 54, 1, 3, 72, 28];const oddNumbers = numbers.filter(number => number % 2 !== 0);console.log(oddNumbers); // [23, 1, 3]
```

或者你想创建一个新的数组，包含给定数组中的所有素数。

```
const isPrime = number => {
  if (number === 1) return false;
  if (number === 2) return true; for (let i = 2; i < number; i++) {
    if (number % i === 0) return false;
  }

  return true;
}const numbers = [23, 54, 1, 3, 72, 28];
const oddNumbers = numbers.filter(isPrime);
console.log(oddNumbers); // [23, 3]
```

# 4.过滤对象数组

虽然一个对象比一个数字更复杂，但是`filter()`的用法仍然很简单。

例如，假设我们有一组人。要求是找到所有年龄大于 18 岁的人。

```
const people = [
  { name: ‘Amy’, gender: ‘female’, age: 28 },
  { name: ‘James’, gender: ‘male’, age: 13 },
  { name: ‘Victor’, gender: ‘male’, age: 17 },
  { name: ‘David’, gender: ‘male’, age: 28 },
  { name: ‘Simon’, gender: ‘male’, age: 33 }
];const filteredPeople = people.filter(person => person.age > 18);console.log(filteredPeople); // [{ name: ‘Amy’, gender: ‘female’, age: 28 }, { name: ‘David’, gender: ‘male’, age: 28 }, { name: ‘Simon’, gender: ‘male’, age: 33 }]
```

所有 18 岁以上的女性呢？好吧，就给回调函数加一个附加条件。

```
const people = [
  { name: ‘Amy’, gender: ‘female’, age: 28 },
  { name: ‘James’, gender: ‘male’, age: 13 },
  { name: ‘Victor’, gender: ‘male’, age: 17 },
  { name: ‘David’, gender: ‘male’, age: 28 },
  { name: ‘Simon’, gender: ‘male’, age: 33 }
];const filteredPeople = people.filter(person => person.age > 18 && person.gender === ‘female’);console.log(filteredPeople); // [{ name: ‘Amy’, gender: ‘female’, age: 28 }]
```

简单，对！？

# 结论

就是这样。您已经学习了一些在 JavaScript 中使用`filter()`的实用方法。

你知道其他的使用案例吗？请在下面的评论中告诉我。

希望你喜欢这篇文章。

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/4-practical-use-cases-for-iifes-in-javascript-6481dcb0ba7d) [## JavaScript 中生命的 4 个实际用例

### 让我们用生命编写更安全的代码

medium.com](https://medium.com/javascript-in-plain-english/4-practical-use-cases-for-iifes-in-javascript-6481dcb0ba7d)