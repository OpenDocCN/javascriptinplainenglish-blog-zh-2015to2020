# 在“反应”中更新对象嵌套数组中的值

> 原文：<https://javascript.plainenglish.io/react-updating-a-value-in-state-array-7bae7c7eaef9?source=collection_archive---------0----------------------->

## 如何只更新一个状态中对象数组中的一个值

![](img/1b858730ad1303dc96e670ac8c89e3a5.png)

“Red and Blue Pelikan Br 40 Eraser on White Surface” by [Pixabay](https://www.pexels.com/@pixabay)

假设我们有一个包含一系列待办事项的组件，如下所示:

```
state = {
  todos: [
   {id: 1, 
    title: "Eat breakfast",
    completed: false},
   {id: 2,
    title: "Make bed",
    completed: false}
  ]
}
```

我们怎么能只在一个对象中更改 key 的值呢？

# 第一步:找到元素

我们首先要找到对象数组中的索引，或者对象在数组中的位置。您可以通过任何键、id 或名称或您认为有用的任何其他键来查找元素。我们将使用我们从函数中获得的 id。通过给定元素的 id 和它在数组中的**索引找到该元素(例如通过使用方法)。findIndex())**

```
const elementsIndex = this.state.todos.findIndex(element => [element.id](http://element.id/) == id )
```

# 第 2 步:创建状态数组的副本

因为我们不应该[直接修改状态](https://reactjs.org/docs/state-and-lifecycle.html#do-not-modify-state-directly)，让我们通过 [ES6 扩展运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)功能创建它的副本，并将其保存到一个变量中。

```
let newArray = [...this.state.todos]
```

# **第 3 步:更新一个值**

以下是我们的步骤:
1。让我们重新定义一个我们想要更新的对象，用括号表示它的索引。
2 .让我们创建对象的副本(再次扩展运算符)，然后重写我们想要的一个键的一个值。在这种情况下，我们将把“完成”键从假转换为真，所以我们使用！来否定当前值。

```
newArray[elementsIndex] = {...newArray[elementsIndex], completed: !newArray[elementsIndex].completed}
```

注意:虽然“todos”是一个数组，这也是索引很重要的原因，但是 toDoItem 是一个对象，对象是无序的集合。因此，您只能无序选择一个将被更新的值。

# 第 4 步:设置状态

将`newArr`作为“todos”的值传递:

```
this.setState({
todos: newArray,
});
```