# JavaScript 中的集合是什么？

> 原文：<https://javascript.plainenglish.io/the-set-object-12039b62c628?source=collection_archive---------10----------------------->

![](img/6cc9250e8457cca08e3f00fb714d6cf4.png)

There can only be one...

刚碰到一个对 JavaScript 有用的工具:`Set`。从来没有人教过我，我也没有在文献中发现它…直到现在。也许它不常用，或者我只是错过了机会。我确实读到过那是一个`ES6`附加物。在最近进行代码挑战时，我发现挑战通常需要使用一组独特的值，通常是从数组中提取的。在`Ruby`中，有一个名为`[.uniq](https://ruby-doc.com/core/Array.html#method-i-uniq)`的内置方法，它产生一个被调用数组的副本，所有重复的值都被删除。例如:

```
arr = [1, 1, 2, 2, 3, 3]arr.uniq
# => [1, 2, 3]
```

我一直在用`JavaScript`写，没有这样的内置方法。有多种方法可以将数组转换为唯一值的数组。两个流行的选择是利用`[.filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)`或`[.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)`，但是这些可能需要额外的方法，并且很冗长。另一种选择是利用`Set`！

`Set`是存储任何数据类型的唯一值集合的对象。原始值、对象、`undefined`，应有尽有。阅读[文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)会很高兴，但是这里有一些关于`Set`的主要品质的要点:

*   它具有类似`.size`的属性
*   它有一个名为`.has(arg)`的评估方法
*   它有类似`.add(arg)`、`.delete(arg)`和`.clear()`的变异方法
*   它有类似`.forEach(callBack)`和`.values()`的迭代方法

`Set`的实际构造移除了所有重复的值，因此这是一个非常简单的将数组转换成唯一数组的工具。香肠是这样制作的:

```
// declare variable and assign value of raw array
const arr = [4, 1, 1, 2, 5, 3, 4, 5, 2, 3]// construct Set and provide arr as an input argument
const set = new Set(arr)console.log(set)
// => {4, 1, 2, 5, 3} 
// *note: set is bound to insertion order// declare variable and assign values of set to new array
const uniqueArr = [...set]
// *note: ES6 spread operator = schwingconsole.log(uniqueArr)
// => [4, 1, 2, 5, 3]
```

所以，这是一个非常简单的工具，用来创建数组的唯一副本。上面的例子甚至可以简化为:

```
const arr = [4, 1, 1, 2, 5, 3, 4, 5, 2, 3]const uniqueArr = [...new Set(arr)]console.log(uniqueArr)
// => [4, 1, 2, 5, 3]
```

关于`Set`的另一个很酷的事情是它可以按原样操作。我最近完成了一项代码挑战，要求程序接受两个参数:

1.  一个整数数组，*如* `[n, n, n, n, n]`
2.  一个加数值，*如* `k`

并返回一个整数，该整数表示数组中能够在此等式`n + k = n`内成功求值的*唯一*对的数量。

使用`Set`创建输入数组的唯一副本的能力是一个有用的快捷方式，它提供了一个集合或值的集合，更容易操作，并且没有必要从集合中创建新的数组。

```
const arr = [1, 2, 3, 4, 5, 6]function findAddendSumPairs(arr, k) {
  const set = new Set(arr)
  const pairs = [] for (n of set) {
    if (set.has(n - k)) pairs.push([n, n - k])
  }
  console.log('pairs', pairs)
  return pairs.length
}findAddendSumPairs(arr, 2)
// => 4
// => pairs [[3, 1], [4, 2], [5, 3], [6, 4]]
```

而且，如果您真的想坚持使用数组，这也是可行的:

```
const arr = [6, 6, 1, 5, 10, 0, -5]function findAddendSumPairs(arr, k) {
  const uArr = [...new Set(arr)]
  const pairs = [] for (let i = 0; i < uArr.length; i++) {
    if (uArr.includes(uArr[i] - k)) {
      pairs.push([uArr[i], uArr[i] - k])
    }
  }
  console.log('pairs', pairs)
  return pairs.length
}findAddendSumPairs(arr, -5)
// => 4
// => pairs [[ 1, 6], [5, 10], [0, 5], [-5, 0]]
```

如果你还没用过`JavaScript`的`Set`对象，那就试试吧！这是一个很酷的工具，可以很容易地创建一个独特的值集合，然后可以对其进行操作，并且还有其他实用工具。现在，今天的单词是:

# **加数**

/ˈaˌdend/
**名词**
*def。*加到另一个数上的数。
`addend + addend = sum`

你可能已经注意到了本文中的“加数”这个词。在这个词汇单调、同质的时代，我喜欢学习简洁的描述词。

[github.com/dangrammer](https://github.com/dangrammer)
[linked.com/in/danieljromans](https://www.linkedin.com/in/danieljromans/)
danromans.com