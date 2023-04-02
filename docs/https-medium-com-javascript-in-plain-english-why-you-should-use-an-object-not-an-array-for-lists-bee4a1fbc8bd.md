# 为什么在 Redux 中应该对列表使用对象而不是数组

> 原文：<https://javascript.plainenglish.io/https-medium-com-javascript-in-plain-english-why-you-should-use-an-object-not-an-array-for-lists-bee4a1fbc8bd?source=collection_archive---------0----------------------->

![](img/533825f41349177968ef21411f3f4b1a.png)

Photo by [Ilija Boshkov](https://unsplash.com/photos/0nI1DczRQAM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 这同样适用于任何状态管理工具

如果你正在开发一个应用程序，你可能会用到一系列的条目:用户、评论、帖子、待办事项、电影等等。

Javascript 中最常见的列表数据结构是数组。

我想解释为什么在状态管理工具(如 Redux 或 React Context)中存储这些列表时，这可能不是最佳选择。

# C[RUD]对单一项目的操作

如果您想对单个项目进行 RUD(读取、更新或删除)操作，您总是需要迭代。

除了创建之外，您只需要将项目添加到列表中。

## 阅读

如果您使用数组，您需要通过过滤找到该项。当你有一个对象时，你只需要你用作键的标识符。

## 更新

使用数组时，最快的方法是映射并更改您想要更改的数组。对于对象，只需设置指向更新项的属性。

## 删除

数组的过滤器。只需删除一个对象键。

# 从 API 获取

这就是使用对象优于使用数组的地方。

当您的项目来自后端时，您需要获取它们。当你两次取同一个物品时会发生什么？还是同一个单子两次？或者有一些重复和一些新项目的列表？

如果你使用一个数组，你需要确保没有重复的项目。你需要处理这种情况。这意味着增加更多的复杂性。

向一个对象添加同一个项目两次怎么样？

无事可做。这些项目按键存储。在一个对象中，同一个键不能出现两次。您只能用已经处于该状态的项目来替换提取的项目。

# 何时不使用对象

如果你没有一个唯一的标识符。或者您希望在不使用额外属性的情况下保持订单。

# 代码示例

让我们来看一些代码示例。下面的代码示例仅代表上述操作。他们不是 redux reducers 或其他类型的状态创建者。它们只是一些片段，以便更好地理解我上面提出的观点。

我们有一个评论列表。

```
// array
const comments = [
  { id: '1', text: 'add code examples' },
  { id: '2', text: 'examples would be great for this article' },
  { id: '3', text: 'hi there' },
];// object
const comments = {
  '1': { id: '1', text: 'please add code examples' },
  '2': { id: '2', text: 'examples would be great for this article' },
  '3': { id: '3', text: 'hi there' },
};
```

## 阅读评论 id 2

```
const commentId = '2';// array
comments.find((comment) => comment.id === commentId)// object
comments[commentId];
```

## 更新注释 id 1

```
const updatedComment = { id: '1', text: 'add code examples, please' };// array
comments.map((comment) => {
  if (comment.id === updatedComment.id) {
    return updatedComment;
  }
  return comment;
});// object
comments[updatedComment.id] = updatedComment;
```

## 删除注释 id 3

```
const commentId = '3';// array
comments.filter((comment) => comment.id !== commentId);// object
delete comments[commentId];
```

## 添加新评论

我们必须假设我们不知道它是否已经存在

```
const newComment = { id: '4', text: 'thanks for the code examples!' };// array
if (comments.find((comment) => comment.id === newComment.id) {
  comments = comments.map((comment) => {
    if (comment.id === newComment.id) {
      return newComment;
    }
    return comment;
  });
} else {
  comments = comments.concat([newComment]);
}// object
comments[newComment.id] = newComment;
```

# 如果您的代码中需要一个数组呢？

有时，您可能会发现对项目列表使用数组更容易。例如，当您呈现列表时。拥有一个数组、使用一个映射并在每次迭代中创建元素更容易。

这有一个简单的解决方案。不要直接访问状态，而是使用函数。也就是通常所说的选择器 T1。

选择器是一个获取某种状态并返回您需要的数据的函数。通常是您想要的模式或结构。

```
// object
const comments = {
  '1': { id: '1', text: 'please add code examples' },
  '2': { id: '2', text: 'examples would be great for this article' },
  '3': { id: '3', text: 'hi there' },
};// transform to array
const commentsSelector = (commentsObj) => {
  return Object.keys(commentsObj)
    .map((commentKey) => commentsObj[commentKey]);
};// usage of the selector
const commentsArray = commentsSelector(comments);
```

# 结论

使用一个对象将为你未来的自己节省一些时间和错误。帮她/他一个忙，用一个对象存储一个项目列表。

即使作为初学者，我也建议您使用对象而不是数组来存储项目列表。奋斗终有回报。正如大多数斗争一样:)

更多关于状态形状的最佳实践，请参见 [Redux 关于规范化的文档](https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape)。

感谢您的阅读。我很想在评论中听到你的想法。

如果你喜欢这篇文章，考虑加入我在 [GIMTEC](https://www.gimtec.io/) 的时事通讯。

[](https://www.gimtec.io/) [## GIMTEC

### 训练营后教育。我希望在我完成训练营时就拥有的每周时事通讯和课程。

www.gimtec.io](https://www.gimtec.io/)