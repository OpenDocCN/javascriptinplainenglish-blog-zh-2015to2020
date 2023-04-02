# 在 ES6 JavaScript 中实现链表

> 原文：<https://javascript.plainenglish.io/implementing-a-linked-list-in-es6-javascript-be896ed51d5f?source=collection_archive---------5----------------------->

![](img/319d9d40045eb9f55330404d5d32a579.png)

Photo by [Daniel von Appen](https://unsplash.com/@daniel_von_appen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

链表是一种线性数据结构。数据结构是一个计算机科学术语，用来指数据存储、组织和管理的方法。JavaScript 中您可能已经熟悉的一些数据结构是数组和对象。类似于不同的算法，不同的数据结构有独特的优点、缺点和用例。让我们看看如何使用 JavaScript ES6 类实现一个链表。

# 什么是链表？

链表是由节点组成的数据结构。每个节点至少有两个组件，它保存的数据和对列表中下一个节点的引用(称为指针)。链表中的第一个节点叫做*头*，链表中的最后一个节点叫做*尾。*链表有几种。单向链表只包含一个指向其后节点的指针，双向链表包含一个指向前后节点的指针，而在循环链表中，尾部指向列表中较早的一个节点(通常是头部),作为后面的节点，这就形成了一个循环。

## 数组与链表

数组占用内存中的连续空间。也就是说，在物理内存中，数组的每个元素都与该数组的其他元素相邻。数组还为它的每个元素留出相同数量的内存。因此，很容易对数组中的元素执行随机查找。如果我们知道数组的第一个元素在内存中的位置，并且每个元素占用相同的空间，并且紧挨着前面的元素，我们就可以计算出数组中任何元素的内存地址。另一方面，链表的节点在内存中并不相邻，这就是为什么我们有一个指向链表中下一个节点的引用。然而，正因为如此，每个节点只需要占用存储其数据和指向其他节点的指针所需的空间。

# 利弊

## 赞成的意见

*   **不浪费内存**。如前所述，链表中的每个节点只占用存储数据和指针所需的内存。
*   **轻松插入新节点**。在一个数组中，当你添加一个元素时，你必须移动它后面出现的每个元素，这样它们仍然保留在连续的内存中。在链表中，新的节点可以放在内存中任何有空闲空间的地方，你只需要改变指针。
*   **轻松删除节点**。类似于插入，当从链表中删除一个节点时，你需要做的就是告诉它前面的节点指向它后面的节点。那么没有一个节点会指向你想删除的节点，它也不再是链表的一部分。
*   **动态尺寸**。在某些语言中，当初始化一个数组时，你需要明确说明它将包含多少元素，以及这些元素是什么类型的。您不需要在 JavaScript 中这样做，但是，当一个数组变得太大而不适合内存中的连续空间时，每个元素都必须复制到一个有足够空间的新位置。所有这些都是在幕后进行的，很容易不去想。这在链表中是不会发生的，所以只要有足够的内存来存储它们各自的数据和指针，你就可以继续添加节点。

## 骗局

*   **每个节点占用更多内存**。因为我们必须存储数据和指针。
*   **无随机存取**。在链表中查找后续节点的唯一方法是跟随指针。
*   **反向穿越困难**。在一个单独的链表中很难回溯，因为只有一个指向下一个节点的指针。在双链表中更容易，但是额外的指针会消耗更多的内存。

# 用 JavaScript 编写单链表

编写链表的第一步是创建一个`ListNode`类。

```
class ListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}
```

这个简单的类如上所述。它保存着我们的数据(可以是字符串、整数、对象、数组，甚至其他节点)，并且它有一个对下一个节点的引用。在你的终端里，你可以尝试一下。制作几个这样的节点:

```
let node1 = new ListNode(1);
let node2 = new ListNode(2);
node1.next = node2;
```

然后如果你检查`node1.next`你会看到它返回`node2`。您可以继续这个过程，事实上，您可以通过这样做来创建一个链表。然而，在另一个类中封装该功能和更多功能会很好。

所以这正是我们要做的。让我们上`LinkedList`课吧。

```
class LinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }
}
```

每个链表都需要跟踪它的`head`和`size`。

从现在开始，其余的方法将在我们的`LinkedList`类中。

我们接下来需要的是向我们的`LinkedList`添加一个新节点的功能。

```
add(data) {
  let newNode = new ListNode(data); if (this.head === null) {
    this.head = newNode;
  } else {
    let currentNode = this.head;
    while (!!currentNode.next) {
      currentNode = currentNode.next;
    } currentNode.next = newNode;
  }
  this.size++;
}
```

`add`方法接收新节点的数据。从那里，它初始化一个`ListNode`的新实例。我们检查我们的链表是否有一个头节点，如果没有，我们把我们的新节点指定为链表的头。否则，我们创建一个 while 循环来遍历链表，从`head`节点开始，直到找到一个没有`next`指针的节点。一旦我们找到那个节点，我们知道我们已经到达了我们的单链表的尾部，所以我们把我们的`newNode`赋值给`next`值，把它添加到链表中(新的节点现在是链表的尾部)。做完所有这些后，我们增加链表的`size`。

现在我们可以添加一个节点，接下来我们可能要做的是检索一个节点。

```
getNodeAt(index) {
    if (index < 0 || index > this.size - 1) {
      return undefined;
    } else {
      let currentNode = this.head;
    for (let i = 0; i < index; i++) {
      currentNode = currentNode.next;
    }
    return currentNode;
  }
}
```

我们的链表将被索引。如果有人传入一个负数，或者一个大于链表中索引数的数(比我们的`size`少一个)，我们将返回`undefined`。否则，我们需要遍历链表，直到到达所需索引处的节点。因为我们将`currentNode`分配给列表中的下一个节点*，所以我们将在距离索引 1 处停止。一旦我们脱离了`while`循环，我们只需要返回`currentNode`。*

现在我们可以添加和检索节点，是时候弄清楚如何删除它们了。首先，让我们按索引删除。

```
removeFrom(index) {
    if (index < 0 || index > this.size - 1) {
      return undefined;
    } else {
      let currentNode = this.head; if (index === 0) {
        this.head = currentNode.next;
      } else {
        let previousNode;
        for (let i = 0; i < index; i++) {
          previousNode = currentNode;
          currentNode = currentNode.next;
        }
        previousNode.next = currentNode.next;
      }
      this.size--;
      return currentNode.data;
    }
  }
```

所以我们在我们的`removeFrom`方法的开始执行一个类似的检查，以确保传入的索引是有效的。之后，我们将`currentNode`赋给头部，如果`index`为 0，我们知道我们想要移除头部节点。我们可以通过简单地将链表的头分配给它后面的节点来实现。

否则，我们需要开始遍历链表，跟踪`previousNode`。对于链表中的每个循环，我们将把`previousNode`分配给旧的`currentNode`，并把`currentNode`重新分配给它后面的节点。一旦我们在我们的索引，`currentNode`是我们想要删除的节点。所以，我们需要做的是让`previousNode`指向 `currentNode`之后的节点*。因此，我们告诉`previousNode`指向`currentNode`当前指向的任何东西。现在我们的链表中没有一个节点指向`currentNode`！从那里我们只需要减少`size`。*

但是如果我们不知道要删除的节点的索引呢？让我们创建一个方法，根据存储的数据删除一个节点。

```
removeNode(data) {
    let currentNode = this.head;
    let previousNode;
    while (!!currentNode) {
      if (currentNode.data === data) {
        if (previousNode) {
          previousNode.next = currentNode.next;
        } else {
          this.head = currentNode.next;
        }
        this.size--;
        return currentNode.data;
      }
      previousNode = currentNode;
      currentNode = currentNode.next;
    }
    return undefined;
}
```

就像之前我们需要遍历链表一样，从头部开始。我们还需要存储之前的节点。我们的`while`循环正在检查以确保`currentNode`存在，因为一旦我们通过了尾部`currentNode`就会是`null`。因此，在每次迭代中，我们检查由`currentNode`存储的数据是否等于我们希望移除的节点的数据。如果是，我们检查`previousNode`是否存在。如果是，我们做与上面相同的事情，并使`previousNode`指向`currentNode`之后的节点。如果`previousNode`未定义，那么我们知道数据存储在头节点，所以我们将头节点重新分配给下一个节点，将其从链表中移除。之后，我们减少大小并返回，中断 while 循环。如果我们遍历整个链表，没有找到保存数据的节点，我们将返回`undefined`。

现在我们可以添加、检索和删除元素。我们需要创建它，这样我们就可以在链表中插入一个新的节点，而不仅仅是添加到末尾。

```
insertAt(index, data) {
    if (index < 0 || index > this.size - 1) {
      return undefined;
    } else {
      let newNode = new ListNode(data);
      if (index === 0) {
        newNode.next = this.head;
        this.head = newNode;
      } else {
        let currentNode = this.head;
        let previousNode;
        for (let i = 0; i < index; i++) {
          previousNode = currentNode;
          currentNode = currentNode.next;
        } previousNode.next = newNode;
        newNode.next = currentNode;
      } this.size++;
    }
} 
```

我们检查以确保我们的索引是有效的。如果是，我们初始化一个新的节点。如果`index`为 0，我们将向列表中添加一个新的头。因此，我们将`newNode`的下一个指针赋给当前的头，然后将链表的头重新赋给`newNode`。否则，我们需要遍历链表。类似于我们移除节点时的操作。一旦我们找到了想要插入的索引之前的节点，以及当前驻留在那里的节点，我们就将`previousNode`的下一个指针分配给我们的`newNode`，然后将`newNode`的指针分配给`currentNode`(曾经在该索引处的节点)。

我们的下一个方法将返回保存一些数据的节点的索引。

```
indexOf(data) {
  let currentNode = this.head;
  let index = 0;
  while (!!currentNode) {
   if (currentNode.data === data) {
     return index;
    } else {
     currentNode = currentNode.next;
    index++;
   }
 }
 return -1;
}
```

我们只是遍历链表，跟踪索引，如果找到保存数据的节点，就返回那个索引。如果我们的链表没有包含该数据的节点，我们将返回-1。这复制了 JavaScript 内置的`indexOf`函数在字符串或数组上的功能。

如果我们想清除我们的链表呢？嗯，过程其实很简单。

```
clear() {
  this.head = null;
  this.size = 0;
}
```

如果没有对头部的引用，我们的链表也会丢失对其余节点的引用，然后我们只需将大小重置为 0。

这些方法将提供链表的基本功能。当然，根据您需要的功能，您还可以添加更多的方法。我们可以添加、插入、删除和查找所有节点，也可以完全清空列表。

# 最后

链表是一种简单的数据结构，但是习惯使用指针和顺序遍历可能需要很大的调整。这篇博客中的所有代码都可以在 GitHub repo 中找到[。链表的优势实际上来自于它的动态内存，以及从链表的开始和中间添加和删除节点的便利性。当你需要频繁地前进和后退时，可以使用双向链表(比如浏览网页，撤销和重做文本/图像编辑器，或者遍历音乐播放列表)。理解链表也有助于理解其他带有指针的数据结构，比如树和图。](https://github.com/ReginaF2012/JavaScript_LinkedList)