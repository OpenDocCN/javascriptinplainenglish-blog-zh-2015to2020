# JavaScript 纯函数和副作用

> 原文：<https://javascript.plainenglish.io/javascript-pure-functions-and-side-effects-f0642b70a484?source=collection_archive---------4----------------------->

想知道开发人员所说的纯功能或副作用是什么意思吗？想知道为什么它们在 JavaScript 生态系统中如此普遍，理解它们非常重要吗？

![](img/f22c3ff99d719711faeb2021e88ba229.png)

Photo by [Sarah Kilian](https://unsplash.com/@rojekilian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/accident?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我将通过定义它们是什么，以及为什么使用代码块示例理解它们很重要，来带您浏览这些纯函数和副作用的术语。到本文结束时，你应该能够分辨出常规函数和纯函数之间的区别。最重要的是，你应该了解什么是副作用，以及为什么你应该避免在许多不同的情况下产生它们。

让我们一起享受和学习:

# **纯函数**

JavaScript 中被认为是纯函数的函数必须具有以下两个属性:

*   给出相同的输入总是返回相同的输出。
*   该功能不会产生任何副作用。

在本文的后面，我们将进一步了解副作用，但首先让我们看看第一个属性:

## 1.同样的输入，同样的输出。

纯函数在其输入和输出之间有一对一的映射或连接，这意味着每个唯一的输入都有一个唯一的对应输出。为了进一步理解这看起来像什么，让我们看下面的 clear 块:

```
const customSalutation = (name) => {
return `Hey ${name}`;
}
```

函数`customSalutation`将*名称*作为一个参数，并返回我的自定义问候语，这个函数的伟大之处在于输出很容易预测，对于您给它的相同名称，您将得到您期望的相同的返回问候语。

如果我们在这里创建一个名为`salutaion`的常量，我调用`customSalutation`并传递我自己的名字，然后我打印出我的问候常量，我得到`Hey Hafid`作为输出。

```
const customSalutation = (name) => {
return `Hey ${name}`;
}const salutation = customSalutation('Hafid');
console.log(salutation);
// Hey Hafid
```

因此，不管这个方法如何被调用或使用，如果你给它相同的输入，你总是可以期望得到相同的输出。这里有一个不同的情况，我们有一个名为`*names*`的数组，它的正下方有一个变量`nameSalutaions`，它是从映射中分配的返回值，首先将第一个函数`customeSalutations`作为一个*回调函数*，它将在`names`数组*的每个元素上被调用。*

```
const customSalutation = (name) => {
return `Hey ${name}`;
}const names = ['Hafid', John', 'Lucy''];
const nameSalutation = name.map(customSalutation);console.log(nameSalutation);
// [
//   'Hey Hafid'
//   'Hey John',
//   'Hey Lucy' 
// ]
```

输出是对每个人的称呼。纯函数允许你有这样的期望，给定相同的输入，你应该有相同的输出，这使得你的代码可读性更好，因为很容易看到你的输入和输出之间的映射。这对于*记忆*或*缓存*来说也是很棒的，因为如果你有相同的输入，你可以预测输出，所以不用每次都调用你的纯函数，你可以把你的输出保存在某个位置，然后在你需要的时候获取它。通过利用这一点，您可以真正优化您的应用程序。

## 2.副作用。

好了，现在我们理解了*相同输入，相同输出*的概念，让我们仔细看看副作用是什么。JavaScript 中的这个术语指的是可以改变应用程序外部状态的函数的概念，这意味着函数可以改变应用程序中不直接驻留在同一方法中的部分或值。

在两种常见情况下，函数会产生副作用。

## 2 .改变外部范围:

第一个是函数可以进入它的外部作用域并直接改变那些外部变量的值。

让我们举一个这个场景的例子:

```
const show = {
  title: 'One peace',
  episodes: 120,
}const watchNextEpisode = () => {
  show.episodes = show.episodes +1;
  return show.episodes;
}console.log(watchNextEpisode);
// 121
console.log(watchNextEpisode);
// 122
```

上面的方法产生了一个副作用，因为它在每次`show`对象中的`episodes`的数量改变时都会改变。

## 2.b 改变方法参数:

在这种情况下，方法直接改变它的参数

```
const comedies = ['The Office', 'House', 'Comunity'];const appendShow = (shows, newShow) => {
  shows.push(newShow);
  return shows;
}const shows = appendShow(comedies, '30 Rock');console.log(shows);
// ['The Office', 'House', 'Comunity', '30 Rock'];console.log(comedies);
// ['The Office', 'House', 'Comunity', '30 Rock'];
```

这里的副作用发生在将`newShow`字符串推送到`shows`数组参数时，该参数是对`comedies`数组的引用。

当有些人不清楚传递给该方法的原始参数将被更改时，就会出现问题。

为了避免这种情况，让我们仔细看看这个代码块:

```
const comedies = ['The Office', 'House', 'Comunity'];const appendShow = (shows, newShow) => {
  return [...shows, newShow];
}const shows = appendShow(comedies, '30 Rock');console.log(shows);
// ['The Office', 'House', 'Comunity', '30 Rock'];console.log(comedies);
// ['The Office', 'House', 'Comunity'];      =]
```

现在，不是直接传递给参数`shows`，而是追加字符串`newShow`，然后返回新的*数组。*

在某些情况下，纯功能和副作用可能真的很明显，可以很好地识别，但有时却不是。所以，就像你学习 JavaScript 过程中的其他事情一样，你需要花一些时间来训练自己识别一个纯函数是什么样的，或者副作用是什么样的，但是我希望这篇文章是一个很好的起点。

# 关于我

一名自动化工程师，一名 Web 开发人员，一名数据科学爱好者，有时也是一名博客作者。总是在寻找新的挑战，所以请在 LinkedIn 上关注我，不要犹豫就任何问题联系我。