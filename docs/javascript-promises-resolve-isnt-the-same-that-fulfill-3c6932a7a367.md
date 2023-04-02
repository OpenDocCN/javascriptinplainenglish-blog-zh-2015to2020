# JavaScript 承诺:解决不同于实现

> 原文：<https://javascript.plainenglish.io/javascript-promises-resolve-isnt-the-same-that-fulfill-3c6932a7a367?source=collection_archive---------6----------------------->

## 简短有用的 JavaScript 课程。让它变得简单

![](img/42f04e412a8122b204d59601b6b6a024.png)

关于术语“解决”、“履行”和“拒绝”,有些小混乱。在这篇小文章中，我将尝试用一些例子来解释它们。

让我们看一些例子。

让我们首先创建一个承诺。提供了两个回调:resolveCallback、rejectCallback。

```
var myPromise = new Promise( function(resolveCallback, rejectCallback){
        // resolveCallback() //¡fulfillment or rejection!
        // rejectCallback()  // Only for rejection
 } );
```

第一次回调**通常是**用于将承诺标记为履行(解决或拒绝)，第二次回调**总是**将承诺标记为拒绝。

这些参数的命名在技术上并不重要。名称不会被引擎解释为任何意思，但是您使用的单词不仅会影响您对代码的看法，还会影响团队中其他开发人员对代码的看法。

我们已经使用“解决承诺”来表示履行或拒绝承诺，但是如果这个参数似乎是专门用来履行承诺的，为什么我们称之为履行(..)而不是 resolve(..)更确切的说？为了回答这个问题，我们来看一些例子。

```
var fulfilledPromise = Promise.resolve( “OK” ); 
var rejectedPromise  = Promise.reject( “Error” );
```

Promise.resolve("OK ")创建一个解析为值" OK "的承诺。实现的承诺 fulfilled promise 是为值“OK”创建的。

Promise.reject("Error ")创建拒绝的承诺 rejected promise，原因是" Error "。

现在让我们来说明为什么当在可能导致实现或拒绝的上下文中明确使用时，单词“resolve”比使用例如“fulfill”更明确和更精确:

```
var rejectedThenable = {
   then: function(resolved, rejected) {
        rejected( "Error" );
    }
};
var rejectedPromise = Promise.resolve( rejectedThenable );
```

Promise . resolve(rejected enable)将直接返回收到的承诺或打开收到的表格。如果那个可打开的解包释放出一个拒绝状态，那么从 Promise . resolve(rejected enable)返回的承诺实际上处于相同的拒绝状态。

所以 Promise.resolve(..)对 API 方法来说，这是一个精确而好的名字，因为它实际上可以导致实现或拒绝。

在下一个例子中，我们用一个被拒绝的承诺来解决这个承诺。当我们调用 rejectedPromise 时，我们得到消息错误:“rejected:这是一个有趣的错误”

```
var rejectedPromise = new Promise( function (resolve, reject){
    // resolve this promise with a rejected promise
    resolve( Promise.reject( "this is a funny  error" ) );
} );rejectedPromise.then(
    function fulfilled(){
        // never gets here
    },
    function rejected(err){
        console.log("rejected:"+ err ); 
       //"rejected: this is a funny  error"
    }
);
```

## 如果这对你有帮助，请点击下面的拍手按钮。非常感谢！