# 为什么不应该通过将数组重新分配给[]来删除数组的所有元素

> 原文：<https://javascript.plainenglish.io/why-you-should-not-use-array-to-remove-all-the-elements-of-array-42ad2951d47e?source=collection_archive---------1----------------------->

## 理解使数组为空时的棘手行为，并学习如何修复它。

![](img/f9ee179ae5bd7ae51494c3b3136dfcbf.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

很多时候，当我们想删除数组中的所有元素时，也许我们有一个 todo 列表，我们想一次删除所有的 todo。

考虑一下，我们有一个项目列表

```
let items = ["tea", "coffee", "milk"];
```

然后从数组中移除所有的元素，我们把它设置成一个空数组，就像这样

```
items = []
```

这非常有效，你会发现它一直在被使用。但是这有一个问题。

考虑一下，在代码下面

```
let items = ["tea", "coffee", "milk"];let copy = items;
```

这里我们创建了一个`copy`变量，并赋予它一个`items`数组的引用，因此`copy`实际上指向了`items`数组。

现在，让我们检查一下它们各自的值

```
console.log(items); // ["tea", "coffee", "milk"]console.log(copy); // ["tea", "coffee", "milk"]
```

这是意料之中的。

现在，如果我们改变其中的任何一个，它会修改原始数组本身，因为它只是一个引用，而不是一个精确的副本。

```
copy[2] = "oranges";console.log(items); // ["tea", "coffee", "oranges"]console.log(copy); // ["tea", "coffee", "oranges"]
```

但是，如果我们想删除项目数组的所有元素，我们这样做

```
items = [];
```

现在，如果我们再次检查`items`和`copy`。

```
console.log(items); // []console.log(copy); // ["tea", "coffee", "oranges"]
```

如你所见，`items`数组是空的，但是`copy`仍然包含原始数组，并且没有被清除。

这是因为当我们说，`items = []`时，它实际上创建了一个新的空数组，并将其赋给`items`，因此`copy`变量不受影响。

对于像`array`、`objects`这样的非原语类型，当我们赋值时，不是实际的值被赋值，而是被分配的内存地址的唯一引用被存储在那个变量中。

所以当我们说，array = []，[]只是在内存中分配一些空间的一种简便方法，其中没有数据，所以数组将只包含已分配内存空间的引用。

对象也是如此。当我们说`obj = { name: 'David'}`时，obj 变量并不包含对象本身，而只是引用分配的内存空间，在那个内存空间中，实际的对象被存储。

```
let obj = { name: 'David' };
let newObj = obj;console.log(obj); // { name: 'David' }
console.log(newObj); // { name: 'David' }obj = null;console.log(obj); // null
console.log(newObj); // { name: 'David' }
```

所以有两种方法可以解决这个问题。

1.  **将数组长度设置为 0:**

```
let items = ["tea", "coffee", "milk"];let copy = items;
console.log(items); // ["tea", "coffee", "milk"]
console.log(copy); // ["tea", "coffee", "milk"]items.length = 0;console.log(items); // []
console.log(copy); // []
```

2.**使用数组拼接方法:**

```
let items = ["tea", "coffee", "milk"];let copy = items;
console.log(items); // ["tea", "coffee", "milk"]
console.log(copy); // ["tea", "coffee", "milk"]// remove elements of items array starting from position 0 till length of array
items.splice(0, items.length);console.log(items); // []
console.log(copy); // []
```

这些修复之所以有效，是因为它们实际上改变了原始数组，而不只是像 items = []那样改变了项的引用

**结论:**

我们不应该为数组或对象创建额外的引用，因为这可能会产生不希望的结果，如果你确定没有对原始数组的引用需要清空，那么你应该使用`array = []`来清空它，否则使用上述两种方法中的任何一种来清空数组。

今天到此为止。我希望你学到了新东西。

**别忘了直接在你的收件箱** [**订阅我的每周简讯，里面有惊人的技巧、诀窍和文章。**](https://yogeshchavan.dev/)