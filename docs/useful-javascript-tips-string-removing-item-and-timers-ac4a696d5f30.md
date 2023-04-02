# 有用的 JavaScript 技巧——字符串、删除项目和计时器

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-string-removing-item-and-timers-ac4a696d5f30?source=collection_archive---------7----------------------->

![](img/7c57ba9ca03734b902bca5781f4d5ef2.png)

Photo by [Jacques Bopp](https://unsplash.com/@jacquesbopp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 将字符串的第一个字母大写

我们可以通过获取第一个字母并使其大写来使字符串的第一个字符大写。

然后，我们将字符串的其余部分附加到第一个字母上。

例如，我们可以编写以下内容:

```
const name = 'joe'
const capitalized = name[0].toUpperCase() + name.slice(1)
```

然后我们得到`“Joe”`作为`capitalized`的值。

# 从数组中删除项目

我们可以用索引从数组中移除一个项。

## 移除单个项目

例如，给定数组的索引，我们可以如下使用`slice`。

我们写道:

```
const items = ['foo', 'bar', 'baz'];
const i = 1;
const filteredItems = items.slice(0, i).concat(items.slice(i + 1, items.length));
```

我们使用`slice`来获取数组中不包括索引`i`的部分。

然后，我们使用`concat`将切片与数组的其余部分以及索引`i`中的项目连接起来。

所以，我们从`i + 1`开始。

同样，我们可以在没有给定项或值本身的索引的情况下调用`filter`。

例如，我们可以写:

```
const items = ['foo', 'bar', 'baz'];
const valueToRemove = 'bar';
const filteredItems = items.filter(item => item !== valueToRemove);
```

我们也可以对指数做同样的事情:

```
const items = ['foo', 'bar', 'baz'];
const i = 1;
const filteredItems = items.filter((_, index) => index !== i);
```

## 移除多个项目

我们可以通过编写以下内容，按索引删除多个项目:

```
const items = ['foo', 'bar', 'baz', 'qux'];
const indexes = [1, 2];
const filteredItems = items.filter((_, index) => !indexes.includes(index));
```

同样，我们可以对值本身使用`includes`方法。

例如，我们可以写:

```
const items = ['foo', 'bar', 'baz', 'qux'];
const valuesToRemove = ['foo', 'qux'];
const filteredItems = items.filter(item => !valuesToRemove.includes(item));
```

`slice`和`filter`不变异原数组，所以比变异数组好。

# 检查字符串是否有子串

我们可以用`includes`方法检查一个字符串是否有子串。

例如，我们可以写:

```
'I like dogs'.includes('dogs')
```

那么这会返回`true`，因为`'dogs'`在字符串中。

`includes`也使用索引的第二个参数开始搜索。

例如，我们可以写:

```
'I like dogs'.includes('dogs', 11)
```

然后`includes`从 11 开始搜索，所以我们得到`false`，因为它不在从索引 11 开始的子串中。

我们也可以使用`indexOf`方法。

它采用相同的参数，除了我们比较了返回的索引。

例如，我们可以写:

```
'I like dogs'.indexOf('dogs') !== -1
```

如果`indexOf`找不到给定的子串，则返回 1。否则，它将返回子串的索引。

我们也可以传入索引开始搜索:

```
'I like dogs'.indexOf('dogs', 11) !== -1
```

# 计时器

JavaScript 为我们提供了`setTimeout`函数来延迟 JavaScript 代码的执行:

```
setTimeout(() => {
  // ...
}, 2000)
```

然后代码在 2 秒钟后运行。

我们可以通过编写以下代码将参数传递给回调函数:

```
const callback = (firstParam, secondParam) => {
  // ...
}setTimeout(callback, 2000, firstParam, secondParam)
```

然后`firstParam`和`secondParam`被传递到`callback`函数中。

`setTimeout`返回一个定时器 ID。

然后我们可以调用`clearTimeout`来删除内存中的计时器。

例如，我们可以写:

```
const timer = setTimeout(() => {
  // ...
}, 2000)clearTimeout(timer)
```

我们调用`clearTimeout`来移除计时器。

我们可以将延迟设置为零。然后回调被排队，并在同步代码运行后无延迟地运行。

所以我们可以写:

```
setTimeout(() => {
  //...
}, 0)
```

`setInterval`函数类似于`setTimeout`，但是回调是在一个区间内运行的。

例如，我们可以写:

```
setInterval(() => {
  // ...
}, 2000)
```

然后回调每 2 秒运行一次。

和`setTimeout`一样，我们可以用`clearInterval`停止回调运行。

例如，我们可以写:

```
const id = setInterval(() => {
  // ...
}, 2000)clearInterval(id)
```

我们通过使用带有返回 ID 的`clearInterval`来清除计时器。

![](img/a393a2b35ea0db4f74fcb479e0787025.png)

Photo by [Kouji Tsuru](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`toUpperCase`方法大写一个字符串的第一个字母。

从数组中移除一个项有很多种方法。

此外，我们可以使用计时器在一定间隔或延迟后运行代码。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**