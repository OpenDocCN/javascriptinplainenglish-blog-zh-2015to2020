# 更好的 JavaScript —事件队列

> 原文：<https://javascript.plainenglish.io/better-javascript-event-queue-3aa737e9c5a6?source=collection_archive---------3----------------------->

![](img/24703bba00d025a12f248940d06927ab.png)

Photo by [Product School](https://unsplash.com/@productschool?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 不阻塞 I/O 上的事件队列

JavaScript 是单线程语言，所以如果同步代码是长时间运行的，那么用同步代码阻塞 I/O 是不好的。

如果我们阻塞了事件队列，一段代码就可能阻塞整个程序。

所以我们不应该有这样的代码:

```
const text = downloadSync("http://example.com/file.txt");
console.log(text);
```

在我们的节目中。

等待文本被下载，然后返回。

这可能需要很长时间。

因此，JavaScript 为我们提供了异步运行代码的方法。

相反，我们可以写:

```
downloadAsync("http://example.com/file.txt", (text) => {
  console.log(text);
});
```

如果我们运行异步 JavaScript 代码，那么代码将被挂起，直到获得结果。

我们知道结果是在回调被调用时获得的。

当代码被挂起时，这段代码后面排队的东西就可以运行了。

如果我们正确地构建代码，我们就不必担心由于并发执行代码而导致的某些对象或变量的改变。

# 对异步代码使用回调

如果我们有一些简单的异步代码，那么我们可以使用回调来运行我们的代码。

例如，我们有:

```
downloadAsync("http://example.com/file.txt", (text) => {
  console.log(text);
});
```

它接受一个回调，当从服务器检索结果时调用这个回调。

这是创建异步代码最简单的方法。

当`downloadAsync`运行时，该功能被启动，但尚未执行操作。

一旦操作被执行，回调被运行，我们得到`text`。

如果我们必须按顺序运行多个异步操作，那么我们需要在第一个操作的回调中运行第二个操作。

所以我们可以写:

```
downloadAsync("http://example.com/foo.txt", (text1) => {
  console.log(text1);
  downloadAsync("http://example.com/bar.txt", (text2) => {
    console.log(text2);
  });
});
```

我们将第二个`downloadAsync`调用嵌套在第一个调用中。

如果我们需要按顺序运行更多的异步回调，那么我们会得到更多的嵌套。

这肯定是一个问题，因为我们不想嵌套它们。

为了避免这种情况，我们可以使用承诺。

# 对复杂的异步操作使用承诺

如果我们有很多需要按顺序运行的异步代码，那么嵌套的异步回调就会太混乱。

我们不想在代码中有太多层次的嵌套，因为很难调试和跟踪。

相反，我们用承诺来避免所有的嵌套。

例如，我们可以通过写下以下内容来创建承诺链:

```
promise
  .then((val) => {
    return promise2;
  })
  .then((val) => {
    return promise3;
  })
  .then((val) => {
    return promise4;
  })
  .then((val) => {
    return promise5;
  })
```

我们用回调函数调用`then`,回调函数返回一个承诺来调用另一个承诺。

`val`具有承诺解决的价值。

承诺也可以被拒绝。

为了从被拒绝的承诺中捕捉错误，我们调用`catch`方法:

```
promise
  .catch((err) => {
    console.log(err);
  })
```

这比嵌套回调好多了。

![](img/b47822e63c02644a3a695413bfdaea32.png)

Photo by [Levi Jones](https://unsplash.com/@levidjones?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该用长期运行的同步代码阻塞 JavaScript 代码的 I/O。

如果我们有任何可能长期运行的东西，我们应该使用异步回调或承诺。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**