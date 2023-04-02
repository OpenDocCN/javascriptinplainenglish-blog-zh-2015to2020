# 你能用 20 行代码实现一个异步等待函数吗？

> 原文：<https://javascript.plainenglish.io/can-you-implement-an-async-await-function-with-just-20-lines-of-code-1f5b187c6a4f?source=collection_archive---------10----------------------->

## 有一件事会让你在面试中脱颖而出

![](img/6bf3228ee094ae3952967dc44403cdd9.png)

Photo by [Moritz Kindler](https://unsplash.com/@moritz_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果面试官让你手写一个异步函数，你能完成任务吗？

异步函数是生成器函数的语法糖。在本文中，我将首先研究`async`关键字和生成器函数如何协同工作来管理异步编程。然后，我将与您一起使用生成器函数实现一个简单的异步函数。

首先，让我们看一个使用 async/await 关键字的函数。

```
const getData = () => new Promise(resolve => setTimeout(() => resolve("data"), 1000))async function test() {
  const data = await getData()
  console.log('data: ', data);
  const data2 = await getData()
  console.log('data2: ', data2);
  return 'success'
}// The function will print `data` after 1 second, then `data2` after another second and then 'success'
test().then(res => console.log(res))
```

如果我们想用生成器函数表达上面的例子，应该怎么写代码？

我们可以这样写:

```
const getData = () => new Promise(resolve => setTimeout(() => resolve("data"), 1000))function* testG() {
  // await is translated as yield
  const data = yield getData()
  console.log('data: ', data);
  const data2 = yield getData()
  console.log('data2: ', data2);
  return 'success'
}
```

众所周知，生成器功能不会自动执行。每次调用它的下一个方法时，程序都在下一个产量处停止。

有了这个特性，我们可以编写一个自动执行的函数，并让生成器函数完全实现异步函数。

```
function asyncToGenerator(fn){
  // ...
}

var test = asyncToGenerator(
    function* testG() {
      // await is translated as yield
      const data = yield getData()
      console.log('data: ', data);
      const data2 = yield getData()
      console.log('data2: ', data2);
      return 'success'
    }
)test().then(res => console.log(res))
```

嗯，我们还没有写 asyncToGenerator 函数，但是总体思路已经定了。AsyncToGenerator 应该接受一个生成器函数并返回一个 promise 对象。这里的关键是函数使用 yield 来划分异步进程，那么这应该如何自动化呢？

在写这个函数之前，我们模拟手动调用生成器函数，然后一步一步地走完这个过程，这有助于我们后面更好地思考。

```
function* testG() {
  const data = yield getData()
  console.log('data: ', data);
  const data2 = yield getData()
  console.log('data2: ', data2);
  return 'success'
}
```

我们首先调用`testG`生成一个迭代器:

```
// An iterator is returned
var gen = testG()
```

然后第一次执行`next`:

```
// After the first call `next`, the program stays at the first yield
// The return promise object contains the data needed for `data`
var dataPromise = gen.next()
```

返回一个承诺，是第一次执行`getData()`返回的承诺。

```
const data = yield getData()
```

但是注意:这段代码被分成左右两部分，对`next`的第一次调用实际上只是停留在`yield getData()`，这里`data`的值没有确定。

那么`data`的值是什么时候确定的呢？在下一次调用`next`时，传入的参数被接受为最后一次让步之前的值。

也就是当我们再次调用`gen.next(‘a value to data’)`时，数据的值将被确定为`‘a value to data’`。

```
gen.next('a value to data')// This is when the data is assigned
const data = yield getData()console.log('data: ', data);// Then go to next yield
const data2 = yield getData()
```

这是生成器函数理解的一个难点，但为了达到我们的目的，我们必须学习它。

利用这个特性，我们可以控制产出的过程，然后实现异步功能。

这种函数调用看起来像回调地狱，让 async/await 通过生成器函数实现。

这是我们最后的 22 行代码:

那么我们来分析一下这段代码:

`

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****