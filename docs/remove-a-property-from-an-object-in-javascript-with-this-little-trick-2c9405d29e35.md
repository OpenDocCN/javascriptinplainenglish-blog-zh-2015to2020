# 用这个小技巧在 JavaScript 中删除对象的属性

> 原文：<https://javascript.plainenglish.io/remove-a-property-from-an-object-in-javascript-with-this-little-trick-2c9405d29e35?source=collection_archive---------3----------------------->

## 在 reducers 中使用 JavaScript spread 语法

![](img/1efd006023a684eb0b07e4382c3176bb.png)

Photo by [Devon Janse van Rensburg](https://unsplash.com/@devano23?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/remove-property?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在使用 reducer 中的函数时，我遇到了一个简洁的模式(一个`useReducer()`钩子，不是 redux)。

## **目标**

我需要从 state 中存储的列表(只是一个普通的对象)中删除一个特定的属性。

我有物业 id，所以应该不会太难。

当我们在一个 reducer 中工作时，我们需要警惕状态突变和函数的副作用。即，我们想要返回新的和更新的状态，而不改变原始状态。

下面是我最初尝试使用 reducer 函数的一个例子:

```
function deleteProperty(state, payload) {
  const { id } = payload 

  // make a copy of the state variable to avoid mutation
  const newState = Object.assign({}, state) const { list } = newState *// delete the property for the id
  delete list[id]* return newState
}
```

*注意:这是一个来自更复杂的减速器函数的人为例子，只是试图说明其中的一部分！*

它不太复杂，只有几行代码，但是我们可以做得更好！

## **使用扩展语法克隆对象**

使用 JavaScript，我们可以使用 spread 语法创建一个对象的新的浅层克隆，而不是使用`Object.assign`:

```
let obj1 = { foo: 'bar', x: 42 };

let clonedObj = { ...obj1 }
```

按照这种模式，我们还可以使用恰当命名的 rest 参数来扩展对象的“Rest”。

从[MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters):

> rest 参数语法允许我们将不定数量的参数表示为一个数组。

## 解决方案

因此，在这种情况下，我们知道要删除的属性的 id，并且只想克隆对象的“其余部分”,我们可以执行以下操作:

```
function deleteProperty(state, payload) {
  const { id } = payload // clone the list, excluding the property with the selected id
  const { [id]: remove, ...newList } = state.list // return a clone of the state variable to avoid mutation
  return { ...state, list: newList }}
```

如果我们只关注这条线:

```
const { [id]: remove, ...newList } = state.list
```

我们将关键字`id`的属性值赋给一个名为 remove 的变量(变量名无关紧要)，并将对象的“其余部分”分配给一个名为`newList`的变量。

这意味着我们的`newList`变量将是原始变量`list`的克隆，减去属性`id`。

## 其他类似文章

[](https://medium.com/javascript-in-plain-english/which-equals-operator-should-i-use-in-javascript-comparisons-vs-c3bc44960518) [## 我应该在 JavaScript 比较中使用哪个等号运算符？(== vs ===)

### 在 JavaScript == vs ===中比较东西，应该用哪个？

medium.com](https://medium.com/javascript-in-plain-english/which-equals-operator-should-i-use-in-javascript-comparisons-vs-c3bc44960518) [](https://medium.com/javascript-in-plain-english/how-to-remove-an-element-from-an-array-in-javascript-54612785295e) [## 如何在 JavaScript 中从数组中移除元素

### 在 JavaScript 中从数组中快速移除元素的两种方法

medium.com](https://medium.com/javascript-in-plain-english/how-to-remove-an-element-from-an-array-in-javascript-54612785295e)