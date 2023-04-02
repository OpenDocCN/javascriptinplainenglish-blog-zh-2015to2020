# ES2017 的最佳特性—异步功能

> 原文：<https://javascript.plainenglish.io/best-features-of-es2017-async-functions-2a18cb7e9140?source=collection_archive---------7----------------------->

![](img/31588c03c11cc4802da97f80d57eb5c7.png)

Photo by [Zoltan Tasi](https://unsplash.com/@zoltantasi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2017 的最佳特性。

# 异步函数

异步功能是 ES2017 发布的一大功能。

这是`then`方法的一个有用的简写。

我们可以用以下方式声明异步函数:

*   异步函数声明:`async function foo() {}`
*   异步函数表达式:`const foo = async function () {};`
*   异步方法定义:`let obj = { async foo() {} }`
*   异步箭头功能:`const foo = async () => {};`

异步函数总是返回承诺。

所以如果我们有:

```
async function asyncFunc() {
  return 'foo';
}
```

然后`asyncFunc`返回解析到`'foo'`的承诺。

然后我们可以通过写来使用它:

```
asyncFunc()
  .then(x => console.log(x));
```

为了拒绝一个承诺，我们抛出一个错误:

```
async function asyncFunc() {
  throw new Error('error');
}
```

然后，我们可以通过编写以下内容来捕捉错误:

```
asyncFunc()
  .catch(x => console.log(x));
```

我们可以用`await`处理承诺的结果和错误。

`await`运算符只允许在异步函数中使用。

它等待它的操作数，这总是一个要解决的承诺。

如果承诺实现了，那么`await`的结果就是它的实现值。

如果承诺被拒绝，那么`await`抛出拒绝值。

所以我们可以写:

```
async function asyncFunc() {
  const result = await promise;
  console.log(result);
}
```

其中`promise`是我们等待结果的承诺。

这等同于:

```
async function asyncFunc() {
  return promise
    .then(result => {
      console.log(result);
    });
}
```

这种语法的好处是我们可以用更短的方式处理多个承诺。

所以我们可以写:

```
async function asyncFunc() {
  const result1 = await promise1();
  console.log(result1);
  const result2 = await promise2();
  console.log(result2);
}
```

这等同于:

```
function asyncFunc() {
  return promise1()
    .then(result1 => {
      console.log(result1);
      return promise2();
    })
    .then(result2 => {
      console.log(result2);
    });
}
```

如我们所见，它要短得多。

要并行运行多个承诺，我们可以使用`Promise.all`:

```
async function asyncFunc() {
  const [result1, result2] = await Promise.all([
    promise1(),
    promise2(),
  ]);
  console.log(result1, result2);
}
```

这与以下内容相同:

```
async function asyncFunc() {
  return Promise.all([
      promise1(),
      promise2(),
    ])
    .then(([result1, result2]) => {
      console.log(result1, result2);
    });
}
```

为了处理错误，我们像处理同步函数一样使用 try-catch:

```
async function asyncFunc() {
  try {
    await promiseFunc();
  } catch (err) {
    console.error(err);
  }
}
```

这与以下内容相同:

```
function asyncFunc() {
  return promiseFunc()
    .catch(err => {
      console.error(err);
    });
}
```

异步函数通过使用生成器来等待结果，直到它继续执行。

每次添加`await`操作符时，它都会等待结果。

一旦承诺兑现，那么异步函数将继续运行。

异步函数同步启动，异步结算。

异步函数的结果总是一个承诺。

承诺是在异步函数启动时创建的。

然后运行它的主体。

可能以`return`或`throw`结束。

或者可以用`await`暂停，一旦获得结果就继续。

然后最后，诺言被归还。

![](img/ebf0b78b6512796e5fa36f43b454ee7b.png)

Photo by [Free To Use Sounds](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

异步函数是编写 promise 代码的一种很好的速记方式。

这些函数总是返回承诺。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**