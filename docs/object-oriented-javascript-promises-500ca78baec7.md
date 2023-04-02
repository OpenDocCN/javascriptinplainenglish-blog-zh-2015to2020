# 面向对象的 JavaScript——承诺

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-promises-500ca78baec7?source=collection_archive---------9----------------------->

![](img/7fc4bc3e180945bae7b14de6be4b4f9b.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 JavaScript 承诺。

# 承诺

承诺是回电的另一种选择。

承诺让我们检索异步函数调用的结果。

承诺比回调更容易，给了我们更多可读的代码。

他们可以挑战几个州。

承诺可能是悬而未决的，这意味着结果还没有准备好。

这是初始状态。

结果准备好了，承诺就兑现了。

如果有错误，那么结果将被拒绝。

当一个待定的承诺被履行或拒绝时，运行由`then`排队的相关回调。

这比异步回调要好。

如果我们想要运行许多异步函数，那么我们必须重复嵌套它们。

例如，我们写道:

```
asyncFunction(arg, result => {
  asyncFunction(arg, result => {
    //...
  })
})
```

多次运行异步函数。

如果我们必须做更多，那么就会有更多的嵌套，阅读起来会更困难。

如果`asyncFunction`是一个承诺，那么我们可以给`then`回电。

承诺兑现后的回拨运行:

```
asyncFunction(arg)
  .then(result => {
    //...
  });
```

我们只是不停地呼唤`then`，直到我们呼唤出所有我们想要的承诺:

```
asyncFunction(arg)
  .then(result => {
    //...
    return asyncFunction(argB);
  })
  .then(result => {
    //...
  })
```

`then`回调返回一个承诺，所以我们可以继续调用`then`直到我们完成。

为了捕捉承诺错误，我们可以用回调来调用`catch`方法。

例如，我们可以写:

```
readFileWithPromises('text.json')
  .then(text => {
    //...
  })
  .catch(error => {
    console.error(error)
  })
```

# 创造承诺

我们可以用`Promise`构造函数创建承诺。

例如，我们可以写:

```
const p = new Promise(
  (resolve, reject) => {
    if (...) {
      resolve(value);
    } else {
      reject(reason);
    }
  });
```

我们传入了一个回调函数，它有`resolve`和`reject`参数，这些都是函数。

`resolve`用给定的价值兑现了承诺。

`reject`拒绝有值的承诺。

我们可以将 than 与`then`和`catch`一起使用，就像我们在前面的例子中所做的那样:

```
p
  .then(result => {
    //...
  })
  .catch(error => {
    //...
  })
```

当我们在`then`回调中抛出错误时，如果在抛出错误的`then`之后调用`catch`,它也会被捕获:

```
readFilePromise('file.txt')
  .then(() => {
    throw new Error()
  })
  .catch(error => {
    'Something went wrong'
  });
```

# Promise.all()

`Promise.all`让我们并行运行一个承诺数组，并返回一个承诺，该承诺解析为数组中所有承诺的数组。

如果所有的承诺都实现了，那么承诺结果的数组将在 then `then`回调的参数中返回。

例如，我们可以写:

```
Promise.all([
    p1(),
    p2()
  ])
  .then(([r1, r2]) => {
    //
  })
  .catch(err => {
    console.log(err)
    ''
  })
```

我们用返回承诺的`p1`和`p2`来称呼`Promise`。

那么`r1`和`r2`就是结果。

`catch`如果任何一个承诺被拒绝，则捕捉其中的错误。

![](img/6022c3e962e66d6756bc2aae5ffe0c43.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`Promise`构造函数创建承诺。

`Promise.all`并行运行多极承诺，并返回一个解析为所有结果的数组的承诺。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**