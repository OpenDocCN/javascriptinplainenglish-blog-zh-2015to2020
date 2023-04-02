# 关于承诺和异步 JavaScript 的最终指南

> 原文：<https://javascript.plainenglish.io/the-final-guide-on-promises-js-putting-together-the-building-blocks-one-by-one-9ec313b4b9ad?source=collection_archive---------11----------------------->

## 这是一个详细的指南，介绍如何编写带有回调、承诺、异步等待等功能的异步代码

![](img/892aaa7e75a86cea0643762ecffa1ba9.png)

许多承诺的文件缺少一些重要的细节。在这篇文章中，我们将尝试涵盖这些细节，并使逻辑完美无瑕。不要记住单个应用程序。学习幕后的逻辑。在此之后，你将能够完全掌握承诺的概念。

不要因为内容太多而感到不知所措。我花了很长时间才彻底理解这一切。慢慢来，一点一点消化。

所以让我们开始吧。

## 一个问题

假设我们有两个数字 x 和 y，我们想在三秒钟后计算总和(这是一个不切实际的场景，但可以用于教育目的)并打印出来。

你可能想写这样的东西:

```
let x = 5;
let y = 6;
let result;setTimeout( ()=> {result = x+y; console.log(result)}, 3000)
```

我们可以用另一种方法解决。利用承诺。

承诺对于异步编程非常有用。特别是对于承诺链，它解决了“回调地狱”问题(一个接一个地嵌套在前一个完成时按顺序执行的函数(任务)序列)。

我们将从零开始深入了解什么是承诺，我们将通过一些练习来实践。

# 第一部分。承诺的基础

## 定义、语法、分解构件

承诺是一个对象，它由**包装一个任务函数，并在其执行的整个过程中对其进行处理。**

**基本语法:**

```
let examplePromise = new Promise( taskFunction );examplePromise.then(callbackSuccess, callbackFailure);
```

***promise . then*** promise 一般**与“then”**法连用(后面我们还会看到不与“then”连用的情况)。

那么，什么是**“任务功能”、“回调成功”、“回调失败”**？这些都是声明和履行承诺的重要因素。这些都是函数。他们必须遵守一些规范。更详细地一个接一个:

## 任务功能

**taskFunction** (从现在开始我们将一直称之为“任务函数”)是一个如下所示的函数:

```
function taskFunction(resolve,reject) { 
     if ( condition ) {
        resolve(result)
     }  else  {
        reject(new Error("error!"))
     }
}
```

***它接受两个参数作为输入。这两个函数*** 我们都没有定义。我们可以给他们一些名字。通常 ***化解*******拒绝*** (或*成功*和*失败*等)。这些是 Promise 中的内部内置函数，我们只能标记它们。*

***解决**和**拒绝**在履行承诺的**管道**中起主要作用。它们将是在 Promise 的“then”方法中发送的结果的处理程序。现在不用担心，我们将在下一节看到这一点。
现在只知道 **resolve 和 reject 将我们想要转发的结果或错误放在它们的括号中**(这种意义将在后面说明)。*

*如果我们得到了想要的结果，我们通过 resolve 发送它。如果我们得到一个错误，我们通过拒绝发送它。*

*例如:假设我们想获取一些数据。如果任务成功，我们将数据保存在 *result* 变量中，并写入“ **resolve(result)** ”。通过*结果*变量引用的数据将被向前发送。如果取数据失败，我们将发送一个错误“ **reject(新错误(‘取数据失败’)**”。(图片很快就清晰了)。*

***当 taskFunction 遇到 resolve 或 reject 时，它停止执行，**忽略剩余的代码行。*

*补充说明:taskFunction 不需要像上面的代码那样有条件结构。重要的是它向前发送了一些数据，所以你总会在 taskFunction 的声明中找到 **resolve(someResult)** (或者 reject(some)但是你不会在实际应用中只找到这个，只有在教育的例子中，下面我后面会用到一些)。通常你甚至找不到一个 reject，要么是因为任务不能失败(对两个我们已经知道的数字求和，例如:不可能发生错误)，要么是因为即使没有 reject 函数，错误也可以被传递，而只是将它扔进任务函数的主体中(现在不重要:如果你不理解这句话，不要担心，我们稍后会看到)。*

## ***回调成功***

*处理任务函数“callbackSuccess”的成功(或解析/解决)的函数将管理将被传递的结果(请等待下一节以了解整体情况)。因此，在定义它时，它可以有一个参数，该参数将假定从承诺传递的结果的值。*

```
*function callbackSuccess(result) {
   // some operations to be made on "result"
}*
```

***callbackSuccess 的输入参数引用了 resolve 传递下来的内容(或者，换句话说，它包装在括号内的内容)。** 简而言之:假设 taskFunction 以一个 resolve 结束:比如说在 encounters 中，在代码中，“resolve(3)”。该函数结束执行，并向前发送值“3”。在这种情况下，callbackSuccess 会将值 3: result = 3 赋给参数 result。*

## ***回调失败***

*这类似于回调成功。只是输入的参数将引用*拒绝*传递的内容:一个错误。(注意:我们只在出错的情况下使用 callbackFailure。它可以传递其他值，但在实际应用中没有用处)。*

*现在，我们已经分别讨论了每个组件(如果还不明白，不要担心，一些部分将在下一节中链接)，我们可以看到它们是如何一起工作的。*

## *基本用法:执行承诺的管道。*

*我们已经看到了承诺的基本定义和用法:*

```
*let examplePromise = new Promise( taskFunction );examplePromise.then(callbackSuccess, callbackFailure);*
```

*以及如何定义流程中涉及的功能。
现在让我们关注一下执行代码:*

```
*examplePromise.then(callbackSuccess, callbackFailure);*
```

*这段代码将为承诺打开一个执行管道。(这只是我的术语，你可能在别处找不到)。*

*这里有**以下事情依次发生:***

1.  *示例 Promise 将执行正在包装的 taskFunction。*
2.  *taskFunction，在某个时刻执行结束时，当第一次遇到一个关键函数 resolve 或 reject 时发生，或者在 reject 之外抛出一些错误(我们提醒它关闭 taskFunction 的执行)。
    此时，结果通过 resolve(或错误通过 reject)传递。这将作为输入参数发送给 callbackSuccess(分别为 callbackError)。*
3.  *只有在这一点上，callbackSuccess 才会被执行(在 resolve 的情况下。在拒绝回调失败的情况下)，将通过 resolve 传递的值作为输入参数(在拒绝的情况下，输入将是通过 reject 传递的错误)。请注意，在 callbackSuccess 和 callbackFailure 之间只会执行一个。从不打扰。此外，还要注意“回调处理程序”的执行(我们用这个术语表示成功时的 callbackSuccess 或失败时的 callbackFailure)只有在 taskFunction 完成执行后才开始。*

## *示例/练习*

```
*let examplePromise = new Promise( (res,rej) => { 
      setTimeout( ()=> res(3), 3000)
})examplePromise.then( (result) => {console.log(result)} )*
```

*这是一个简化的例子，我们不使用 callbackFailure，因为任务肯定会成功(我们可以省略 callbackFailure 的声明)。*

*请注意，我们立即编写了 taskFunction 作为 Promise 构造函数的参数。我们可以在外面定义。我一般都是发现里面定义的。*

*这将在三秒钟后在控制台中打印出值 3。在前三行中，我们定义了承诺和任务函数。*

*然后在最后一行，我们开始承诺的执行管道。*

*1.首先它被执行`taskfunction`。
2。callbackSuccess 将等待 taskFunction 完成。也就是三秒钟之内。那么 resolve 将向下传递值 3。因此，结果将采用值 3: `result=3`。
3。最后`callbackSuccess`被处决。`callbackSuccess`是功能:`(result) => {console.log(result)}`*

## *解决最初的问题。第一次天真的尝试。*

*现在，看了这个例子，我们准备用 promises 解决上面讨论的初始问题(3 秒钟后对 x 和 y 求和)。*

```
*let x = 5;
let y = 6;let sumAfterAWhile = new Promise( (res, rej) => {
   setTimeout( ()=>{res(x+y)}, 3000 )
})sumAfterAWhile.then((result) => {console.log(result)})*
```

*3 秒钟后，taskFunction 将通过 resolve 函数(在上面的代码中缩写为 *res* )发送 x 和 y 之和 x+y 作为结果。*

*我们可以写得更好:*

```
*sumAfterAWhile.then((result) => {console.log(result)})// We can write it as:sumAfterAWhile.then(console.log)* 
```

*为什么？因为 console.log 已经是一个把一个东西作为输入的函数对象了。*

*备注:如上所述， *then 方法不需要 callbackFailure 函数*。在这种情况下，默认情况下设置为 null。*

*还有一个问题。我们不在任何地方存储结果。随着教程的深入，我们将对代码进行改进，并获得处理这种情况的工具。*

# *第二部分。承诺连锁前的一些重要事实*

## *Promise 的执行管道不会中断主代码*

*假设我们有以下情况:*

```
*let waitAWhile = new Promise((res,rej) => {
   setTimeout( ()=>{res(5)}; 3000)
})waitAWhile.then( (result) => {console.log(result)} )console.log(7)*
```

*代码是逐行读取的。当承诺的执行得到满足时，执行就开始了。但是这是否阻塞了主执行呢？不会。它会打开一个新线程，在那里执行承诺，而主线程会继续逐行读取和执行代码。换句话说，在这种情况下，它将立即打印 7，三秒钟后打印 5。*

*它不会执行以下操作:执行承诺，等待 3 秒钟。打印 5，打印 5 后，打印 7。*

*我们继续说**承诺执行是异步的。
当遇到一些可能需要一些时间的异步操作时，比如 promise 执行(promise 后跟 then 方法)，主线会打开一个新的分支，操作将在那里执行。与此同时，主线不会中断。***

## *承诺状态:已解决和已拒绝(以及待定)。并承诺价值(有人称之为结果)。*

*我们说过承诺可以被解决或拒绝。这取决于承诺正在包装的 taskFunction 的结果。*

*尝试在浏览器的控制台中编写以下内容:*

```
*let examplePromise = new Promise((res,rej) => {
    res(3)
})examplePromise // same as console.log(examplePromise)*
```

*这会给你打印`Promise {<resolved>: 3}`。*

*括号<>内的参数是状态。返回值是 3。*

*以下，*

```
*let examplePromise = new Promise((res,rej) => {
    rej(new Error("issue!!!"))
})examplePromise // same as console.log(examplePromise)*
```

*将产生结果`Promise {<rejected>: Error : issue!!! …}`。因此状态被拒绝。并且该值是错误的。*

*还有待定状态，例如:*

```
*let examplePromise = new Promise((res, rej) => {
    setTimeout(() => { res(3) }, 3000)
})examplePromise*
```

*这将产生一个没有值的挂起状态(或者，确切地说，具有未定义的值)。待定状态只是暂时的。挂起状态意味着正在等待 taskFunction 完成其执行。然后，它将达到最终状态:已解决或已拒绝。*

*它们为什么重要？嗯，尤其是在处理承诺的时候，你会关心它们的状态以及它们在接下来的执行中的价值。
***状态*决定执行哪个回调**(如果已解决，则执行成功回调，如果已拒绝，则执行失败回调，如果未决，则不执行任何操作，直到状态变为已解决或已拒绝)。*

****值/结果*决定将什么作为输入参数传递给 callbackHandler。***

## ***常见错误:我们无法保存带有引用承诺的变量的解析结果。***

*假设我们想保存一段时间后承诺返回的结果。例如，下面的承诺在 3 秒钟后解析为值“hi”。*

```
*let examplePromise = new Promise((res,rej) => {
    setTimeout( ()=> {res("hi")}, 3000) 
})*
```

*如果您想保存值“hi ”,您可能会尝试执行以下操作:*

```
*result = examplePromise*
```

*这样不行。结果将是状态为待定且值未定义的承诺。*

*通过 resolve 函数包装的值将只发送给“then”方法中的 callbackHandlers。这是我们继续处理这些值的唯一方法(async/await 将解决这个问题。见下文):他们在承诺中出生，在执行过程中死亡。承诺的执行分支随着它的局部变量而诞生和消亡。*

*解决这个问题的一种方法是采用以下方法:*

```
*let finalResult;examplePromise.then( (result) => { result = finalResult} )*
```

*在将存储结果的变量外部初始化。然后让 callbackSuccess 函数处理结果:它将附加到 finalResult 之外的变量，结果只存在于 Promise.then 的执行管道中。*

*这并不是很酷，因为我们希望尽可能减少外部定义的变量数量。Async/await 将帮助我们解决这个问题。*

*看到这里，我们可以尝试改善最初的问题。我们可以再做一次尝试*

## *改进初始问题的解决。第二次天真的尝试。*

*留给我们一个改进的问题:我们如何存储 x+y 的和？正如最后一段所示，这不是一个无关紧要的问题。我们将引入一个外部变量来存储结果。Promise.then 的回调成功将处理引用。*

```
*let x = 5;
let y = 6;
let sum;    // Improvement herelet sumAfterAWhile = new Promise( (res, rej) => {
   setTimeout( ()=>{res(x+y)}, 3000 )
})function callbackSuccess(result) {
     sum = result;    // referencing here
     console.log(result)
}sumAfterAWhile.then(callbackSuccess) // Execution here* 
```

*现在让我们暂时搁置这个问题。我们用承诺链来结束承诺的话题。在谈到 async/await 之后(在链接承诺之后)，我们将对这个问题进行最后的改进。对这个问题的改进不会使用任何承诺链，所以如果你不耐烦看到这个问题的结束，你可以跳到 async/await。但是不要避免承诺链，因为这是一个非常重要的话题，如果你想建立网站，你必须知道。*

# *第三部分。承诺链*

*为了结束承诺部分，让我们深入到承诺链中。阅读这篇文章会让你更好地理解**为什么你应该使用承诺而不是回电**。*

## *链接简介*

***动机:**在现实场景中，我们可能想要获取一些数据。这可能需要一些时间。数据取出后，我们想进行其他操作，如提取数据中的某些特定信息，对其进行操作等...*

*这最好在打开的 Promise 管道内完成，因为，正如我们在上面看到的，当执行管道关闭时，我们会丢失管道内的所有内部变量。除非使用一些外部变量。即使在这种情况下，我们也无法在主代码中确切地知道数据何时被提取，那么涉及数据的操作如何在管道之外继续呢？*

***于是，解决方案似乎很自然地是保持承诺执行管道的畅通，并在执行的这条线索中一个接一个地进行操作。**所以我们有了承诺链的概念。作为示例代码的直观概念:*

```
 *Promise.then(handlers1).then(handlers2)*
```

*更容易记住的是 **Promise.then.then** (最终更多 then)。*

***一个简单的例子:**(不是现实场景，仅用于学习目的)。假设我们要等待 3 秒钟，然后对两个数字 x 和 y 求和。再过 3 秒钟，我们要将结果乘以 10 并打印出来。
代码将是(如果您现在不理解，请不要担心。到这一部分结束时，一切都会很清楚):*

```
*let x = 1;
let y = 2;let firstTask = new Promise((res, rej) => {
    setTimeout(() => {res(x+y)}, 3000) //wait 3s and then resolve
})function secondTask(result) {
    return new Promise((res, rej) => {
        setTimeout(() => {res(10*result)}, 3000)// w. 3s & resolve.
    })
}firstTask.then(secondTask).then(console.log)*
```

*这将在 6 秒后打印 30 (= 3*10)。我们连锁了两次行动。在解释完概念后，我们将分析代码。*

*Promises chaining 的主要思想是 Promise(taskFunction)将执行 taskFunction，直到它被解决(或拒绝)。*

*第一个随后的“*然后“*”，使用回调处理程序(即回调成功和回调失败，如果指定的话)将处理结果。*

*随后，第二个随后的“*then”*将接收来自前一个 then 的内容，并使用其 callbackSuccess 函数处理它(最终也是一个 callbackFailure)。诸如此类。**理解下面的*【然后】*如何收到结果**是这一部分的主要目的。*

## *承诺，那就是承诺*

*第一个重要事实。当我们有一个 Promise 对象，并对其应用“then”方法时，不仅 taskFunction 和 callbackHandlers 会被执行。**Promise . then 返回另一个 Promise 对象。如果你不相信，你可以在你的游戏机上测试一下:***

```
*let examplePromise = new Promise((res, rej) => {
      res(2)
})console.log(examplePromise.then((result) => { return result+2 }))
// still a Promise!*
```

*但是，这应该是合理的:t **he then 方法毕竟是一种适用于允诺对象的方法**。因此，如果我们想继续应用 other then，拥有类似于 Promise.then.then 的东西，那么 Promise.then 应该返回一个承诺(或将其视为承诺本身)，这确实是有意义的。*

*从现在开始，我们可以把承诺对象“**Promise . then”**称为“ **then-Promise** ”。这将有助于我们区分主要的原始承诺，我们将称之为**“主要承诺”。所以在上面的例子中，examplePromise 是我们的主承诺，而 examplePromise.then(...)是我们当时的承诺。***

**为了理解链条是如何运作的，把我们得到的理解为* ***那么——承诺(解决了吗？被拒绝了吗？用哪些价值观？)*** *，来自 main-Promise 和 handlers 函数(callbackSuccess，callbackFailure)。**

*当我们弄清楚当时承诺的状态(已解决、已拒绝、待定)和价值时，我们就弄清楚了当时承诺的一切。那么让我们看看如何推导它们。*

*为了弄清楚**那么承诺**是什么，我们需要了解 3 种不同的一般情况。*

## *第一种情况*

**callbackHandler 返回任何非承诺的内容(callback handler 中不会出现任何错误，无论是 callbackSuccess 还是 callbackFailure)。随后,“then-Promise”的状态将为 SOLVED，值为 callbackHandler 返回的值。**

**术语小议*:*

**如果你一直对我使用的****callback handler****这个词感到困惑。对于 callbackHandler，如果主承诺成功，我引用 callbackSuccess，在这种情况下，将执行 callbackSuccess。如果主承诺被拒绝，我称之为 callbackFailure，在这种情况下，将执行 callbackFailure。他们中只有一个会被处决。**

*让我们假设主承诺解决了任务函数。由于成功，回调成功将被执行。callbackSuccess，如果返回任何不是承诺的内容(并且没有引发任何错误),那么 then-Promise 将被解析(状态为 resolved ),其值正好是返回值(如果 callbackSuccess 函数没有返回任何内容)。则它是未定义的值)。*

*即使主承诺被拒绝(状态被拒绝)并且将要被调用的回调失败没有引发/抛出任何错误，这种行为也是正确的。*

*在这种情况下，如果 callbackFailure 没有抛出任何错误/异常，并且状态总是已解决，则该值将是 callback failure 返回的值。*

*示例 1:*

```
*let examplePromise = new Promise((res, rej) => {
      res(2)
})examplePromise.then(console.log) // the then-Promise*
```

*“then-Promise”是一个*已解决的*承诺，值为*未定义的*，因为 callbackSuccess(在本例中是 console.log 函数)不返回任何内容。*

*示例 2:使用回调失败*

```
*let examplePromise = new Promise((res, rej) => {
      rej(2)
})examplePromise.then(console.log, console.log) // the then-Promise*
```

*注意，callbackSuccess 和 callbackFailure 都是 console.log 函数。*

*首先，这种情况下，主-诺会被拒绝！(任务函数代码中的 rej(2))。因此将执行回调失败。但是 console.log 不会引发任何错误。所以…我们在这种情况下。*

*那时的承诺会是什么？*

*then-Promise 是值未定义的已解决的承诺。
已解决，因为无论执行什么(无论是成功还是失败的回调)，如果回调中没有出现错误，callbackHandler 函数的返回值(如果有的话)将确定 then-Promise 的状态。*

*未定义，因为 console.log 函数不返回任何内容。*

**小提示:*对于那些想知道我们为什么设置 reject 一个不是错误的值(2)的人来说，通常不是这样。在实际应用中，你总是拒绝错误。*

## *第二种情况*

**如果 callbackHandler(无论是成功还是失败的回调)在执行过程中抛出一个错误，“then-Promise”将被拒绝，并给出错误的值。**

*示例:*

```
*let examplePromise = new Promise((res, rej) => {
      res(2)
})function successCallback(result) {
   throw new Error('throwing an error!')
}examplePromise.then(successCallback) // the then-Promise*
```

*主承诺的解析值为 2。因此将执行 successCallback(我们甚至没有定义 failureCallback)。我们可以看到 successCallback 函数引发了一个错误(对于那些不知道的人，语法“抛出新错误(..)”会引发错误。这是你需要知道的全部)。注意:我们在 successCallback 中抛出了错误！我们可以随时随地抛出错误。如果我们在 failureCallback 中抛出错误，情况也是如此。*

*在这种情况下，当时的承诺将是一个被拒绝的承诺，并重视提出的错误。(状态:已拒绝；值:抛出的错误)。*

*即使 taskFunction 以拒绝结束，调用 failureCallback，并且 failureCallback 抛出错误，也会出现相同的输出。*

*小提示:稍后会有更多关于错误的细节。它们其实并不重要。现在专注于逻辑，不要迷失在细节中。下面我也会讨论一些次要的事情，比如:吸收错误，捕捉等等…*

## *第三种情况:callbackHandler 返回一个承诺(并且没有产生错误)。在这种情况下，当时的承诺将完全是返回的承诺。*

*这个例子正是我们在本部分开始时展示的例子。*

```
*let x = 1;
let y = 2;let mainPromise = new Promise((res, rej) => {
    setTimeout(() => {res(x+y)}, 3000) //wait 3s and then resolve
})function secondTask(result) {
    return new Promise((res, rej) => {
        setTimeout(() => {res(10*result)}, 3000)// w. 3s & resolve.
    })
}mainPromise.then(secondTask).then(console.log)*
```

*让我们从回答几个问题开始:作为练习，你应该先回答。然后读解。*

***【Q1】**then-Promise，本例中是承诺对象“mainPromise.then(secondTask)”，是解决还是拒绝？
**【Q2】**用哪个值？
**【Q3)**当时的承诺是立即解决/拒绝，还是先待定？*

***答案:***

*当时的承诺是一个承诺。具体来说，我们在第三种情况下:successCallback(也可能是 failureCallback)，在本例中是 secondTask 函数，返回一个承诺。因此，当时的承诺将完全是承诺返回。这将是一个值为 30 的已解决承诺(几秒钟后)。*

*当时的承诺在三秒钟后被实例化。这是因为主承诺仍未解决。只有在值为 3 的情况下，3 秒后才会解决。*

*一旦 then-Promise 被初始化，在 3 秒钟内，该承诺将处于未决状态。在这 3 秒钟后，它将被求解，值为 30。
最后，第二个也是最后一个“然后”(。然后(console.log))，将值 30 作为输入发送给 callbackSuccess 函数，在本例中是 console.log。*

## *回到连锁经营的大背景:*

***问题化简:代替 main-Promise.then，** *我们化简为:* **then-Promise.then，**为*“then-Promise = main-Promise . then”*。我们完全知道如何只处理一个？*

*因此，如果我们有主承诺和 4，然后，我们可以一个接一个地减少它。首先，理解什么是“当时的承诺”。所以一旦想通了，我们就会有一个“然后承诺”和 3 个然后。然后我们可以继续重复同样的逻辑。*

*链接部分到此结束。*

*一旦你有了更多的“然后”，你就知道如何一个接一个地减少，通过一次又一次地弄清楚“然后的承诺”。因此，链接过程变成了一个循环过程，你只需要知道一个过程，这就简化了开始时看到的承诺基础。*

*你应该锻炼。试着想象一些包含要执行的任务过程的练习。试着算出你需要多少，以及如何将问题分解成单个的任务。*

*这些题目并不容易，强烈建议通过练习来掌握它们。*

*如果你愿意，我可以在最后列一份练习清单。*

## *包扎*

*因此，一些承诺链接的管道是根据单个承诺过程进行回收的。管道将在最后一次*然后*的最后一次回调后结束。
***破除承诺链:知道如何计算出当时承诺的地位和价值。****

# *第四部分。异步/等待前的一些观察和评论。(这并不那么重要，但只是为了完整起见。)*

*在此，我们提出一些上面没有讨论过的小观察结果。这些只是一些最后的装饰，如果你没有消化以前的概念，我不建议你去看。*

## *我们可以抛出一个错误，而不是通过拒绝发送错误*

*即本承诺:*

```
*let throwing = new Promise((res, rej)=> {
     throw new Error('issue!')
})*
```

*这个承诺:*

```
*let rejecting = new Promise((res,rej) => {
     rej(new Error('issue!'))
})*
```

*完全一样的承诺。两者都具有相同的被拒绝状态和相同的错误值。*

*这意味着，如果我们用一些处理程序附加“然后”方法，两者的行为将完全相同:回调失败将被执行，相同的错误被发送下去。*

*所以抛出错误或拒绝错误是可以互换的。
有些人只喜欢抛出错误，从不使用拒绝功能。*

## *抛出错误不同于返回错误*

*这适用于承诺链。假设我们有一个承诺，然后，然后，假设第一个承诺，有了回调处理程序，将“返回(新的错误(‘问题！’)) )".当时的承诺不会被视为被拒绝！以下内容不会使“当时的承诺”成为被拒绝的承诺。*

```
*callbackHandler(input) {
     //.... some code...
     //
    return new Error('issue!')
}*
```

*我们必须抛出错误！*

```
*callbackHandler(input) {
     //.... some code...
     // if failed:      throw new Error('type of error...')

     // eventually other code
}*
```

## *链中的捕捉和吸收错误。*

*您可能听说过承诺的捕获方法。它只接受一个函数作为输入:在被拒绝的情况下的回调失败。*

```
*catch(callbackFailure) {...}*
```

**catch(f)* 法等同于 *then(null，f)* 法。*

*通常情况下，常见的做法是:通常只对 callbackSuccess 使用 *then* 方法，省略 callbackFailure。为了管理错误/拒绝，我们使用了 catch。例如，我们可以有一个以下类型的链:*

```
*Promise.then(cS1).then(cS2).catch(cF1).then(cS3).catch(cF2)* 
```

*其中 cS 代表回呼成功，cF 代表回呼失败。*

**当错误通过回调失败被“捕获”时，它从承诺链接的执行管道中消失。*此外，如果发生错误，它会被向下发送，直到满足捕获条件(如果有)并处理该错误。*

*在上面的示例中，如果 cS1 中发生错误，则。那么(cS2)“不会处理这个问题，会将其发送给”。catch(cF1)"，即吸收错误。*

***总是在链的末端放一个钩是一个好的习惯。***

```
*Promise.then(cS1).then(cS2)*
```

*这条链子不会处理任何错误。如果承诺中发生错误，随后该错误将被发送至“当时承诺”和“当时承诺”，因为它从未被处理过。*

# *第五部分。异步/等待*

*以前，我们预计在执行承诺链的管道中处理的数据在那里只是生与死，不能直接引用它。*

*Async/Await 解决了这个问题。*

## *句法*

*Async 放在函数声明之前:*

```
*async function saveResult(input) { let waitPromise = new Promise((res, rej) => {
         setTimeout(() => resolve(input), 5000)
    }); let result = await waitPromise;    console.log(result);
}saveResult('hi') // Will print hi after 5 seconds.*
```

*Await 用在带有异步声明的函数中，放在承诺之前。*

*Await 允许停止函数的执行，直到承诺被解决或拒绝。这意味着 await 下面的任何其他行(我指的是 async 函数中的内部代码)都不会被读取，直到 Promise 被求解，此时 resolution 的值将被返回(这不会停止函数的执行)并存储在 result 变量中。*

*在上面的例子中，变量*结果*将是解析的值，只有在承诺已经解析之后。这样一来:*

***我们正在捕捉“承诺”管道的最终数据。** 终于注意到我们不需要那么一个许诺的方法。我们可以有一连串的承诺(正如我们所看到的，这是一个“然后-然后-…-承诺”)。在这种情况下，await 返回这个长管道的最终值。*

*最后，**用 async 声明的函数相对于外部代码是异步的。***

```
*saveResult('hi')console.log(2)*
```

*第一行正在执行并打开一个新线程(上面定义的异步函数)。主线程不会停止:紧接其后的一行被读取。*

*所以值 2 会立即打印出来。5 秒钟后，输出值“hi ”,因为异步函数“saveResult”的执行发生在另一个执行分支中。*

## ***最后让我们改进我们最初的问题***

*现在我们已经看到了 async/await，代码可以改进:*

```
*let x = 5;
let y = 6;let sumAfterAWhile = new Promise( (res, rej) => {
   setTimeout( ()=>{res(x+y)}, 3000 )
})async function success() {
     let sum = await sumAfterAWhile;  
     console.log(sum);
     return sum
}success() // prints and returns the sum after 3 seconds.*
```

***其他改进:***

*最后，我们可能希望将 x 和 y 作为自由参数。我们可以将 x 和 y 作为异步函数的参数，并嵌套承诺:*

```
*async function success(x,y) { let sumAfterAWhile = new Promise( (res, rej) => {
         setTimeout( ()=>{res(x+y)}, 3000 )
     })     let sum = await sumAfterAWhile;       console.log(sum);
     return sum
}*
```

***最后一点提示:**如果不使用 async/await，我们可以使用常见的技术— **在函数中包装承诺。**我们可以用“special function . then”代替“Promise.then”，其中 special function 是一个返回承诺的函数。因此，如果我们想将 x 和 y 设置为特定函数的输入参数，我们会得到这样的结果:*

```
*function particolarFunction(x,y) {
     return new Promise((res,rej) => {
           setTimeout( ()=>{res(x+y)}; 3000) 
     })
}particularFunction(5,6).then(console.log) // IF we named the particularFunction as sumAfterAWhile, 
// The code becomes immediately explicit and this is desirable.*
```

*尽管如此，仍然存在从管道中保存临时数据的问题。这就是为什么 async/await 是首选。*

# *结论*

*如果你已经通读了这篇文章(并有望理解)，那么恭喜你！我希望这有所帮助。这是艰难而漫长的。但现在你应该觉得自己是承诺大师了！*

*下次再见，祝你愉快！*

**PS。这篇长文可能有一些错误。如有任何更正，不胜感激。**