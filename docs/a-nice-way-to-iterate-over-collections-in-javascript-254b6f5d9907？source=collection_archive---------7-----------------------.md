# JavaScript 中迭代集合的好方法

> 原文：<https://javascript.plainenglish.io/a-nice-way-to-iterate-over-collections-in-javascript-254b6f5d9907?source=collection_archive---------7----------------------->

## 用一致的方法减少认知负荷

![](img/d9e6edb100b097ed3596f1045d8563da.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

有许多方法可以循环集合、[对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)和[数组](https://developer.mozilla.org/en-US/docs/Glossary/array)。其中许多非常适合特定的用例。我们将讨论一种迭代集合项的“好”方法，这将涵盖大多数用例。

# 什么叫“好看”？

在我谈论循环集合的方法之前，让我定义一下“好”

迭代集合的一种“好”方法应该是一致的:

*   支持对象和数组
*   不会遍历原型链中的所有属性
*   允许使用`async/await`
*   允许使用关键字`break`，或者另一种提前结束循环的方法
*   允许使用`continue`关键字，或者用另一种方法来退出单个项目的逻辑，并转移到下一个项目
*   可以访问`index`进行`array`收藏
*   自动管理集合上的迭代
*   适合中等规模数据集的性能

总而言之,“好”的方式将允许开发人员使用它作为迭代集合的默认方式，而不用考虑太多。

# For…Of 循环

在下面的例子中，我们迭代包含各种搜索引擎的 URL 的集合。在每一次迭代中，我们调用传递给它的搜索引擎 URL 并返回结果的函数。如果找到结果，我们`break`退出循环。如果给定的 URL 是`null`或者没有定义，我们就`continue`到集合中的下一个项目。

## **数组**

这里，我们使用以下一般结构迭代数组:

```
for(const item of array) {
  // logic
}
```

示例:

```
const array = [
  'www.google.com',
  'www.yahoo.com',
  null,
  'www.bing.com'
];for(const searchEngineUrl of array) {
  if(!searchEngineUrl) {
    continue;
  }
  const results = await getSearchEngineResults(searchEngineUrl);
  if(results) {
    break;
  }
}
```

## **对象**

在这里，我们使用以下通用结构迭代对象:

```
for(const [key, value] of Object.entries(obj)) {
  // logic
}
```

我们在目标对象上使用 [Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) 来使对象可枚举，从而可供 for…of 循环使用。我们[解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)每一项以暴露变量“键”和“值”。

示例:

```
const obj = {
  google: 'www.google.com',
  yahoo: 'www.yahoo.com',
  doesNotExist: null,
  bing: 'www.bing.com',
};for(const [key, searchEngineUrl] of Object.entries(obj)) {
  if(!value) { 
    console.log(`Skipping ${key}`);
    continue;
  }
  const results = await getSearchEngineResults(searchEngineUrl);
  if(results) {
    break;
  }
}
```

## **阵列带**

为了在迭代数组时获得`index`,我们使用以下通用结构:

```
for(const [ index, value ] of array.entries()) {
  // logic
}
```

我们使用[。entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/entries) 方法在数组上公开了`index`。我们[解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)每一项，以暴露变量“索引”和“值”。

示例:

```
const array = [ 'www.google.com', 'www.yahoo.com', 'www.bing.com' ];for(const [ index, searchEngineUrl ] of array.entries()) {
  if(!value) { 
    console.log(`Skipping index ${index}`);
    continue;
  }
  const results = await getSearchEngineResults(searchEngineUrl);
  if(results) {
    break;
  }
}
```

## 为什么不是为了…在循环中？

循环中的[很棒，除了以下限制:](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)

*   它遍历原型链中的所有属性

如果要迭代对象原型链中的所有属性，请使用 for…in 循环。

## 为什么不呢？forEach()循环？

[数组。forEach()循环](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)很棒，但是它有以下限制:

*   不能在对象上循环
*   无法有效使用`async/await`
*   不能`break`过早退出循环

使用。forEach()用于同步逻辑，在那里你不希望`break`提前退出。

## 为什么不是经典的 For 循环？

[经典 for 循环](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)非常棒，除了它们有以下限制:

*   不会自动管理集合上的迭代

对于传统的 for 循环，开发人员必须定义一个`index`，递增`index`，并定义退出循环的条件。此外，在每次迭代中，要获得集合的值，必须手动使用`index`来查找值。

当需要对`index`增量(或减量)进行粒度控制时，使用传统的 for 循环。此外，当大型数据集的性能很重要时，使用经典的 for 循环，或 [while 循环](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while)。

## 摘要

有许多方法可以迭代集合。大多数都有特定的用例。for…of 循环是遍历集合的“好”方法，因为它缺乏限制和灵活性。这是 JavaScript 中的一个好去处。

帕特里克

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****