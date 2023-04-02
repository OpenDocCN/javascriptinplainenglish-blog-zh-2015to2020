# 用 JavaScript 实现一个链表

> 原文：<https://javascript.plainenglish.io/implementing-a-linked-list-in-javascript-3f71c83487b5?source=collection_archive---------0----------------------->

## JavaScript 中的数据结构介绍

![](img/d5c9b8f10e95c5f3d9956f5407e00cbd.png)

Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在列表中存储数据的最好方式是什么？在计算机科学课程中，链表是学习者首先遇到的数据结构之一。它们是一种抽象的数据类型，其中每个元素指向下一个元素，理论上，这为性能带来了一定的优势。

在 C 和 C++这样的低级语言中，有大量关于链表的信息，它们更有可能被应用。但是，在本文中，我想让其他人——比如自学的开发人员或新手——更容易接触到这些信息，他们更习惯于使用更高级的 web 开发语言，比如 JavaScript。

# 我需要用 JavaScript 实现一个链表吗？

对于一篇关于这个主题的文章来说，这似乎是一个奇怪的答案——但不，你可能不会。

理论上，链表*在某些情况下*应该比数组快，比如插入新的项目。但是，实际上，他们不是。这是因为数组被 JavaScript 引擎高度优化了，比如用 C++编写的 Google V8。除非链接列表被引入到 ECMAScript 规范中，并被给予同样的优化，否则数组([或集合](https://medium.com/@bretcameron/how-to-make-your-code-faster-using-javascript-sets-b432457a4a77?source=---------27------------------))仍然是最快的选择。

# 所以，何必呢？

嗯，随着 JavaScript 成为越来越多程序员的首选语言，重要的计算机科学主题应该用人们熟悉的语言来解释，这似乎是合适的。

经历这一过程有助于加深我对构建库、编译器甚至整个编程语言的考虑和权衡的了解。这些知识可以帮助我们在生产代码中做出更好的决策，即使我们没有使用链表。

另外，[这个流行的 npm 包](https://www.npmjs.com/package/linked-list)有超过 50，000 的周下载量，所以显然有一些需求！

# 数组和链表有什么区别？

数组和链表都是有序的数据集合，但是在规模上，一个提供更有效的数据访问，另一个提供更有效的插入。根据实现的不同，可能还会有其他的不同，但这些是最重要的。

## 排列

数组中的每一项都有一个数字，称为数字索引，允许您访问它。这使得访问元素非常有效。

假设我们有一个四个字符串的列表，`'A'`、`'B'`、`'C'`和`'D'`。如果我们将这个列表构造成一个数组，它看起来会像这样:

```
| **Value**: | 'A'  | 'B'  | 'C'  | 'D'  |
|--------|------|------|------|------|
| **Index**: | 0    | 1    | 2    | 3    |
```

## 链表

每个元素指向下一个元素，而不是使用数字索引。我们不必在每次插入或删除元素时都更新数字索引。特别是，在链表中插入元素比在数组中插入元素更有效。

下面是我们的列表看起来的样子，结构像一个链表:

```
| **Value**: | 'A'  | 'B'  | 'C'  | 'D'  |
|--------|------|------|------|------|
| **Next**:  | 'B'  | 'C'  | 'D'  | null |
```

# 用 JavaScript 实现一个链表

那么，我们如何在 JavaScript 中实现一个链表呢？Oleksii Trekhleb 的[优秀的 JavaScript 算法库](https://github.com/trekhleb/javascript-algorithms)有一个很好的实现。在本文中，我们将使用 Trekhleb 的实现作为我们自己的链表的基础，但是我们将在几个重要的方面对它进行更改，通过…

*   添加一个附加方法(`includes`)，
*   添加一个附加属性(`size`)，并由
*   使 Trekhleb 的一些现有方法(如`constructor`函数)更加通用。

在我们进入代码之前，在您最喜欢的 IDE 中打开一个新文件夹，并创建三个新的 JavaScript 文件:`index.js`、`LinkedList.js`和`LinkedListNode.js`。将我们的`LinkedList`和单个节点作为单独的类是一个很好的实践，所以首先我们将构建我们的节点类，所以我们将从`LinkedListNode.js`开始。

# 第 1 部分:类构造函数

## LinkedListNode

每个节点都需要保存自己的值和列表中下一个节点的值，所以我们可以在类构造函数中将每个节点指定为一个属性:

```
class LinkedListNode {
  constructor(value, next) {
    *this*.value = value;
    *this*.next = next || null;
  }
}module.exports = LinkedListNode
```

这就是我们的`LinkedListNode`课所需要的！回到`index.js`，我们可以导入我们的新类并试用它:

```
const LinkedListNode = require('./LinkedListNode');console.log(new LinkedListNode(3));
console.log(new LinkedListNode(3, 10));
console.log(new LinkedListNode('string', true));
```

## 链接列表构造函数

现在，让我们进入`LinkedList.js`，这将是一个更大的类。我们将开始研究构造函数。我们希望在内存中保存第一个条目(T15)和最后一个条目(T16)，默认情况下是 T17。

对于这个实现，我还希望将列表的`size`保存在内存中，这样就可以很容易地找出列表中一次有多少项:

```
const LinkedListNode = require('./LinkedListNode');class LinkedList {
  constructor() {
    *this*.size = 0;
    *this*.head = null;
    *this*.tail = null;
  }
}module.exports = LinkedList
```

请注意，以下所有代码——除非另有说明——都将在`LinkedList`类中。

# 第 2 部分:添加新条目

## 预先考虑

在测试新类之前，我们需要一种添加新值的方法。我们将从一个`prepend`方法开始，将值添加到列表的前面，类似于`Array.unshift`:

```
prepend(value) {
  *this*.size += 1; const newNode = new LinkedListNode(value, *this*.head); *this*.head = newNode;
  if (!*this*.tail) *this*.tail = newNode;
  return *this*;
}
```

*   首先，我们基于提供的值和当前的`head`节点定义一个新节点。
*   然后，我们更新`head`来等于这个新节点。
*   第三，如果没有指定`tail`(因为这是列表中的第一项)，那么`head`和`tail`应该是相同的。
*   最后，我们用`*this*`返回整个`LinkedList`。

## 附加

我们还希望有一种方法将元素添加到列表的末尾，类似于`Array.push`。要做到这一点，还需要几个步骤:

```
append(value) {
 *this*.size += 1; const newNode = new LinkedListNode(value); if (!*this*.head) {
    *this*.head = newNode;
    *this*.tail = newNode;
    return *this*;
  }; *this*.tail.next = newNode;
  *this*.tail = newNode;
  return *this*;
}
```

*   像以前一样，我们从创建一个`new LinkedListNode`开始，但是这次我们不需要指定第二个参数(因为没有下一个值)。
*   如果这是列表中的第一个元素，我们也要更新`*this*.head`，我们可以提前返回函数。
*   最后，我们需要更改当前最终节点的`next`属性，然后在末尾插入我们的`newNode`。

要了解发生了什么，我建议回到`index.js`中，尝试向我们的列表添加值。

# 第 3 部分:与数组相互转换

为了方便起见，应该可以在构造函数中提供一个数组，并按照这个顺序获得一个`LinkedList`项——类似于`Set`构造函数。

## from 数组

首先，让我们创建一个`fromArray`方法，其中我们`append`数组中的每一项:

```
fromArray(values) {
  values.forEach(value => *this*.append(value));
  return *this*;
}
```

## 构造器

接下来，我们可以在`constructor`中触发我们的新方法:

```
constructor(value) {
  *this*.size = 0;
  *this*.head = null;
  *this*.tail = null; if (value) {
    if (Array.isArray(value)) return *this*.fromArray(value);
    return new TypeError(value + ' is not iterable');
  }; return *this*;
}
```

如果一个数组被传递，我们运行`fromArray`方法。如果传递的不是数组，而是一个值，我们返回一个`TypeError`。如果没有提供值，我们将`*this*.head`和`*this*.tail`设置为`null`，就像之前一样。

## toArray

我们还需要一种方法将链表转换回数组。

```
toArray(useNodes = false) {
  const nodes = [];
  let currentNode = *this*.head;
  while (currentNode) {
    nodes.push(useNodes ? currentNode : currentNode.value);
    currentNode = currentNode.next;
  };
  return nodes;
}
```

这个方法包含一个名为`useNodes`的参数，当`true`时，它将使用每个`LinkedListNode`对象填充数组，而不仅仅是值，这对于调试很有帮助。

回到`index.js`，我们可以测试不同之处:

```
const LinkedList = require('./LinkedList');let list = new LinkedList([1, 2, 3]);console.log(list.toArray());
console.log(list.toArray(true));
```

# 第 4 部分:删除条目

## 删除

`delete`操作是我们所有链表方法中最复杂的，因为它需要稍微不同的步骤，这取决于是否需要删除头、尾或任何其他节点。

默认情况下，我们的函数将删除某个值的所有节点。但是我们可以将`true`作为第二个参数来传递，只删除我们遇到的具有给定值的第一个节点。

```
delete(value, deleteOne = false) {
  if (!*this*.head) return false;
  let deletedNode = null; *// If the head needs to be deleted* while (*this*.head && *this*.head.value === value) {
    *this*.size -= 1;
    deletedNode = *this*.head;
    *this*.head = *this*.head.next;
    if (deleteOne) return true;
  }; let currentNode = *this*.head; *// If any node except the head or tail needs to be deleted* if (currentNode !== null) {
    while (currentNode.next) {
      if (currentNode.next.value === value) {
        *this*.size -= 1;
        deletedNode = currentNode.next;
        currentNode.next = currentNode.next.next;
        if (deleteOne) return true;
      } else {
        currentNode = currentNode.next;
      };
    };
  }; *// If the tail needs to be deleted* if (*this*.tail.value === value) {
    *this*.tail = currentNode;
  }; if (deletedNode === null) {
    return false;
  } else {
    return true;
  };
}
```

对我来说，这段代码清楚地说明了为什么删除项目比插入项目要昂贵得多！

## 删除头

删除列表开头或结尾的项目也是有用的。对于第一项，这很简单:

```
deleteHead() {
  if (!*this*.head) return false; *this*.size -= 1; const deletedHead = *this*.head; if (*this*.head.next) {
    *this*.head = *this*.head.next;
  } else {
    *this*.head = null;
    *this*.tail = null;
  } return true;
}
```

## 删除尾巴

删除最后一个项目是一个开销更大的操作，因为——当列表中有多个项目时——我们需要遍历整个列表来定位倒数第二个项目。

```
deleteTail() {
  if (*this*.size === 0) return false; if (*this*.size === 1) {
    if (*this*.head === null) {
      return false;
    } else {
      *this*.head = null;
      *this*.tail = null;
      *this*.size -= 1;
      return true;
    }
  } const deletedTail = *this*.tail; let currentNode = *this*.head;
  while (currentNode.next) {
    if (!currentNode.next.next) {
      *this*.size -= 1;
      currentNode.next = null;
    } else {
      currentNode = currentNode.next;
    }
  } *this*.tail = currentNode;
  return deletedTail;
}
```

# 第 5 部分:访问条目

在链表中，访问值具有线性时间复杂度，因为我们必须遍历整个列表——总是从第一个条目到最后一个条目。

## 包含

在我们的实现中，我们希望我们的`includes`方法接受一个值或者一个`LinkedListNode`类的实例。如果具有正确值的元素存在于我们的列表中，它应该返回`true`，否则返回`false`。

```
includes(value) {
  if (!*this*.head) return false; let isNode = value.constructor.name === 'LinkedListNode';
  if (isNode) value = value.value; let currentNode = *this*.head; while (currentNode) {
    if (value !== undefined && value === currentNode.value) {
      return true;
    };
    currentNode = currentNode.next;
  }; return false;
}
```

## 发现

我们的`find`方法应该返回链表中满足回调函数的第一个元素的值。否则应该返回`undefined`。如果提供的回调不是一个函数，我们将抛出一个`TypeError`:

```
find(callback) {
  if (Object.prototype.toString.call(callback) !== '[object Function]') {
    return new TypeError(callback + ' is not a function');
  }; if (!*this*.head) return undefined;

  let currentNode = *this*.head; while (currentNode) {
    if (callback && callback(currentNode.value)) {
      return currentNode;
    };
    currentNode = currentNode.next;
  }; return undefined;
}
```

# 结论

就这样结束了！

要查看我们实现的完整代码，请查看 GitHub 要点。

这些方法应该足以涵盖链表的核心用例。当然，有很多方法可以扩展我们的`LinkedLists`的功能。如果你有兴趣在我们目前所做的基础上进行构建，你可以尝试添加一个`sort`、`filter`、`reverse`、`forEach`、`toString`或`clear`方法。

我希望您发现这是对计算机科学的基本数据类型之一的有用介绍，如果您有任何问题或反馈，请随时留下评论。

# 进一步阅读

对于链表的可选 JavaScript 实现，请查看:

[](https://www.npmjs.com/package/linked-list) [## 链表

### 小型双向链表。子类化:创建新的链表。创建一个新的 this 并添加给定的项目数组…

www.npmjs.com](https://www.npmjs.com/package/linked-list)  [## trekhleb/JavaScript-算法

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/trekhleb/javascript-algorithms/blob/master/src/data-structures/linked-list/LinkedList.js) 

如果您热衷于学习更多关于用 JavaScript 编写的数据结构和算法的知识，我强烈建议您看看 Oleksii Trekhleb 的完整 JavaScript 算法库:

[](https://github.com/trekhleb/javascript-algorithms) [## trekhleb/JavaScript-算法

### 这个库包含许多流行算法和数据结构的基于 JavaScript 的例子。每个算法和…

github.com](https://github.com/trekhleb/javascript-algorithms)