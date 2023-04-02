# JavaScript 中的异步等待

> 原文：<https://javascript.plainenglish.io/async-await-e949a0d06d63?source=collection_archive---------2----------------------->

![](img/31c453e51feeff4a7878b6b932c73b01.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/bubbles?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**TL；博士:**

*   await 关键字接受一个返回承诺的函数，并返回该承诺的 *resolved* 值。
*   您只能在用`async`关键字声明的函数中使用`await`关键字。
*   `async`功能是如何工作的？
*   返回承诺时，不需要在`async`函数的`return`语句中使用`await`。
*   你可以使用`Promise.all([…])`和`await`一次执行多个承诺。
*   您可以使用`for/await`循环异步遍历承诺。

# 介绍

ES2017 引入了两个关键字，为 JavaScript 中的异步编程引入了一个全新的范式。`async`和`await`极大地简化了我们使用承诺的方式，并允许我们以同步的方式编写基于承诺的异步代码。尽管`async`和`await`隐藏了裸承诺所需的所有代码，但了解承诺如何工作以及它们提供的 API 以充分利用`async/await`仍然非常重要。

如果你不知道承诺是如何工作的，我鼓励你阅读我之前的两篇文章，关于你需要知道的关于承诺的要点。[第 1 部分](https://medium.com/javascript-in-plain-english/javascript-promises-part-1-70cbc4d188e5?source=friends_link&sk=cd5a4af398827a420386c3a66dce3e91)介绍了承诺何时在事件循环中执行，而[第 2 部分](https://medium.com/javascript-in-plain-english/javascript-promises-part-2-f13184b46bd7?source=friends_link&sk=be21028491f764ad9c3ba8c45b1c41b7)介绍了承诺如何工作，它们提供的 API 以及一些常用的模式。

说完这些，让我们来看看`async/await`是如何工作的！

# 等待表达式

关键字`await`采用一个返回承诺的函数，并返回该承诺的*解析*值。如果实现了承诺的解析，则返回实现的值，否则它*抛出*拒绝原因。让我们看一个例子:

```
const users = await fetch('/users');
```

假设`fetch(…)`返回一个承诺，表达式`await fetch(…)`会等待`fetch(…)`被*摆平*。如果`fetch(…)`的解决方案是实现，那么`users`被分配返回值。然而，如果`fetch(…)` 因为某种原因遇到错误并被拒绝，那么`await fetch(…)`表达式将*抛出*拒绝原因。

重要的是要注意到,`await`表达式不会导致你的程序停止，直到指定的承诺完成。你可以认为`await`只是隐藏了用于处理承诺解析的`.then(…, …)`调用。

# 异步函数

因为任何使用`await`的代码都是异步的，所以有一条规则你必须绝对遵守:*你只能在已经用* `*async*` *关键字*声明的函数中使用 `*await*` *关键字。这里有一个例子:*

```
async getFoo() {
  const foo = await fetch('/api/v1/getFoo');
}
```

您应该注意，当您将函数声明为`async`时，这意味着该函数将返回一个承诺，即使函数体中没有出现与承诺相关的代码。请考虑以下情况:

```
async addFiftyTwo(number) {
  const plusFiftyTwo = number + 52;
  return plusFiftyTwo;
}
```

## 在后台

当你观察`async`函数如何在幕后工作时，你会发现它们只是返回承诺的函数，正如本文中描述的[。让我们看一个例子:](https://medium.com/javascript-in-plain-english/javascript-promises-part-2-f13184b46bd7?source=friends_link&sk=be21028491f764ad9c3ba8c45b1c41b7)

```
async function get52() {
  return 52;
}// The following works exactly the same
function get52() {
  return new Promise((resolve, reject) => {
    resolve(52);
  });
}// Which can also be written as 
function get52() {
  return Promise.resolve(52);
}
```

这就是为什么理解承诺的概念对于有效使用`async/await`仍然非常重要。`async/await`只是围绕承诺的*句法糖*，所以如果你不知道如何有效地使用承诺，你很可能也不会有效地使用`async/await`。接下来让我们看看一些与`async/await`一起使用的常见模式。

# 常见模式

## 回报承诺

新开发人员在开始使用`async/await`时会犯的一个常见错误是什么时候使用`await`以及什么时候不使用。请考虑以下情况:

```
async someFunc() {
  return await fetch('/api/v1/getFoo');
}async someOtherFunc() {
  const foo = await someFunc();
}
```

如前所述，`async`函数返回一个承诺，该承诺解析为从函数返回的的值*并且`await`在继续执行`async`函数之前等待承诺被解析。如果一个`async`函数返回一个承诺作为`return`值，那么你不需要在`return`关键字后使用`await`。让我们看看那会是什么样子*

```
async someFunc() {
  return fetch('/api/v1/getFoo');
}async someOtherFunc() {
  const foo = await someFunc();
}
```

你会怎么读这个？`someFunc()`返回一个承诺，该承诺解析为另一个承诺，该承诺解析为来自 API 调用的响应。因此，`someOtherFucn()`中的`await`将等待内部最大承诺被解析，并将最后一个值赋给变量 foo。

## 等待多重承诺

假设您已经编写了一个类似如下的`getJSON`函数

```
async function getJSON(url) {
    const response = await fetch(url);
    const body = await response.json();
    return body;
}
```

现在您想从外部 API 获取一些 JSON 数据，所以您这样做

```
async function doSomething() {
  const users = await getJSON('/api/v1/users');
  const themes = await getJSON('/api/v1/themes');
}
```

执行该函数所需的时间完全取决于两个`getJSON`调用何时解决。这意味着如果第一次调用花了 5 秒，第二次调用直到那时才开始，如果第二次调用也花了 5 秒，那么该功能总共需要 10 秒才能完成。如果两个调用互不依赖，这可能是不必要的。这是新开发人员在使用`async/await`时经常犯的错误。那么怎么修呢？Promise API 提供了一个我们可以使用的名为`all([…])`的静态方法。让我们看看这是如何工作的。

**注:** *我在这里* *的帖子里讨论了* `*Promise.all([…])*` *和其他 Promise API 提供* [*的方法。*](https://medium.com/javascript-in-plain-english/javascript-promises-part-2-f13184b46bd7?source=friends_link&sk=be21028491f764ad9c3ba8c45b1c41b7)

```
async function doSomething() {
  const [users, themes] = await Promise.all([
    getJSON('/api/v1/users'),
    getJSON('/api/v1/themes')
  ]);
}
```

这将同时执行两个`getJSON()`调用，从而减少`doSomething()`的执行时间。现在你可能会想，为什么把`await`放在`Promise.all()`前面，而不是`getJSON`呼叫前面。这是因为`Promise.all([…])`它自己返回一个承诺，这个承诺的实现会产生一个数组，该数组由传入的承诺列表中的值组成。我之前提到的帖子对此有更详细的描述。

## 异步迭代

迭代器和异步迭代器超出了本文的范围，所以我们将只讨论如何将`async/await`用于异步迭代。异步迭代器就像常规迭代器一样，但基于承诺，可以用于新形式的`for/of`循环、`for/await`。

像常规的`await`表达式一样，`for/await`循环是基于承诺的。异步迭代器上的`next()`方法产生一个承诺，`for/await`循环等待该承诺解析，将实现值赋给循环变量，并运行循环体。然后它重新开始，从迭代器获得另一个承诺，并等待新的承诺被解析。让我们看一个如何使用它的例子，在下面的例子中，我们将使用一个普通迭代器*(不是异步迭代器)*，但是同样的概念也适用。

假设您有一个 URL 数组，您希望对其进行循环并得到一个承诺数组:

```
const urls = [url1, url2, url3];
const promises = urls.map((url) => fetch(url));
```

现在，我们可以在这里使用 Promise.all([…])并等待所有的承诺被解决，但是如果我们需要它们一回来就得到结果呢？然后呢？我们可以这样做:

```
for(const promise of promises) {
  const response = await promise;
  // do something with response here...
}
```

这完全可以接受，没有错。由于迭代器返回承诺，我们也可以利用如下所示的`for/await`循环:

```
for await (const response of promises) {
  //do something with response here...
}
```

`for/await`循环在循环中使用了`await`调用，使我们的代码更加紧凑，但是这两个例子做了完全相同的事情。需要注意的一点是，这两个例子只有在声明为`async`的函数中才会起作用:`for/await`循环在这方面与常规的`await`表达式是一样的。

# 结论

现在，你应该对`async/await`及其工作原理有了很好的理解。我们分别讨论了关键词，并简要介绍了`async`函数是如何工作的。之后是使用`async/await`的一些常见模式，以及新开发人员在开始使用`async/await`时会犯的一些常见错误。

像往常一样，让我知道你对 async/await 的体验。你在使用它时遇到了什么问题，以及你可能有什么问题。

下次见，干杯！