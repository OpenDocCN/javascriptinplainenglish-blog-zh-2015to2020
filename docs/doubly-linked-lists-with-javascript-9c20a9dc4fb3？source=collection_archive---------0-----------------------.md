# 使用 JavaScript 的双向链表

> 原文：<https://javascript.plainenglish.io/doubly-linked-lists-with-javascript-9c20a9dc4fb3?source=collection_archive---------0----------------------->

## JavaScript 数据结构系列的第 4 部分

![](img/79167dd3cf141281958a7217ffff7bdb.png)

Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将看看双向链表，并使用 JavaScript 将它们与单向链表进行比较。如果你不熟悉单链表，可以看看下面的两篇文章。

[](https://chadmuro.medium.com/building-a-singly-linked-list-with-javascript-8427733361f8) [## 用 JavaScript 构建一个单链表

### 他们说学东西的最好方法是教它。在这一系列文章中，我将教你关于…

chadmuro.medium.com](https://chadmuro.medium.com/building-a-singly-linked-list-with-javascript-8427733361f8) [](https://medium.com/swlh/reversing-a-singly-linked-list-with-javascript-a2d3dc58d344) [## 用 JavaScript 反转单向链表

### 在我的上一篇文章中，我们用 JavaScript 构建了一个具有 push、pop、shift 和 unshift 功能的单链表。检查…

medium.com](https://medium.com/swlh/reversing-a-singly-linked-list-with-javascript-a2d3dc58d344) 

# 什么是双向链表？

双向链表非常类似于单向链表；它是由节点组成的数据结构。该列表将有一个头、尾和长度值。与数组不同，链表没有指向每个节点的索引，所以搜索或访问数据的唯一方法是遍历链表。

![](img/76c1935a850ab72275d2e2078f79be9d.png)

[https://www.geeksforgeeks.org/doubly-linked-list/](https://www.geeksforgeeks.org/doubly-linked-list/)

双向链表和单向链表的一个区别是双向链表中的每个节点都有指向下一个和上一个值的指针，而单向链表中的节点只有指向下一个值的指针。要建立一个双向链表类，它将与单向链表完全相同。下面的代码为节点建立了一个类，为双向链表建立了一个类。

```
class Node {
  constructor(val) {
    this.val = val;
    this.prev = null;
    this.next = null;
  }
}class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
}
```

# 向双向链表添加方法

我们现在将向双向链表类添加方法。在本文中，我们将处理 push、pop、shift 和 unshift 的基本方法。它类似于构建一个单链表类，除了我们需要记住每个节点都有一个 next 和一个 previous 值。

## **推送(在末尾添加一个节点)**

push 方法将接受一个值作为参数，我们将首先用该值创建一个新节点，我们在这里将其保存为 new node。我们需要检查一种极端情况，即列表是否为空。我们可以通过检查是否没有头，或者长度是否等于 0 来做到这一点。如果是这样，那么我们将简单地将列表的头部和尾部设置为新的节点。如果列表不为空，我们将把当前尾部的 next 属性设置为新节点，把新节点的 previous 属性设置为当前尾部，把列表的 tail 属性设置为新节点。最后，我们将列表的长度增加一，并返回列表。

```
push(val) {
  const newNode = new Node(val);
  if (this.length === 0) {
    this.head = newNode;
    this.tail = newNode;
  } else {
    this.tail.next = newNode;
    newNode.prev = this.tail;
    this.tail = newNode;
  }
  this.length++;
  return this;
}
```

## Pop(从末端移除节点)

在 pop 方法中，我们将有两种边缘情况。首先，如果列表是空的，我们返回 undefined。第二，如果列表的长度等于 1，我们将删除该节点，因此我们将列表的 head 和 tail 属性设置为 null。在其他情况下，我们首先需要将当前尾部存储在一个临时变量中，这样我们就不会丢失对它的引用。然后，我们将设置列表的尾部等于当前尾部的前一个节点。接下来，我们需要切断新旧尾巴之间的联系。这是通过将新尾部的下一个设置为空并将旧尾部的前一个(存储为 temp)设置为空来实现的。最后，我们将长度减少 1，并返回旧的尾部。

```
pop() {
  if (this.length === 0) return undefined;
  const temp = this.tail;
  if (this.length === 1) {
    this.head = null;
    this.tail = null;
  } else {
    this.tail = temp.prev;
    this.tail.next = null;
    temp.prev = null;
  }
  this.length--;
  return temp;
}
```

## **Shift(从开始处删除一个节点)**

对于一个双向链表，shift 方法与 pop 方法非常相似，除了我们将使用头部，而不是尾部。我们将再次首先检查长度等于 0 和等于 1 的两种情况，并做完全相同的事情。否则，我们将把当前头存储在一个临时变量中，然后把列表的新头设置为旧头的下一个值。然后，我们将通过将新头的前一个值设置为空，并将旧头的下一个值(存储为 temp)设置为空，来切断新旧头之间的联系。最后，我们将列表的长度减 1，并返回旧的头。

```
shift() {
  if (this.length === 0) return undefined;
  const temp = this.head;
  if (this.length === 1) {
    this.head = null;
    this.tail = null;
  } else {
    this.head = temp.next;
    this.head.prev = null;
    temp.next = null;
  }
  this.length--;
  return temp;
}
```

## **Unshift(在开头添加一个节点)**

就像 shift 类似于 pop，unshift 类似于 push，除了我们将使用头部而不是尾部。unshift 方法有一个参数，我们将用这个值创建一个新节点。我们将首先检查空列表的边缘情况。如果是这样，我们将把列表的 head 和 tail 属性设置为新的节点。否则，我们将设置新节点的下一个值为旧头，设置旧头的上一个值为新节点，设置列表的头为新节点。最后，我们将长度增加 1 并返回列表。

```
unshift(val) {
  const newNode = new Node(val);
  if (this.length === 0) {
    this.head = newNode;
    this.tail = newNode;
  } else {
    newNode.next = this.head;
    this.head.prev = newNode;
    this.head = newNode;
  }
  this.length++;
  return this;
}
```

最终的代码应该如下所示。在我们的例子中，我们有节点类和双向链表类，以及 push、pop、shift 和 unshift 方法。在代码的最后，我们创建了一个列表，并将值 1–5 推送到列表中。从这里，您可以使用我们编写的方法轻松地添加或删除节点。

# 双向链表与单向链表

从上面的例子中可以看出，在双向链表中添加和删除与单向链表相似。但是，需要记住一些不同之处。

## 双向遍历

如果需要从头到尾遍历列表，拥有一个双向链表会很方便。与只能从头开始遍历的单链表相比，它使得在链表中导航容易得多。

## 流行音乐的大 O 符号

在单链表中，pop 方法有一个大 O 符号 O(N)。这是因为我们需要在尾部之前获取节点的值，唯一的方法是从头开始遍历每个节点。在一个双向链表中，pop 有一个很大的 O(1)符号，因为我们可以从尾部访问它作为前一个值。

## 记忆

单链表需要更少的内存来存储，因为每个节点只有一个方向的指针。在双向链表中，每个节点也有一个 previous 指针，所以会占用更多内存。

*喜欢这篇文章吗？如果有，通过* [***订阅我们的 YouTube 频道***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) ***获取更多类似内容！***

*如果您错过了本系列的第 3 部分，在那一部分中，我们讨论了使用 JavaScript 的堆栈和队列，请查看下面的文章。*

[](https://medium.com/javascript-in-plain-english/stacks-vs-queues-with-javascript-eeb33ae4c93c) [## 使用 JavaScript 的堆栈与队列

### JavaScript 数据结构系列的第 3 部分

medium.com](https://medium.com/javascript-in-plain-english/stacks-vs-queues-with-javascript-eeb33ae4c93c) 

*敬请关注该系列的第 4 部分，我们将在那里深入了解二分搜索法树。*