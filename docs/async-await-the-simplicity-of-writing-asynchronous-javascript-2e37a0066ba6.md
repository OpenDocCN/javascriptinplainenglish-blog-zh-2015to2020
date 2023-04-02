# Async/Await:编写异步 JavaScript 的简单性

> 原文：<https://javascript.plainenglish.io/async-await-the-simplicity-of-writing-asynchronous-javascript-2e37a0066ba6?source=collection_archive---------4----------------------->

![](img/b2dbed70caa66c8a4262c75d0965e5ff.png)

# Async/Await:编写异步 JavaScript 的简单性

JavaScript 的定义特性之一是它是同步的。它是单线程的，这意味着在任何时候只能执行一个页面上的一个 JavaScript 代码块，并且一行接一行地执行。看起来很简单，对吧？确实如此，只要您不等待从外部数据源返回任何数据。

但是，当您发送一个获取请求并需要在下一个代码块中使用请求的返回值时，会发生什么呢？如果任其自生自灭，JavaScript 引擎将:

1.  发出获取请求
2.  返回一个叫做**承诺**的特殊 JavaScript 对象
3.  继续执行下一个代码块
4.  请求完成后，要么解决承诺，要么返回错误
5.  一个块接一个块地继续同步执行代码

上面的序列描述了**异步 JavaScript，**，因为您可以在不阻塞线程的情况下发出获取请求。在处理和完成初始提取请求的同时，您的程序可以继续执行代码。

根据代码的不同，这个序列可能完全没问题。您的程序可能不需要获取请求的返回值来运行后续代码。不过，通常情况下，请求之后的代码需要获取数据才能执行。

在我们进入异步 JavaScript 之前，我们将讨论承诺实际上是什么。这对于理解异步 JavaScript 至关重要，因为只有返回 Promise 对象才有可能。如果你已经对承诺有了很好的理解，你可以进入下一部分。

# 什么是承诺？

promise 是一个特殊的 JavaScript 对象，之所以这样称呼它，是因为它的功能与现实生活中的 Promise 非常相似。在软件工程之外，承诺是指某人对另一个人做出的承诺。我承诺要做一些事情，你很快就会看到我是否会实现这个承诺。

JavaScript 中的承诺是相同的，但是上面提到的“某事”是一个请求。您发出获取请求，并获得一个承诺对象——一个完成获取请求的承诺，此后不久，您就会知道该承诺是实现了还是失败了。

在这里，“完成”意味着成功地发布、获取、更新或删除数据库中的某些内容。“失败”意味着您会得到一个错误消息，提示失败。我们想关注的部分是“此后不久”的时间线。如果您的获取请求返回大量数据，那么这段时间将会相应增加。但是它从来都不是即时的——即使对于少量的数据也是如此。承诺总是被返回，允许 JavaScript 引擎继续执行下一个代码块，但是有些承诺比其他的更快解决。

# 异步 JavaScript

多亏了承诺，异步 JavaScript 才成为可能。如果 fetch 请求没有返回承诺，JavaScript 引擎将不得不等待 fetch 请求完成，以便继续下一个代码块，因为它是单线程的。对于一个需要 30 秒甚至几分钟的大型请求，所有代码都会被阻止运行，直到它完成为止— *而不是*这是一个很好的用户体验。

然而，有时我们需要等待。希望我们正在等待的请求只需要几秒钟或更短的时间，但是不管怎样，我们都需要它完成，以便后续的代码能够正确执行。例如，如果下一个代码块处理从请求返回的数据，如果在请求完成之前执行该代码就不会很顺利。

如果您一直在使用 JavaScript，您可能已经看到了用于处理这个问题的 then/catch 语法。然后/catch 工作完全正常，但是说实话，它会变得很难看*快*。如果你只是链接一个命令，这可能是好的，但是一系列的命令会变得混乱，很难理解或使用。

# 异步/等待

幸运的是，我们可以使用一些语法糖来做与 async/await 中一系列复杂的 then/catch 命令完全相同的事情。我们需要做的不是长链，而是在定义函数时使用“async”关键字，这将允许你使用“await”关键字向引擎指定程序*在继续之前需要等待哪个代码块的响应。就这么简单。你告诉引擎，“嘿，我们将在这个函数中有一些异步代码，但是你需要在你看到‘await’的地方等待响应”。*

这个选项优于 then/catch 的地方在于，你可以像以前一样准确地编写代码*和*，只需在需要的地方添加几个关键字。这些都不是将整个代码块包装成。然后()并创建长而复杂的链。看看下面用两种不同方式写的同一个调用。第一个使用 then/catch，第二个使用 async/await:

```
const getUsersThen = () => {
 return fetch(`localhost:3000/users`, {
 method: 'GET',
 headers:{
 'Content-Type': 'application/json'
 }
 })
 .then(res => res.json())
 .then(res => {
 console.log(res)
 });
};const getUsersAwait = async (req, res) => { const users = await fetch(`localhost:3000/users`, { method: 'GET', headers:{ 'Content-Type': 'application/json' } }); const usersJson = users.json(); console.log(usersJson); };
```

这是一个非常简单的函数，我们只需将 JSON 响应记录到控制台，但是您已经可以看到 async/await 版本要干净得多。这种差异随着每一个后续命令——随着每一个不再需要的链式‘then’而被放大。有了 async/await，编写异步 JavaScript 变得简单多了。

*原载于 2019 年 11 月 27 日*[*https://www . syntx . io*](https://www.syntx.io/javascript/async-await-the-simplicity-of-writing-asynchronous-javascript/)*。*