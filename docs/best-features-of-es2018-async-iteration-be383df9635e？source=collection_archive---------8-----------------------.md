# ES2018 的最佳特性—异步迭代

> 原文：<https://javascript.plainenglish.io/best-features-of-es2018-async-iteration-be383df9635e?source=collection_archive---------8----------------------->

![](img/a23a1b865ad7e523a8d3b6a830b5f6f7.png)

Photo by [sydney Rae](https://unsplash.com/@srz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2018 的最佳特性。

# 异步迭代

在 ES6 中，JavaScript 引入了 for-of 循环。

它让我们同步迭代可迭代对象。

然而，没有办法异步迭代。

ES2018 引入了异步迭代功能来填补这一空白。

同步迭代适用于任何具有`Symbol.iterator`方法的对象，这是一种生成器方法。

当调用`next` next 方法时，它按顺序返回每一项。

返回的对象具有从生成器返回的值，这是迭代器命中的当前条目。

它还具有`done`属性来指示是否所有 th 值都已返回。

例如，如果我们有:

```
const iterable = ['foo', 'bar'];
const iterator = iterable[Symbol.iterator]();
```

然后当我们打电话时:

```
iterator.next()
```

第一次，我们得到:

```
{ value: 'foo', done: false }
```

然后当我们再次调用它时，我们得到；

```
{ value: 'b', done: false }
```

当我们再次调用它时，我们得到:

```
{ value: undefined, done: true }
```

迭代是同步的，所以我们不能异步迭代。

我们希望能够逐个调用可迭代对象中的承诺。

这可以通过 for-await-of 循环来解决。

例如，我们可以这样使用它:

```
async function main() {
  const arr = [
    Promise.resolve('foo'),
    Promise.resolve('bar'),
  ];
  for await (const x of arr) {
    console.log(x);
  }
}
main();
```

我们有一系列的承诺，我们迭代通过。

它们会被一个接一个地运行。

所以我们得到:

```
foo
bar
```

记录在控制台日志中。

for-await-of 循环也适用于同步可重复项。

所以我们可以写:

```
async function main() {
  for await (const x of ['foo', 'bar']) {
    console.log(x);
  }
}
main();
```

我们记录了同样的结果。

# 异步发电机

异步生成器让我们创建异步可迭代的。

异步生成器只是一个返回承诺的生成器函数。

例如，我们可以通过编写以下代码来创建一个异步生成器函数:

```
async function* createAsyncIterable(arr) {
  for (const elem of arr) {
    yield elem;
  }
}
```

普通生成器返回一个生成器对象。

每次调用`next`方法都会返回一个具有属性`value`和`done`的对象。

异步生成器返回一个生成器对象。

每个`next`调用返回一个具有属性`value`和`done`的对象的承诺。

当我们调用一个异步生成器时，JavaScript 引擎将在调用下一个之前等待承诺被解决。

每个异步发电机都有一个队列，承诺用`yield`或`throw`解决。

当`next`被调用时，一个承诺被排队。

除非异步生成器已经在运行，否则它会继续运行并等待承诺完成。

可以用`yield`、`throw`或`return await`结束。

一旦完成，承诺就会被退回。

约定承诺的结果以异步方式交付。

![](img/40fd8bad4b4d32c783c409f31e2a82d7.png)

Photo by [Gabriel Gusmao](https://unsplash.com/@gcsgpp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

for-await-of 循环让我们通过顺序返回每个值的承诺来遍历异步生成器。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**