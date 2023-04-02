# JavaScript 异步编程简介

> 原文：<https://javascript.plainenglish.io/introduction-to-javascript-async-programming-48c15b709300?source=collection_archive---------7----------------------->

![](img/62a4a9a82a90c88408eb56086bdc53a6.png)

Photo by [Kasia Wanner](https://unsplash.com/@tueio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将看看如何用 JavaScript 进行异步编程。

# 异步性

在同步编程中，事情一次发生一件。

在异步或异步编程中，多件事情可以同时发生。

当一件事情发生时，我们的程序继续运行。

JavaScript 程序依靠异步编程而不是线程来运行多个进程。

我们等待一件事在后台完成，同时运行另一件事。

# 复试

异步编程的一种方法是回调。这些 ate 函数在慢速动作完成后被调用。

例如，一个基本的例子是`setTimeout`函数:

```
setTimeout(() => console.log("hello"), 500);
```

`setTimeout`函数在 500 毫秒后运行回调函数。

回调将`'hello'`记录到控制台。

然而，如果我们有一个接一个发生的不止一个异步动作，这就不能很好地工作。

例如，如果我们有:

```
readStorage("book", caches => {
  const [firstCache] = caches;
  readStorage(firstCache, info => {
    console.log(info);
  });
});
```

我们必须嵌套回调以异步读取缓存，然后运行另一个异步回调来记录信息。

嵌套越多，我们的代码就越难阅读。

因此，我们必须想出一个更好的方法。

# 承诺

承诺是一个异步动作，可能在某个时刻完成并产生一个值。

最简单的承诺是用`Promise.resolve`产生的。

例如，我们可以写:

```
const prom = Promise.resolve(1);
prom
  .then(val => console.log(val));
```

我们调用`Promise.resolve`来返回一个解析为 1 的承诺。

已解决意味着产生了一个值。

然后我们调用`then`并传入一个回调来获取解析后的值。

`then`方法也返回另一个承诺。

为了创建一个承诺，我们可以使用`Promise`作为构造函数。

例如，我们可以写:

```
const fs = require('fs');const readFile = (fileName) => {
  return new Promise(resolve => {
    fs.readFile(fileName, (err, data) => {
      resolve(data);
    });
  });
}
```

我们创建了一个用 Node.js `fs`模块读取文件的承诺。

解析后的值是由`resolve`函数产生的，我们将它与文件内容一起传入`data`参数。

# 失败

为了处理承诺中的失败，我们不能像处理同步代码那样用 try-catch 来处理它。

相反，我们使用`reject`函数拒绝具有错误值的承诺。

然后我们可以使用`catch`方法来处理错误情况。

例如，我们可以写:

```
const fs = require('fs');const readFile = (fileName) => {
  return new Promise((resolve, reject) => {
    fs.readFile(fileName, (err, data) => {
      if (err){
        return reject(err);
      }
      resolve(data);
    });
  });
}
```

现在我们拒绝定义`err`时的承诺。

一旦我们这样做了，我们可以如下使用`catch`:

```
readFile('foo.txt')
  .catch(err => console.log(err))
  .then(file => console.log(file))
```

未捕获的异常由环境处理，如果不处理，承诺拒绝将传播到环境。

# 承诺的集合

我们可以使用`Promise.all`方法同时运行一组不相关的承诺。

它以一系列承诺作为其论据。

例如，我们可以写:

```
Promise.all([
    Promise.resolve('foo'),
    Promise.resolve('bar')
  ])
  .then(([val1, val2]) => console.log(val1, val2))
```

数组中有两个不相关的承诺，所以我们可以使用`Promise.all`。

当我们调用`then`时，我们在`then`回调中得到一个数组值作为参数。

每个承诺的解决值都有一个条目。

# 异步函数

异步函数是上面提到的 promise 代码的简写。

`await`类似于`then`。

try/catch 类似于`catch`。

例如，我们可以重写:

```
readFile('foo.txt')
  .catch(err => console.log(err))
  .then(file => console.log(file))
```

收件人:

```
(async () => {
  try {
    const file = await readFile('foo.txt');
  } catch (err) {
    console.log(err)
  }
})();
```

我们调用`readFile`函数，用`await`得到解析后的值。

`try/catch`同`catch`。

![](img/2438bd5b2ea30273e591779e27447be3.png)

Photo by [Bradley Brister](https://unsplash.com/@bradley_brister?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

异步编程因承诺而变得简单。

我们可以使用`Promise`构造函数来创建我们自己的承诺。

`then`方法获取承诺的解析值。

`catch`从被拒绝的承诺中捕捉错误。

`async`功能是`then`和`catch`的较短语法。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**