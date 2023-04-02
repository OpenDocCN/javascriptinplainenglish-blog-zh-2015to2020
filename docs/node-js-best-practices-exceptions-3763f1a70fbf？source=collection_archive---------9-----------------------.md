# Node.js 最佳实践—例外

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-exceptions-3763f1a70fbf?source=collection_archive---------9----------------------->

![](img/1d996c11dd7d862872c28aad8154b361.png)

Photo by [Vasily Koloda](https://unsplash.com/@napr0tiv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用异步错误处理的承诺

我们应该使用异步错误处理的承诺。

如果我们有许多嵌套的回调函数，那么用回调函数的方式处理异步错误将会很糟糕。

节点风格的回调不允许我们在没有嵌套的情况下链接异步调用。

为了避免这种情况，我们应该确保使用承诺。

例如，不要写:

```
getData(someParameter, function(err, result){
    if(err !== null && err !== undefined)
    getMoreData(a, function(err, result){
          if(err !== null && err !== undefined)
        getMoreData(b, function(c){ 
                getMoreData(d, function(e){ 
                    //...
                });
            });
        });
    });
});
```

我们有 4 个嵌套的回调函数，即使我们省略了逻辑代码，也很难读懂。

相反，我们写道:

```
doWork()
  .then(doWork)
  .then(doMoreWork)
  .then(doWork)
  .catch(errorHandler)
  .then(verify);
```

我们可以把它变成一个更长的 cleaner，因为 promises 有一个可以传递回调的`then`方法。

如果我们想捕捉错误，我们使用`catch`方法。

我们还可以使用 async 和 await 语法来改变承诺，并使用 try-catch 来捕捉错误。

例如，我们可以写:

```
const work = async () =>
  try {
    const r1 = await doWork();
    const r2 = await doMoreWork();
    const r3 = await doWork();
    const r4 = await verify();
  }
  catch (e) {
    errorHandler(e)
  }
}
```

每个`then`回调都返回一个承诺，所以我们可以对它们使用`await`。

# 仅使用内置的错误对象

内置的错误构造函数应该用于创建错误。

`Error`构造函数有错误消息和堆栈跟踪。

它们对于解决问题很有用，如果我们扔掉其他东西，这些问题就会丢失

例如，不要写:

```
throw 'error';
```

我们写道:

```
throw new Error("error");
```

它会让堆栈跟踪到抛出错误的地方。

# 区分操作错误和程序员错误

我们应该区分操作错误和程序员错误。

操作错误是指错误的影响被完全理解并且可以被处理的错误。

程序员错误是指未知的代码失败，我们需要优雅地重启应用程序。

对于我们可以处理的错误，我们应该能够处理它们，避免重复我们的应用程序。

我们可以抛出错误，通过写:

```
if(!product) {
  throw new Error("no product selected");
}const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('error'));
```

我们可以在同步代码和事件发射器中抛出错误。

我们可以在承诺中抛出一个错误:

```
new Promise((resolve, reject) => {
  DAL.getProduct(productToAdd)
    .then((product) =>{
       if(!product)
         return reject(new Error("no product added"));
       }
    })
});
```

我们承诺用一个`Error`实例调用`reject`来抛出错误。

这些是抛出错误的正确方法。

其他途径包括收听`uncaughtException`事件:

```
process.on('uncaughtException', (error) => {
  if (!error.isOperational)
    process.exit(1);
});
```

监听`uncaughtException`事件会改变事件的行为，因此它不应该被监听。

`process.exit`也是一种不好的结束程序的方式，因为它会突然结束程序。

![](img/84afb06b9b7624d81d47b219cbd27416.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该以正确的方式抛出和捕捉错误。

这样，我们的应用程序将优雅地处理它们。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**寻找一切的链接 plainenglish.io**](https://plainenglish.io/) ！