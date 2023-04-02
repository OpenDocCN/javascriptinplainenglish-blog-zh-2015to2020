# 现代 JavaScript 的精华——异步和承诺

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-async-and-promises-38711c834d02?source=collection_archive---------9----------------------->

![](img/c435baea199eae085ea3207e9ee4c14b.png)

Photo by [Goran Ivos](https://unsplash.com/@goran_ivos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 异步编程。

# 同步和异步

同步代码总是在异步代码之前运行。

所以如果我们有:

```
setTimeout(function() { 
  console.log(2);
}, 0);
console.log(1);
```

然后我们得到:

```
1
2
```

外面的控制台日志是同步的，所以先运行。

回调的控制台日志在同步代码之后运行，因为它是异步的。

如果我们运行同步的东西，事件循环可能会被阻塞，因为整个应用程序是由单个进程管理的。

用户界面和其他计算都在一个线程中。

因此，如果一件事情运行，那么在那件事情完成之前，随后发生的任何事情都不能运行。

例如，如果我们有一个同步`sleep`函数，那么这将暂停整个程序的执行。

如果我们有:

```
function sleep(ms) {
  const start = Date.now();
  while ((Date.now() - start) < ms);
}console.log('start');
sleep(2000);
console.log('end');
```

然后我们看到`'start'`首先被记录，然后等待 2 秒，然后`'end'`被记录。

`sleep`函数使用了一个`while`循环，所以它是同步的。

这意味着它会阻碍整个程序的运行。

# 避免阻塞

为了避免阻塞 UI 线程，我们可以使用不同种类的异步代码，比如 web workers，或者`setTimeout`或者 Promises。

异步代码的一个例子是 Fetch API。

我们可以在不阻塞整个线程的情况下，通过发出 HTTP 请求来获取数据。

例如，我们可以写:

```
fetch('https://api.agify.io/?name=michael')
  .then(res => res.json())
  .then(res => {
    console.log(res);
  })
```

使用`fetch`函数获取数据。

它返回一个承诺，所以它是异步的。

然后`then`回调只有在结果准备好的时候才被调用。

如果不是，承诺就会暂停，直到得到一个结果。

# 异步结果

其他示例包括 Node.js 异步回调。

例如，`readFile`方法在读取文件时接受异步运行的回调。

我们可以写:

```
fs.readFile('foo.txt', {
    encoding: 'utf8'
  },
  function(error, text) { 
    if (error) {
      // ...
    }
    console.log(text);
  });
```

然后我们用`readFile`读`file.txt`。

# 复试

回调是有问题的，因为错误处理很复杂。

我们必须在每次回调时处理错误。

签名也不够优雅，因为输入和输出之间没有分离。

异步函数使用回调函数，回调函数不能返回任何东西。

构图也比较复杂。

节点样式回调一些问题。

我们必须检查`if`语句的错误。

重用错误处理程序更加困难。

并且提供默认处理程序也更加困难。

# 承诺

承诺是异步编程的一种模式，其中单个结果被异步返回。

它们比回调更好，因为它们可以被链接。

承诺由各种函数返回，并作为最终结果的占位符。

例如，一个承诺链可以写为:

```
asyncFunction(arg)
  .then(result => {
    console.log(result);
  });
```

如果`then`回调返回一个承诺，我们可以有不止一个`then`调用。

例如，我们可以写:

```
asyncFunction(arg)
  .then(result1 => {
    console.log(result1);
    return asyncFunction2(x, y);
  })
  .then(result2 => {
    console.log(result2);
  });
```

![](img/6eaed2e0dc810d1a3726ef6c003253c9.png)

Photo by [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

承诺是用 JavaScript 编写异步代码的一种更好的方式。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**