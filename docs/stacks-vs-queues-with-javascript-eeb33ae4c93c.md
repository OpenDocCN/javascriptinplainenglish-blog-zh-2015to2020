# 使用 JavaScript 的堆栈与队列

> 原文：<https://javascript.plainenglish.io/stacks-vs-queues-with-javascript-eeb33ae4c93c?source=collection_archive---------19----------------------->

## JavaScript 数据结构系列的第 3 部分

![](img/3631dfde82347d0887706cfd8696cc58.png)

Photo by [Iva Rajović](https://unsplash.com/@eklektikum?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将使用 JavaScript 来看看堆栈和队列之间的区别。请记住，我将使用数组来解释不同之处。如果您对创建堆栈或队列的最有效方法感兴趣，我建议您创建一个链表。查看我下面的文章，了解更多关于构建单链表的知识。

[](https://chadmuro.medium.com/building-a-singly-linked-list-with-javascript-8427733361f8) [## 用 JavaScript 构建一个单链表

### 他们说学东西的最好方法是教它。在这一系列文章中，我将教你关于…

chadmuro.medium.com](https://chadmuro.medium.com/building-a-singly-linked-list-with-javascript-8427733361f8) 

# 堆栈与队列

堆栈和队列是添加和删除数据的数据结构。它们之间的区别在于删除数据的顺序。

把书库想象成桌子上的一堆书。你可以在这堆书的上面加一本书。当你拿走一本书时，你从书堆的顶部拿走它(这是你在顶部添加的最后一本书)。这是 LIFO 结构(后进先出)。

把队列想象成一个队列或者一条线。如果你在商店排队购买你的商品，可以在队伍的后面添加一个人。当收银员准备好了，排在队伍前面的人是第一个被帮助的人。这是 FIFO 结构(先进先出)。

# 栈和队列用在哪里？

堆栈最常见的用途是在调用堆栈中。您还会看到在使用撤销/重做或路由时。例如，如果您按下浏览器中的后退按钮，您将返回添加到堆栈中的最后一个站点。

当你需要排队等候时，随时都可以使用队列。一个常见的例子是打印任务。打印机一次只能打印一页，添加到队列中的第一页是打印的第一页。

# 使用数组创建堆栈

使用数组创建堆栈很简单。我们需要使用 push 和 pop 方法在堆栈末尾添加和移除值。这遵循后进先出结构。在下面的例子中，我创建了一个名为 stack 的新数组，并加入了三个值(我访问过的网站)。为了从数组中移除，我将使用 pop 方法来移除我添加到堆栈中的最后一个网站。在这种情况下，我们将删除“youtube”。

```
let stack = [];stack.push('google');
stack.push('medium');
stack.push('youtube');stack.pop();
```

# 使用数组创建队列

使用数组创建队列与创建堆栈非常相似。唯一的区别是，我们不是使用 pop 来删除队列末尾的值，而是使用 shift 来删除队列开头的值。这遵循 FIFO 结构。在下面的例子中，我们创建了一个具有三个值的队列，分别命名为 first、second 和 third。通过使用 shift 方法，我们删除了“first”，它是添加到队列中的第一个值。

```
let queue = [];queue.push('first');
queue.push('second');
queue.push('third');queue.shift();
```

# 堆栈和队列的大 O 符号

堆栈和队列用于插入和删除数据。如果您需要搜索或访问数据，我们通常会使用其他数据结构。在我上面的解释中，我们使用数组来实现堆栈和队列。当处理大量数据时，数组在创建堆栈和队列时效率不高，因为数组还会为每个值创建索引并附加许多其他方法。当添加或删除时，您可能需要重新索引整个数组，这可能导致 O(N)时间复杂度。创建堆栈和队列的更好方法是使用链表。

您可以在下面的代码中看到使用链表创建的堆栈和队列。这将确保添加和删除数据的时间复杂度为 O(1)。

对于堆栈来说，单链表的实现是相同的，只是我们将使用 unshift 和 shift 方法，并将它们重命名为 push 和 pop。我们将使用 shift 和 unshift，而不是从单链表中进行 push 和 pop 的原因是，pop 不是常数时间。

对于队列，我们将使用 push 添加到列表的末尾，使用 shift 从列表的开头删除。因为它是一个队列，我们将方法的名称从 push 改为 enqueue，从 shift 改为 dequeue。

*感谢阅读！如果有兴趣了解更多，推荐查看柯尔特·斯蒂尔的 Udemy 课程，* [*JavaScript 算法与数据结构大师班*](https://www.udemy.com/course/js-algorithms-and-data-structures-masterclass/) *。如果你错过了本系列的第 2 部分，在那里我们学习了如何反转一个单链表，请看下面。*

[](https://chadmuro.medium.com/reversing-a-singly-linked-list-with-javascript-a2d3dc58d344) [## 用 JavaScript 反转单向链表

### 在我的上一篇文章中，我们用 JavaScript 构建了一个具有 push、pop、shift 和 unshift 功能的单链表。检查…

chadmuro.medium.com](https://chadmuro.medium.com/reversing-a-singly-linked-list-with-javascript-a2d3dc58d344)