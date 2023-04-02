# 如何在 JavaScript 中使用 Map、Filter 和 Reduce

> 原文：<https://javascript.plainenglish.io/learn-how-to-use-map-filter-and-reduce-in-5-minutes-fbf5c5540b2f?source=collection_archive---------0----------------------->

![](img/14381ced414f07ac021880fd5945deb9.png)

# Map、Filter 和 Reduce 是 Javascript 中的三个主要功能，是 2015 年 ES6 规范的一部分。

最终，map、filter 和 reduce 只是可以在数组上使用的函数。它们所做的事情都可以通过其他方式实现，从简单的 for 循环到稍微复杂一点的事情，比如在 for 循环中调用函数。在它的核心，映射，过滤和减少提供了非常好的，简洁的方法来做某些事情。

*注意:如果你想听起来像你知道你在说什么，Map，Filter 和 Reduce 是高阶函数(这些基本上都是以其他函数为参数的函数)。*

注意，本教程将使用箭头函数。如果您还没有学会如何使用它们，那没问题，因为您仍然可以使用常规函数语法编写 map、filter 和 reduce 函数。

# 好了，现在没什么事了，我们走吧！

所以，假设我们一直在打造一个待办 app(因为这个世界需要另一个待办 App！)

假设每当我们创建一个新任务时，它都会被添加到我们现有的数组中，假设到目前为止我们已经向它添加了三个任务。

想看看目前为止我们的阵列是什么样的吗？看下面:

```
let tasks = [
 {
   'name'     : 'Buy milk from the shop', 'duration' : 20, 'priority' : 1
 },
 {
   'name'     : 'Clean the house', 'duration' : 120, 'priority' : 3
 },
 {
   'name'     : 'Study JS functions', 'duration' : 180, 'priority' : 1
 },
]
```

现在，假设我们想要一种快速简单的方法来显示所有的任务名称，但不关心持续时间，因为我们只是想要一个漂亮、干净的需要完成的任务列表。这就是我们可以使用地图功能的地方。

`tasks.map(task => task.name)`

//输出

`["Buy milk from the shop", "Clean the house", "Study JS functions"]`

现在让我们想象一下，我们的待办事项应用程序能够为每个任务标记不同的优先级(1 表示高优先级，2 表示中优先级，3 表示低优先级)。如果我们希望我们的应用程序有一个只显示高优先级任务的部分，我们可以使用过滤功能。

`tasks.filter(task => task.priority == 1)`

//输出

`[{name: "Buy milk from the shop", duration: 20, priority: 1}, {name: "Study JS functions", duration: 180, priority: 1}]`

现在让我们假设 tasks 数组中每个对象的 duration 部分被分配给我们估计的完成每个任务所需的时间。也许我们希望在我们的待办事项应用程序的角落里有一个小部分，显示完成我们列表中所有任务所需的预计总时间。嗯，我们可以把它们一个一个加起来，或者我们可以用一个简洁的 reduce 函数来做这件事。

`tasks.map(task => task.duration).reduce((total, amount) => total + amount)`

//输出

`320`

reduce 函数需要更多的设置，因为我们最初需要使用 map 来创建一个持续时间数组。然后，我们可以在其上链接一个 reduce 函数，简单地将数组中的数字相加并输出结果。

# 现在我们有了一些如何使用映射、过滤和减少的工作示例😄

*对于任何想要阅读关于 map、filter 和 reduce 的非常、非常、非常详细的评论的人，我推荐阅读:* [*https://code . tuts plus . com/tutorials/how-to-use-map-filter-reduce-in-JavaScript-CMS-26209*](https://code.tutsplus.com/tutorials/how-to-use-map-filter-reduce-in-javascript--cms-26209)*。这篇文章深入到了真正的本质，应该能帮助你更好地理解它——说实话，它是这篇文章的基础:)*