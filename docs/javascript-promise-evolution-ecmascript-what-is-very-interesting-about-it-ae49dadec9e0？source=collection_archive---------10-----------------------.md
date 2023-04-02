# 你可以用 JavaScript 做一些新的事情

> 原文：<https://javascript.plainenglish.io/javascript-promise-evolution-ecmascript-what-is-very-interesting-about-it-ae49dadec9e0?source=collection_archive---------10----------------------->

![](img/5e407ce4f49e6c4e5459125d565f1e41.png)

Do you Promise with JavaScript?

promise 用于处理 JavaScript 中的**异步**操作。

这个承诺是一个克服 [**回调地狱**](https://www.geeksforgeeks.org/what-is-callback-hell-in-node-js/) 问题的想法，与回调和事件相比**有更好的** **错误处理**。

A `Promise`处于以下状态之一:

*   *待定*:初始状态，未履行也未拒绝。
*   *完成*:表示操作成功完成。
*   *拒绝*:表示操作失败。

## **1。基础知识**

```
Promise.resolve(value);
```

`**Promise.resolve()**`方法返回一个用给定值解析的`[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)`对象。

```
Promise.reject(value);
```

`**Promise.reject()**`方法返回一个因给定原因被拒绝的`Promise`对象。

## 2.Promise.all()

如果每一个承诺值(即:承诺 1，2，3)都**依赖于彼此的承诺值**，那么使用这个会很有帮助 **Promise.all()。**

```
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);
const promise3 = Promise.resolve(3);Promise.all([promise1, promise2, promise3])
.then((values) => {
      console.log(“All promise resolved”,values);})
.catch((error) =>{
      console.log(“Any one Promise Rejected”,error)
})
```

如果所有的承诺都被解析，那么所有被解析的**值**作为**数组**返回。

```
**Output:** "All promise resolved" Array [1, 2, 3]
```

如果任何一个承诺被拒绝，则返回被拒绝的**值**。

```
const promise1 = Promise.resolve(1);
const promise2 = Promise.reject(2);
const promise3 = Promise.resolve(3);Promise.all([promise1, promise2, promise3])
.then((values) => {
      console.log(“All promise resolved”,values);})
.catch((error) =>{
      console.log(“Promise Rejected”,error)
})**Output:** "Promise Rejected" 2
```

## **3** 。 **Promise.allSettled()**

当你有**多个异步任务**并且**不依赖于另一个**来成功完成时，或者你总是想知道每个承诺的结果时，通常会用到它。

```
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);
const promise3 = Promise.reject(3)
const promise4 = Promise.resolve(4);
const promises = [promise1, promise2,promise3,promise4];Promise.allSettled(promises).then((results) => console.log(results))**Output:** [
{ status: "fulfilled", value: 1 }, 
{ status: "fulfilled", value: 2 }, 
{ status: "rejected", reason: 3 }, 
{ status: "fulfilled", value: 4 }]
```

## **4** 。 **Promise.any()**

返回一个**单个承诺**，该单个承诺使用该承诺的值进行解析。你可以把这当成 **Promise.all()** 的反义词

```
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);const promises = [promise1, promise2];Promise.any(promises).then((value) => console.log(value))
 .catch(error=>{
 console.log(error);
})**Output:** 1
```

`[AggregateError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)`,`[Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)`的一个新子类，将单个错误组合在一起。

```
const promise1 = Promise.reject(1);
const promise2 = Promise.reject(2);const promises = [promise1, promise2];Promise.any(promises).then((value) => console.log(value))
 .catch(error=>{
 console.log(error);
})**Output:** AggregateError: All promises were rejected
```

## 承诺，承诺，竞赛

`**Promise.race()**`方法返回一个承诺，只要 iterable 中的一个承诺满足或拒绝，该承诺就满足或拒绝，并带有该承诺的值或原因。

```
const promise1 = new Promise((resolve, reject) => {
 setTimeout(reject, 500, ‘1’);
});const promise2 = new Promise((resolve, reject) => {
 setTimeout(reject, 100, ‘2’);
});Promise.race([promise1, promise2]).then((value) => {
 console.log(value);
 // Both resolve, but promise2 is faster
}).catch(error=>{
 console.log(error);
})**Output:** "2"
```