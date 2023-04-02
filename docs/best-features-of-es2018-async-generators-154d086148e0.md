# ES2018 的最佳特性——异步发电机

> 原文：<https://javascript.plainenglish.io/best-features-of-es2018-async-generators-154d086148e0?source=collection_archive---------14----------------------->

![](img/42d53980ab495b315622c7d031e7fc7e.png)

Photo by [Jamie Street](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2018 的最佳特性。

# `yield*`异步发电机中

`yield*`异步发电机的工作方式类似于普通发电机。

如果我们有:

```
async function* gen() {
  yield 'foo';
  yield 'bar';
  return 'baz';
}
```

然后我们可以通过写来使用它:

```
(async function() {
  for await (const x of gen()) {
    console.log(x);
  }
})();
```

我们得到了:

```
foo
bar
```

记录在案。

与普通生成器一样，for-of 循环会忽略返回值。

`yield*`的操作数可以是任何异步可迭代的。

Sync iterables 会自动转换为 Sync iterables，就像 for-await-of 一样。

# 异步迭代错误

在正常的生成器中，`next`可以抛出异常。

有了异步发电机，`next`就可以拒绝它回报的承诺。

例如，我们可以写:

```
async function* asyncGen() {
  throw new Error('error');
}
```

在异步生成器中引发错误。

然后，我们可以通过编写以下内容来捕捉错误:

```
asyncGen().next()
  .catch(err => console.log(err));
```

我们得到记录在`catch`回调中的错误对象。

`next`退回被拒绝的承诺。

# 异步函数和异步生成器函数之间的不同

异步函数立即返回一个承诺。

用`return`实现承诺或用`throw`拒绝承诺。

所以我们可以写:

```
(async function () {
    return 'foo';
})()
.then(x => console.log(x));
```

或者:

```
(async function() {
  throw new Error('error');
})()
.catch(x => console.log(x));
```

异步生成器立即返回一个异步可迭代对象。

并且`next`的每次调用都用`yield x`返回一个承诺，用`{value: x, done: false}`完成当前的承诺。

它还可以用`err`抛出当前承诺的错误。

所以我们可以写:

```
async function* genFn() {
  yield 'foo';
}
const gen = genFn();
gen.next().then(x => console.log(x));
```

那么`x`就是`{value: “foo”, done: false}`。

# 将异步可迭代对象转换为数组

我们可以通过循环异步 iterable 将一个异步变量转换成一个数组，然后将一个值压入一个常规数组。

例如，我们可以写:

```
async function convertToArr(asyncIterable) {
  const result = [];
  const iterator = asyncIterable[Symbol.asyncIterator]();
  for await (const v of iterator) {
    result.push(v);
  }
  return result;
}
```

创建一个采用`asyncIterable`的函数，然后将项目推入数组并返回。

它返回一个解析到`result`数组的承诺。

然后我们可以通过写来使用它:

```
async function* genFn() {
  yield 'foo';
  yield 'bar';
}(async () => {
  const arr = await convertToArr(genFn());
  console.log(arr);
})()
```

我们创建了一个异步生成器，并将它的返回结果传递给我们之前创建的`convertToArr`函数。

由于它返回一个解析为数组的承诺，我们将在记录它时看到值。

# 异步生成器的内部属性

异步生成器有两个内部属性。

`[[AsyncGeneratorState]]`具有发电机当前所处的状态。

可以是`"suspendedStart"`、`"suspendedYield"`、`"executing"`、`"completed"`或`undefined`。

没有完全初始化就是`undefined`。

`[[AsyncGeneratorQueue]]`保存`next`、`throw`或`return`的未决调用。

每个队列有 2 个字段，分别是`[[Completion]]`和`[[Capability]]`。

`[[Completion]]`具有用于`next`、`throw`或`return`的参数，该参数导致条目排队。

`next`、`throw`或`return`表示创建条目的方法，并确定出队后发生的情况。

`[[Capability]]`是承诺的能力。

![](img/266f726d9b8e4fdba15f1a2771bf324b.png)

Photo by [Angelo Pantazis](https://unsplash.com/@angelopantazis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`yield *`可以与异步生成器一起使用来调用另一个生成器。

异步函数和异步生成器之间也有很大的区别。

## **简明英语 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**