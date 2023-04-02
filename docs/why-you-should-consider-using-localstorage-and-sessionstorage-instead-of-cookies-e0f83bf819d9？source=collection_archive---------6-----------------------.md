# 为什么应该考虑使用 localStorage 和 sessionStorage 而不是 cookies

> 原文：<https://javascript.plainenglish.io/why-you-should-consider-using-localstorage-and-sessionstorage-instead-of-cookies-e0f83bf819d9?source=collection_archive---------6----------------------->

![](img/834ddf19ede921e847432bb8692f6dd4.png)

Photo by Mollie Sivaram on Unsplash

当您想到 web 应用程序的数据持久性时，我敢打赌您首先想到的是 cookies。你没有错。Cookies 是在应用程序中保存数据的一种非常常见的方式。在需要由服务器读取数据的情况下，例如身份验证，它们绝对是正确的选择。然而，在大多数其他情况下，更好的工具是`localStorage`或`sessionStorage`。

那么什么是`localStorage`和`sessionStorage`？它们是 web 存储 API 的一部分，用于存储键值对。总的来说，这两者可以互换使用，因为它们都执行完全相同的功能并使用完全相同的语法。两者之间的主要区别在于存储的数据持续多长时间。在`sessionStorage`的情况下，数据将持续到窗口或标签关闭。然而，使用`localStorage` 时，数据将一直存在，直到浏览器缓存被手动清除，或者直到您的代码清除了应用程序中的数据。因为`localStorage`和`sessionStorage`执行相同的任务，所以从现在开始我只提到`sessionStorage`，我们都会同意我也在谈论`localStorage`。记住，语法是一样的！

## 为什么在 cookies 上使用`sessionStorage`？

使用`sessionStorage`代替 cookies 有很多好处。

*   数据保存在本地，这意味着它不能被服务器读取。如果您试图执行某种身份验证，这显然是一件坏事，但在其他情况下，它通常会消除 cookies 带来的安全问题。
*   `sessionStorage`可以存储的数据量要大得多。在大多数浏览器中，这是 10Mb，而 cookies 允许的是 4Kb。存储容量增加了 2500 倍！
*   所有现代浏览器都支持它。
*   语法非常简单，使得`sessionStorage`更容易使用。

所以现在你在想，“这太神奇了！如果他能给我一些例子，我可以立即开始使用 sessionStorage！”

## 创建新条目

使用语法`sessionStorage.setItem`创建新的`sessionStorage`条目，然后分配一个键值对。

如果需要更新现有条目，方法与创建新条目相同。使用`sessionStorage.setItem`，但不使用新键，而是使用现有键并输入新值。现有键现在将指向`sessionStorage`中的更新值。

## 读取现有条目

使用语法`sessionStorage.getItem`从`sessionStorage`中读取条目，然后调用所需的键。

## 删除现有条目

使用`sessionStorage.removeItem`方法可以删除现有的`sessionStorage`条目。

## 清除所有的东西！

还记得我在文章开头提到的保存到`localStorage`的数据会一直存在，直到用户手动清除浏览器缓存，或者直到您的应用程序清除来自`localStorage`的数据吗？如果你想清除一个条目，你可以使用前面提到的删除方法。但是如果你想清除所有已经保存到`localStorage`或者`sessionStorage`的数据呢？为此，您可以简单地使用`clear`方法。事实上，它太简单了，甚至不值得创建一个要点给你看。就叫`sessionStorage.clear()`。就是这样。

## 存储非字符串值

关于 sessionStorage 需要记住的一点是，您只能存储字符串值。这意味着，如果您想要存储更复杂的数据，比如来自 API get 请求的 JSON 响应，就需要进行更多的数据操作。为了存储数据，您需要使用`JSON.stringify`，然后以 JSON 的形式从会话存储中读取对象，您需要使用`JSON.parse`。

总而言之，如果您没有使用 sessionStorage 或 localStorage，那么您应该使用它们！这种语法使它们比 cookie 更容易使用，可以存储的数据量要大得多，而且由于数据存储在本地，因此消除了服务器读取 cookie 的安全风险。