# 6 个包含承诺和暂停的面试问题

> 原文：<https://javascript.plainenglish.io/6-interview-questions-that-combine-promise-and-settimeout-34c430fc297e?source=collection_archive---------0----------------------->

## 彻底掌握这种类型的面试问题。

![](img/f0b925fb5746716fb4d8006da90c074c.png)

Photo by [Bharat Patil](https://unsplash.com/@bharat_patil_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在我们开始之前，我希望你能弄清楚几个知识点。

事件循环按以下顺序执行:

*   JS 引擎中有两个任务队列:宏任务队列和微任务队列
*   整个脚本最初作为宏任务执行
*   执行过程中，同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
*   当当前宏任务完成时，检查微任务队列，并依次执行所有的微任务
*   执行浏览器 UI 线程的呈现(在本文中可以忽略这一点)
*   如果存在任何 Web Worker 任务，就执行它(在本文中可以忽略这一点)
*   检查宏任务队列，如果它不为空，返回到步骤 2 并执行下一个宏任务。

值得注意的是，第四步:当一个宏任务完成后，先依次执行其他所有的微任务，然后执行下一个宏任务。

微任务包括:`MutationObserver`、`Promise.then()`和`Promise.catch()`，其他基于承诺的技术如 fetch API、V8 垃圾收集过程、节点环境中的`process.nextTick()`。

宏任务包括:初始脚本、`setTimeout`、`setInterval`、`setImmediate`、I/O、UI 渲染。

好吧，如果你没有完全理解这是怎么回事也没关系，我们用例子来练习一下。

共有 10 个问题:前 4 个是简单的承诺问题，帮助你理解微任务；接下来的 6 个问题混合了承诺和暂停。

# 1.

让我们从一个简单的例子开始解释微任务。

## 示例:

```
const promise1 = new Promise((resolve, reject) => {
  console.log(1);
  resolve('success')
});promise1.then(() => {
  console.log(3);
});console.log(4);
```

## 流程分析:

*   首先，执行这段代码的前四行。控制台会打印出`1`，然后`promise1`会变成`resolved`状态。
*   然后开始执行`promise1.then(() => {console.log(3);});` 片段。因为`promise1`现在处于已解决状态，所以`() => {console.log(3);}`将被立即添加到微任务队列中。
*   但是我们知道`() => {console.log(3);}`是一个微任务，所以没有立即调用。
*   然后执行最后一行代码(`console.log(4);`)，并在控制台中打印`4`。
*   此时，所有同步的代码，即当前的宏任务，都被执行。然后 JavaScript 引擎检查微任务队列并依次执行它们。
*   然后执行`() => {console.log(3);}`,并在控制台中打印`4`。

## 结果:

![](img/90fe9cc279e2c469f3c6fa6f15ecdd98.png)

# 2.

## 示例:

```
const promise1 = new Promise((resolve, reject) => {
  console.log(1);
});promise1.then(() => {
  console.log(3);
});console.log(4);
```

## 流程分析:

这个例子和上一个非常相似，除了在这个例子中，`promise1`将一直处于`pending`状态，所以`() => {console.log(3);}`不会被执行，控制台也不会打印 3。

## 结果:

![](img/14b3d77f7ff9bb8a48cfbc13d5627a7a.png)

# 3.

## 示例:

```
const promise1 = new Promise((resolve, reject) => {
  console.log(1)
  resolve('resolve1')
})const promise2 = promise1.then(res => {
  console.log(res)
})console.log('promise1:', promise1);
console.log('promise2:', promise2);
```

仔细考虑控制台打印结果的顺序和每个承诺的状态。

## 流程分析:

*   首先，前四行代码和前面一样，`1`打印在控制台中，`promise1`的状态是`resolved`。
*   然后执行`const promise2 = promise1.then(...)`，将`res => {console.log(res)}`添加到微任务队列中。同时，`promise1.then()`会返回一个新的`pending`承诺对象。
*   然后执行`console.log(‘promise1:’, promise1);`，控制台打印出解析状态下的字符串`'promise1'`和`promise1`。
*   然后执行`console.log(‘promise2:’, promise2);`，控制台打印出字符串`‘promise2’`和挂起状态的`promise2`。
*   此时，所有同步的代码，即当前的宏任务，都被执行。然后 JavaScript 引擎检查微任务队列并依次执行它们。
*   `res => {console.log(res)}`是微任务队列中唯一的任务，现在将执行它。然后控制台会打印`'reslove1'`。

## 结果:

![](img/1c5706425ce80a3d6793bd9b3cc6feb5.png)

# 4.

## 示例:

```
const fn = () => (new Promise((resolve, reject) => {
  console.log(1)
  resolve('success')
}));fn().then(res => {
  console.log(res)
});console.log(2)
```

## 流程分析:

与之前不同，在这个例子中，创建 Promise 对象的行为发生在`fn`函数中。虽然`fn`函数是一个普通的同步函数，但它并没有什么特别之处，这个例子仍然很简单。

## 结果:

![](img/a0ff6ab15b9f28f053a5ed0547891ea4.png)

*前面的例子比较简单，现在问题会逐渐变得复杂，你准备好了吗？*

# 5.

## 示例:

```
console.log('start')setTimeout(() => {
  console.log('setTimeout')
})Promise.resolve().then(() => {
  console.log('resolve')
})console.log('end')
```

## 流程分析:

*   首先，JS 引擎中有两个任务队列:宏任务队列和微任务队列。

![](img/5fd65ceb6f7059748c9df33f2b122b2f.png)

*   在程序开始时，所有的初始代码都被当作一个宏任务，推入宏任务队列。

![](img/006c119f44eddfb07484eb79454842a1.png)

*   然后执行第一行代码`console.log('start')`，并在控制台中打印`‘start’`。

![](img/7874a8786f5b7cc99727b545afa906e6.png)

*   那么`setTimeout(...)`就是一个等待时间为 0 的定时器，会立即执行。正如我们在本文开头提到的，`setTimeout`是一个宏任务，所以`setTimeout(...)`、`() => {console.log(‘setTimeout’)}`的回调函数不会立即执行，它会被压入宏任务队列，等待以后执行。

![](img/4f4dd2efcaa4c2d1227b4d2355cd2301.png)

*   然后它开始执行`Promise.resolve().then(…)`，并且`() => {console.log('resolve')}`被推入微任务队列。

![](img/7e480a59b5e3c7dd3454ddfa5598db5a.png)

*   现在执行`console.log(‘end’)`，在控制台打印`‘end’`，第一个宏任务完成。

![](img/af483fe6adbb300353a37c73faaba089.png)

*   当一个宏任务完成时，JS 引擎首先检查微任务的队列，然后依次执行所有的微任务。

![](img/11e824d60e8e79de3e1a6bdc061e0e2e.png)

*   当微任务队列为空时，JS 引擎检查宏任务队列，并开始执行下一个宏任务。

![](img/f48e59fa9d37f890bc61c0eeffaf0668.png)

值得强调的是，虽然`setTimeout(...)`在`Promise.resolve().then(...)`之前执行，但是`setTimeout(...)`的回调函数仍然在后面执行，因为`setTimeout`是一个宏任务。这是新手犯错误最多的地方。

好了，以上示例代码就是这样运行的。希望我的素描能帮到你。

## 结果:

![](img/6fb7b84bf766f532f75beb7531669ef6.png)

# 6.

## 示例:

```
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    console.log("timerStart");
    resolve("success");
    console.log("timerEnd");
  }, 0);
  console.log(2);
});promise.then((res) => {
  console.log(res);
});console.log(4);
```

## 流程分析:

*   首先，我们暂时忽略那些回调函数，简化代码:

```
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(..., 0);
  console.log(2);
});promise.then(...);console.log(4);
```

*   然后我们像以前一样画图。起初，所有的代码都可以被认为是一个宏任务。

![](img/7f66c288fb2c0d6f8cbede528444843d.png)

*   然后开始执行`new Promise(...)`，然后进入执行器内部执行`console.log(1)`。

![](img/167ab76325986751da9ce0c92d0878a5.png)

*   然后开始执行`setTimeout(..., 0)`。计时器立即结束，其回调函数被推入宏任务队列。

![](img/9e169e9dadda7e9fbfb420dc50d2acc5.png)

*   然后开始执行`console.log(2)`。

![](img/ce9a1daaefaaa10eeca5f997df524088.png)

*   现在开始执行`promise.then(…)`。因为 promise 对象仍然处于未决状态，所以它的回调函数还没有被压入微任务队列。也就是说，微任务队列当前仍然是空的。

![](img/143d6daadb7f29144dd459ce5cbed82d.png)

*   然后开始执行`console.log(4)`。

![](img/d047873992430de84f517899d3ef0d1c.png)

*   此时，第一个宏任务结束，微任务队列仍然为空，因此 JS 引擎启动下一个宏任务。
*   然后开始执行`console.log(‘timerStart')`。

![](img/b22367a4ef4cb210c30ab35e881f82fd.png)

*   现在`resolve()`函数被执行，承诺的状态将是`resolved`并且`promise.then(…)`的回调函数被推入微任务队列。

![](img/d15f761b9ec5efacf0546502b3effee9.png)

*   然后开始执行`console.log(‘timerEnd')`。

![](img/ba898ef79b06607b7a704e350f6afcab.png)

*   现在当前的宏任务已经结束，JS 引擎再次检查微任务队列，并依次执行它们。

![](img/f4c04025ddf25a5d290bd210a96edb5d.png)

## 结果:

![](img/28d30c4dc52fbdaea184c18b7233a8ea.png)

# 7.

## 示例:

```
const timer1 = setTimeout(() => {
  console.log('timer1');const timer3 = setTimeout(() => { 
    console.log('timer3')
  }, 0)
}, 0)const timer2 = setTimeout(() => {
  console.log('timer2')
}, 0)console.log('start')
```

## 流程分析:

在这个例子中有三个`setTimeout`函数，所以程序累积了三个额外的宏任务。

*   首先，让我们画出初始的宏任务队列。

![](img/74fc23952c90f44bad6250c07f928d41.png)

*   然后开始执行`timer1`对应的`setTimeout(...)`。同时，创建一个新的宏任务。

![](img/7ec22ae6273d0fa91d620477c192d9bd.png)

*   然后开始执行`timer2`对应的`setTimeout`。同时，创建了另一个新的宏任务。

![](img/9fd7fed889ffdcf1ad1baa4d3b92cbf1.png)

*   好，现在我们有三个宏观任务，没有微观任务。
*   然后

![](img/f63daae0a9b43b34bc7ea35833d76d9e.png)

*   既然第一个宏任务及其执行已经完成，并且微任务队列仍然为空，那么 JS 引擎将开始执行下一个宏任务。
*   `console.log('timer1')`被执行。

![](img/9b5084fc0db788c02c9594c3339bafe6.png)

*   然后开始执行`timer3`对应的`setTimeout(...)`。将创建一个新的宏任务。

![](img/0b2710f64072b0cf6355a5c92d7660a1.png)

*   然后

![](img/6aa2434f135f09f99241b8c22a06529a.png)

*   然后

![](img/dc62c30dbbdf6534619f70c38b09fee8.png)

## 结果:

![](img/a8a8528de9bc575cb89ffff443a85e26.png)

# 8.

## 示例:

```
const timer1 = setTimeout(() => {
  console.log('timer1');
  const promise1 = Promise.resolve().then(() => {
    console.log('promise1')
  })
}, 0)const timer2 = setTimeout(() => {
  console.log('timer2')
}, 0)console.log('start')
```

## 流程分析:

这个例子与上一个类似，除了我们用一个`Promise.then`替换了一个`setTimeout`。因为`setTimeout`是一个宏任务，而`Promise.then`是一个微任务，并且微任务优先于宏任务，所以控制台输出的顺序是不一样的。

*   首先，让我们画出初始任务队列。

![](img/dd1b89c01f07bff8c94ba771e6d961f9.png)

*   然后

![](img/db799f330a875f39fc0c137f24d489ff.png)

*   然后

![](img/7c345c596e668a61190f9b8a7af0054f.png)

*   然后

![](img/7743e4b5649ef1d3a74b94a1315c4585.png)

*   然后

![](img/f202371e8dbd092e08902cec8fb21c28.png)

*   然后

![](img/d7e211189e9c939354ae1c95eb1e8a32.png)

*   注意此时`Promise.then()`正在创建一个微任务。它的回调函数由 JS 引擎在下一个宏任务之前执行。

![](img/61ec388a3da9620c749bd582c2bfed9f.png)

*   然后结束。

![](img/23bbca70a738740d5cbc3c2e41456428.png)

## 结果:

![](img/8c4c64ebae8aa2485f68049931306950.png)

# 9.

## `Example:`

```
const promise1 = Promise.resolve().then(() => {
  console.log('promise1');
  const timer2 = setTimeout(() => {
    console.log('timer2')
  }, 0)
});const timer1 = setTimeout(() => {
  console.log('timer1')
  const promise2 = Promise.resolve().then(() => {
    console.log('promise2')
  })
}, 0)console.log('start');
```

## 流程分析:

在这个例子中，宏任务和微任务是交替创建的，这是一个困难的话题。如果你只是在头脑中思考，那就很容易出错。但是如果你开始和我一起画图表，就很容易找到正确的答案。

*   首先，让我们画出初始的宏任务队列。

![](img/48d6ad7def9334a1564d39cc47c88405.png)

*   然后执行第一段代码，并创建一个微任务。

![](img/0b9581d0209cadb13d94a1307a8e0300.png)

*   然后执行第二段代码，并创建一个宏任务

![](img/67dcccfd7e7ff5448fc6c12bd5d93cbf.png)

*   然后

![](img/c533d27fb15ebad54e9cb1f17fe0a191.png)

*   当前宏任务完成，微任务队列中的任务开始。

![](img/4a6f31c3d348b4c202756da1a18fb013.png)

*   然后，开始执行与定时器 2 相关的`setTimeout(...)`并创建一个新的宏任务

![](img/b4573d96e49994397868e8f1abb9c621.png)

*   清除当前微任务队列，并开始下一个宏任务。

![](img/ba98f3f86e74993d47fc4e42916a476f.png)

*   然后，创建另一个微任务。

![](img/c80696a8b9c7bbf776a67588b2382302.png)

*   当前宏任务已完成。JS 引擎再次检查微任务队列，发现队列不为空，并开始区分微任务队列中任务的优先级。

![](img/ffd92f827979759ac14d3b14dd95f907.png)

*   最后

![](img/7952641e1c8c559711d1cd5c6e20a292.png)

## 结果:

![](img/8b14bf3dd8f8586005c9d506e3c5d694.png)

# 10.

## `Example:`

```
const promise1 = new Promise((resolve, reject) => {
  const timer1 = setTimeout(() => {
    resolve('success')
  }, 1000)
})const promise2 = promise1.then(() => {
  throw new Error('error!!!')
})console.log('promise1', promise1)
console.log('promise2', promise2)const timer2 = setTimeout(() => {
  console.log('promise1', promise1);
  console.log('promise2', promise2);
}, 2000)
```

## 流程分析:

*   首先，它通过处于待定状态的`new Promise(…)`创建了`promise1`。还创建了一个延时 1 秒的定时器。
*   然后执行`const promise2 = promise1.then(...)`，因为`promise1`当前处于`Pending`状态，所以`promise1.then()`的回调函数还不会加入到微任务队列中。
*   然后执行`console.log(‘promise1’, promise1)`。此时，`promise1`还是`Pending`。
*   然后执行`console.log(‘promise2’, promise2)`。此时，`promise2`仍然是`Pending`。
*   然后执行`const timer2 = setTimeout(…)`。还创建了一个延时 2 秒的定时器。
*   1000 毫秒后，`timer1`完成。然后执行`resolve(‘success’)`，并且`promise1`变成`resolved`。
*   调用了`promise1.then(...)`的回调函数，执行了`throw new Error(‘error!!!’)`。一个错误被抛出，`promise2`变成了`reject`。
*   又过了 1000 毫秒，`timer2`完成了。`() => {console.log(‘promise1’, promise1); console.log(‘promise2’, promise2);}`被执行。

## 结果:

![](img/a9eb3947cd22c9213b4ae9193315d285.png)