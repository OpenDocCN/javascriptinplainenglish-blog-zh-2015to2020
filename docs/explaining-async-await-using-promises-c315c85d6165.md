# 使用承诺解释异步/等待

> 原文：<https://javascript.plainenglish.io/explaining-async-await-using-promises-c315c85d6165?source=collection_archive---------22----------------------->

![](img/a9bc4bc8a0ca7def3308a7937e780881.png)

[JavaScript](https://commons.wikimedia.org/wiki/File:JavaScript-logo.png)

# 承诺

## 解决

用值调用`[Promise.resolve](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)`:

与在回调函数中用一个*实现的*值实例化一个`[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)`是一样的:

## 拒绝

打电话给`[Promise.reject](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)`是有原因的:

等同于在回调函数中实例化一个带有*拒绝*原因的`Promise`:

# 异步/等待

## 异步功能

一个[异步函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)被声明，函数前面有`async`:

这类似于异步[箭头功能](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions):

当调用一个异步函数时，它返回一个`Promise`:

# 比较

## 感到满足的

Async/await 是使承诺看起来同步的语法糖(但实际上不是):

这相当于:

对于实现的承诺，您可以将`await`视为承诺链中的`[then()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)`方法。

## 拒绝

当一个承诺在异步函数中被拒绝时，你可以用一个 [try…catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) 来处理它:

这与以下情况相同:

对于一个被拒绝的承诺，可以把`catch`块想象成承诺链中的`[catch()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)`方法。

[【remarkablemark.org】本文原载于 2020 年 8 月 19 日。](https://b.remarkabl.org/3lwbhGI)