# ES2017 的最佳特性—异步功能缺陷

> 原文：<https://javascript.plainenglish.io/best-features-of-es2017-async-functions-pitfalls-3588900c796b?source=collection_archive---------15----------------------->

![](img/b51dec5d6b2dc4176f38e261f136f9eb.png)

Photo by [Andrew Small](https://unsplash.com/@andsmall?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2017 的最佳特性。

# 异步函数的属性

异步函数不包装我们返回的承诺。

例如，如果我们有:

```
async function asyncFunc() {
  return Promise.resolve('foo');
}
```

那么如果我们有:

```
asyncFunc()
  .then(x => console.log(x))
```

那么`x`就是`'foo'`。

如果我们返回被拒绝的承诺，那么抛出的错误值将在`catch`回调中。

例如，我们可以写:

```
async function asyncFunc() {
  return Promise.reject(new Error('error'));
}
```

那么如果我们写:

```
asyncFunc()
  .catch(err => console.error(err));
```

而`error`是`Error`实例。

# 异步功能提示

如果我们在`=`右边的代码是一个承诺，我们永远不应该忘记`await`。

例如，我们不应该写:

```
async function asyncFunc() {
  const value = promiseFunc();
  //...
}
```

那是行不通的，因为没有`await`来等待承诺的结果。

`await`即使函数不返回任何东西也有意义。

我们可以使用承诺信号来暂停功能，直到事情完成。

例如，我们可以写:

```
async function asyncFunc() {
  await sleep(2000);
  //...
}
```

用我们自己的`sleep`功能使功能暂停 2 秒钟。

我们可以实现`sleep`,只要它返回一个承诺。

# 我们不需要总是等待

如果我们只想调用一个承诺，而不想等待它的结果，那么我们不需要向它添加`await`。

例如，我们可以写:

```
async function asyncFunc() {
  const writer = openFile('foo.txt');
  writer.writeLine('foo'); 
  writer.writeLine('bar'); 
  await writer.close(); 
}
```

我们没有将`await`添加到`writer.writeLine`方法调用中，因为我们不想等待结果。

这样，我们可以让我们的函数运行得更快，因为我们不必等待。

# `await`是顺序的，`Promise.all()`是平行的

我们必须记住`await`是顺序运行的。

并且`Promise.all`运行我们并行传递的承诺数组。

所以如果我们有:

```
async function foo() {
  const result1 = await promise1();
  const result2 = await promise2();
}
```

然后`await`等待每个异步功能完成，直到运行下一行。

另一方面，如果我们有:

```
async function foo() {
  const [result1, result2] = await Promise.all([
    promise1(),
    promise2(),
  ]);
}
```

然后`promise1`和`promise2`并行运行。

# 异步函数和回调

`await`只影响周围的异步功能。

所以异步函数不能在回调中`await`。

然而，回调可以是异步函数。

这使得回调很难使用。

例如，我们不能在非异步的`map`回调中使用`await`:

```
async function download(urls) {
  return urls.map(url => {    
    const content = await makeRequest(url);
    return content;
  });
}
```

我们在函数前面没有一个`async`关键字。

但是我们也不能写:

```
async function download(urls) {
  return urls.map(async url => {    
    const content = await makeRequest(url);
    return content;
  });
}
```

因为我们不能从回调中得到每个异步函数的结果。

等待只在回调中完成。

我们应该做的是将我们的 URL 映射到承诺，然后在承诺数组上调用`Promise.all`。

例如，我们写道:

```
async function download(urls) {
  const promiseArray = urls.map(async (url) => {
    const content = await makeRequest(url);
    return content;
  });
  return await Promise.all(promiseArray);
}
```

我们调用`map`将 URL 数组映射到承诺数组，然后我们返回我们的`Promise.all`调用以返回一个承诺，该承诺解析为数组中所有承诺的结果。

![](img/3e15c2d8bc282ca94b9a6a22f60c8657.png)

Photo by [Serg B](https://unsplash.com/@sergeebee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

异步函数使用起来很方便，但是如果我们不小心的话，它可能会被误用。

所以我们应该确保正确使用它们以避免意想不到的结果。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**