# 你写的 JavaScript 更好吗？

> 原文：<https://javascript.plainenglish.io/do-you-write-better-javascript-a75e393d8e09?source=collection_archive---------9----------------------->

## 成为一名注重性能的开发人员

![](img/30ac5b5237fdc98eda25ab93eb36ed0e.png)

Photo by [krakenimages](https://unsplash.com/@krakenimages?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为开发人员，担心我们构建的应用程序的*性能*是必不可少的。**内存管理**是影响应用程序性能的主要因素之一。很多时候，JavaScript 开发人员往往不会有意识地考虑内存管理，因为 JavaScript 会在创建对象时自动分配内存空间，并在不使用时将其作为垃圾收集起来。JavaScript 中这些垃圾收集的幕后行为可能会导致很多混乱。进一步阅读可以更好地理解这一点。

# **内存生命周期**

几乎所有编程语言的内存生命周期都包含三个步骤:

1.  分配您需要的内存
2.  使用分配的内存(读、写)
3.  当不再需要分配的内存时，释放它

如果开发人员不明白这些步骤是如何工作的，他们很容易出现糟糕的代码，从而导致**内存泄漏**。内存泄漏是一种即使不再需要某个对象，内存也不会被释放的情况。

让我们仔细看看这些步骤。

## **JavaScript 中的内存分配**

内存在以下情况下分配:

*   当值被初始化时

```
let num = 123;
let st = 'abc';
let arr = [1,2,3];
```

*   使用函数调用。

```
let date = new Date(); // creates a Date object and allocates memory
let element = document.createElement(‘div’); 
// allocates a DOM element
```

## **使用分配的内存**

每当读取或写入这些值时，就会使用内存。这些值可以是任何变量、函数，甚至是传递给函数的对象。

## **释放分配的内存**

大多数低级语言都有手动分配或释放内存的方法，比如在 C 语言中我们有`malloc()`和`free()`。但是，在 JavaScript 中，这是使用一种叫做**垃圾收集(GC)** 的机制自动发生的。垃圾收集器独自完成内存生命周期中的三个步骤。当 GC 试图判断是否不再需要某个内存时，问题就出现了。

# **垃圾收集**

每个垃圾收集算法使用的一个基本概念是**引用**。如果一个对象可以访问另一个对象，那么它就可以被称为引用。

以下是用于垃圾收集的几种算法:

## **1。引用计数垃圾收集**

这是最简单和最基本的算法之一，只计算与任何对象相关的引用数。如果一个对象根本没有引用，那么它就作为**垃圾**被收集。这种算法最常见的用法之一是在 IE 6 和 IE 7 中，这种算法用于 DOM 对象。

```
let a = {
  b: {
    c: 'Harsha'
  }
};// 2 objects are created. One is referenced by the other as one of // its properties.// The other is referenced by virtue of being assigned to the      // variable 'a'.let z = a.b; // Reference to ‘b’ property of the object.// This object now has 2 references: one as a property,
// the other as the ‘z’ variable.a = 'HarshaVardhan'; //The object initially had json and now it has // lost all references and it’s garbage collectedz = null; // The ‘b’ property of the object originally in a has zero // references to it. It can be garbage collected.
```

这个算法有一个已知的限制——**循环引用**。当两个对象彼此参照创建时，就会出现循环参照，从而形成一个循环。因为每个对象总是有一个引用，所以它永远不会被垃圾收集，从而导致**内存泄漏**。

```
function CircularRef() {
  let obj_1 = {};
  let obj_2 = {};

  // obj_1 references obj_2
  obj_1.object = obj_2; // obj_2 references obj_1
  obj_2.object = obj_1; return ‘Memory leak’;
}CircularRef();
```

## **2。标记和清除算法**

该算法通过仅考虑任何对象的可达性来简化问题。在这个算法中，垃圾收集器从全局对象(又名“根”)开始，然后相应地标记可到达和不可到达的对象。这里，循环引用不再是一个问题。大多数现代浏览器都使用这种算法。唯一的限制是，我们没有办法像在 c 语言中那样手动释放内存。

# **结论**

理解如何管理内存至关重要，因为这有助于更好地理解 JavaScript 如何工作，并帮助您更好地编码，而不必担心应用程序的性能。

你喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**

愿 GC 与你同在:)

# **参考**

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management) [## 内存管理

### 像 C 这样的低级语言有手工内存管理原语，比如 malloc()和 free()。相比之下…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)