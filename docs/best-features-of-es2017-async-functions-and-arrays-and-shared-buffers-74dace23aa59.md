# ES2017 的最佳特性—异步功能和阵列以及共享缓冲区

> 原文：<https://javascript.plainenglish.io/best-features-of-es2017-async-functions-and-arrays-and-shared-buffers-74dace23aa59?source=collection_archive---------4----------------------->

![](img/43c22f9d1bce1abd1ad40e39fefa4273.png)

Photo by [Elaine Casap](https://unsplash.com/@ecasap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2017 的最佳特性。

# `Async Functions and Array.prototype.forEach()`

`Array.prototype.forEach`不支持`async`和`await`语法。

例如，如果我们有:

```
async function downloadContent(urls) {
  urls.forEach(async url => {
    const content = await makeRequest(url);
    console.log(content);
  });
}
```

那么我们不会得到所有承诺的结果，因为`forEach`不会等待每个承诺完成。

相反，我们希望使用 for-of 循环来迭代每个异步函数，以获得我们的结果:

```
async function downloadContent(urls) {
  for (const url of urls) {
    const content = await makeRequest(url);
    console.log(content);
  }
}
```

for-of 循环知道`await`操作符，所以我们可以用它来循环运行所有的异步函数。

如果我们想并行运行异步函数，我们可以使用`Promise.all`:

```
async function downloadContent(urls) {
  await Promise.all(urls.map(
    async url => {
      const content = await makeRequest(url);
      console.log(content);
    }));
}
```

我们将 URL 映射到异步函数，这样我们就可以在承诺数组上调用`Promise.all`。

我们返回一个承诺，其解析值是承诺解析值的数组。

# 立即调用异步函数表达式

我们可以创建立即运行的异步函数。

例如，不写:

```
async function foo() {
  console.log(await promiseFunc());
}
foo();
```

我们可以写:

```
(async function () {
  console.log(await promiseFunc());
})();
```

它也可以是一个箭头函数:

```
(async () => {
  console.log(await promiseFunc());
})();
```

# 未处理的拒绝

当我们使用异步函数时，我们不必担心未处理的拒绝。

这是因为当我们遇到它们时，浏览器会向我们报告它们。

例如，我们可以写:

```
async function foo() {
  throw new Error('error');
}
foo();
```

然后我们会看到控制台中记录的错误。

# 共享数组缓冲区

ES2017 引入了共享数组缓冲区，让我们可以构建并发应用。

它们让我们在多个工作线程和主线程之间共享一个`SharedArrayBuffer`对象的字节。

缓冲区是共享，并被封装在一个类型化数组中，这样我们就可以访问它们。

我们可以在工人之间快速共享数据，工人之间的协调也简单快捷。

例如，我们可以通过编写以下内容来创建共享数组缓冲区:

```
const worker = new Worker('worker.js');const sharedBuffer = new SharedArrayBuffer(
  100 * Int32Array.BYTES_PER_ELEMENT);

worker.postMessage({
  sharedBuffer
});const sharedArray = new Int32Array(sharedBuffer);
```

我们在`worker.js`中创建了一个工人。

然后我们用`SharedArrayBuffer`创建了一个共享缓冲区。

它可以包含 100 个元素。

然后，为了与 worker 共享缓冲区，我们调用`postMessage`将缓冲区传递给 worker。

为了访问缓冲区的数据，我们创建了一个新的`Int32Array`实例。

然后在`worker.js` worker 中，我们通过写:

```
self.addEventListener('message', (event) => {
  const {
    sharedBuffer
  } = event.data;
  const sharedArray = new Int32Array(sharedBuffer);
  //...
});
```

我们监听`message`事件并获取`event.data`的`sharedBuffer`属性。

那么我们可以用同样的方法访问它。

![](img/dd340cca0234026fd9a0d6ecddadf29f.png)

Photo by [Jenn Kosar](https://unsplash.com/@foodwithaview?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

异步函数不能很好地处理现有的数组实例方法。

此外，我们可以使用共享数组缓冲区在主线程和工作线程之间共享数据。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[T3【plain English . io找到一切的链接！](https://plainenglish.io/)