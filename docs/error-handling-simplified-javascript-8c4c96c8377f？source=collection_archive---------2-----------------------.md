# 如何写出更好更安全的 JavaScript 代码

> 原文：<https://javascript.plainenglish.io/error-handling-simplified-javascript-8c4c96c8377f?source=collection_archive---------2----------------------->

![](img/5b3889529c13f6e1428dc109f5f1e06e.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种动态语言。这使得 JavaScript 一开始就是一种非常酷的语言。然而，很难写出更好更安全的代码。一个小错误会导致更大的问题。错误处理在减少错误数量方面起着至关重要的作用。如果你以优雅的方式处理错误，将来会节省很多时间。所以更大的问题是你应该如何处理这个错误。

让我们举一个例子。

```
const express = require("express")
const app = express()
app.get("/", (_, res) => res.end("OK"))app.listen(80)
```

上面的代码是在一个 [express](https://expressjs.com/) 框架上用 nodejs 编写的示例代码。在这段代码中，我试图在端口 80 运行一个服务器。

**问题:**

1.  如果我们知道端口 80 已经被其他应用程序占用，并且我们试图运行上面的代码，那该怎么办？
2.  我们怎么知道会发生什么？
3.  它会运行还是会出错而中断？

看到这段代码时，输出非常不清楚。即使您想处理一个错误，您可能也必须阅读文档。但是不用担心，就像任何自然语言一样。编程语言有一些语法。

*ECMA，没有特别提到的规格/标准。然而，JavaScript 社区遵循一定的编码准则。*

## **1。错误类型**

因为 JavaScript 具有不同的编译器风格，并且大多数由不同的组织编写和维护。除了 [SyntaxError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError) 之外，错误类型之间没有非常明确或一致的分布。甚至消息也因编译器而异。然而，你可以在这里找到语法错误[的列表。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)

**注意:** JavaScript 是一种动态语言，大部分错误都是运行时错误。

**例如:**

```
console.log(Number(10).toPrecision(200))
```

如果我们运行上面的代码，它将抛出 **RangeError** 。

`RangeError: toPrecision() argument must be between 1 and 100`

## 2.如何处理错误

基于 API(方法)调用同步/异步的性质，错误可以被不同地处理。

## 2.1 **同步**

**试捕:**你可以用一个[试捕](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)块来处理**同步**错误。

```
try {
  console.log(Number(10).toPrecision(200));
} catch (error) {
  **// RangeError: toPrecision() argument must be between 1 and 100**
  console.log(error **instanceof** RangeError); // **true**
}
```

*如果不想捕捉错误并执行任何操作。在较新版本的 JavaScript 编译器中，您可以这样做。*

```
try {
  console.log(Number(10).toPrecision(200));
} catch {}
```

如果想对 error 执行一些默认操作，可以在 catch 块后使用 **finally** **块**。

```
let average;
try {
  average = getAverage(); // Sum function does not exits.
} catch {
} **finally {
  average = 0;
}**
console.log(`Average is ${average}`);
// Average is 0
```

最后块主要用来清理资源像一些**打开文件**，**打开** **连接**。

![](img/b10d323f60d4585f54e928cc54744e1d.png)

Photo by [NordWood Themes](https://unsplash.com/@nordwood?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **2.2 异步**

当结果出现在 EventLoop 的下一个事件周期时，API 在本质上被称为异步的。通常，所有网络调用和 IO 操作本质上都是异步的。

为了收集异步调用的结果，我们或者使用一个**回调**或者**承诺**对象。

**异步回调 API(callback-error-data)中错误处理:**基于核心浏览器的 JavaScript 具有非常有限的异步 API。你可以使用定时器 API 创建一个异步函数，比如 **setTimeout** 和 **setInterval。**您还可以使用 AJAX 调用请求服务器(使用 **fetch)** 。 **setTimeout** 和 **setInterval** 不抛出任何可以处理的错误。并且 **fetch** 是一个基于承诺的异步调用*(我们将在后面学习如何处理基于承诺的错误)*。然而，nodejs 有很多标准和第三方 API，这会引发一个错误。就像我们的第一个 **express.js** 例子一样。

```
const express = require("express");
const app = express();
app.get("/", (_, res) => res.end("OK"));const server = app.listen(80);
server.on("error", function handleListen(error) {
  console.log(error);
});
```

在上面的例子中，Express 没有试图为您处理错误。相反，它返回 nodejs 的核心[服务器](https://nodejs.org/api/http.html#http_event_clienterror)实例。您可以在错误**处理程序回调函数**上捕获错误。

Nodejs 遵循一定的规则。作为[编码标准](https://nodejs.org/api/errors.html)，所有的异步 API 都接受回调。在回调中，第一个参数是 API 生成的错误，第二个参数是成功的数据。这个标准也被整个社区所遵循。

看到这种模式，我们可以如下处理错误

```
fs.readFile('a file that does not exist', (err, data) => {
  if (err) {
    console.error('There was an error reading the file!', err);
    return;
  }
  // Otherwise handle the data
});
```

**注意:**你不能在 **try-catch** 块中处理异步回调错误。然而，有一个例外。ECMA 脚本的最新版本，使用异步等待现在我们可以处理 try-catch 中的错误。我们稍后会了解这一点。

```
// This will not worktry {
  app.listen(80);
} catch (error) {
  // never called
  console.error(error)
}
```

**基于承诺 API 中的错误处理(promise-then-catch):** 在 ES5 之后，JavaScript 引入了一种新的设计模式来处理异步 API 的回调。那是一个[承诺的设计图案](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)。这就解决了前一期的 [**回调地狱**](https://www.bmc.com/blogs/callback-hell/) 。

```
const promise = new Promise((response, reject) => {
  // some async code here
});promise
  .then(function onSuccess(data) {
    console.log("SUCCESS");
  })
  .catch(function onError(err) {
    console.error(err);
  });
```

为了捕捉来自 promise 对象的错误，您必须使用 **catch** 方法并传递一个回调函数。要了解[承诺/延期模式](https://www.tothenew.com/blog/angularjs-deferred-promises-basic-understanding/)，可以在这里阅读我的博客[。](https://www.tothenew.com/blog/angularjs-deferred-promises-basic-understanding/)

**在使用 async-await(try-catch-await)基于承诺的 API 中的错误处理:**承诺比回调干净得多。然而，理解一个大代码库中的流程是非常困难的。ECMA 脚本的最新版本引入了[异步等待](https://javascript.info/async-await)。使用 async-await，我们可以同步编写异步代码。

```
async function main() {
  try {
    await promise1;
    const data = await promise2;
  } catch (error) {
    console.log(error); // SOME ERROR
  }
}
main();
```

使用 **try-catch-await** ，您可以在一个块中处理多个错误，这在 **promise-then-catch** 模式中是不可能/复杂的。

现在我们知道如何处理错误了。然而，在编写代码时，我们不必只处理错误。我们可能想要创建一个自定义错误。这将有助于编写干净和可维护的代码。这是一个很好的实践，您应该为业务逻辑创建自定义错误

![](img/873d10e7c9ca73fdebfe6fb38a45a63d.png)

Photo by [Isis França](https://unsplash.com/@isisfra?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 3.创建自定义错误

创建自定义错误非常简单。您可以使用任何自定义类，并将其作为错误抛出。

```
class SomeNetworkError {
  constructor(status) {
    this.status = status;
  }
}
try {
  throw new SomeNetworkError(4000);
} catch (error) { console.log(error instanceof SomeNetworkError); // true console.log(`SomeNetworkError Status: ${error.status}`); // SomeNetworkError Status: 4000}
```

上面我们有一个类，我们使用这个类的一个实例来抛出一个错误。这是一个有效的代码。然而，作为编码实践，我们应该扩展默认(标准)错误类。所有错误类的基础是[错误](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error#Constructor)并使用消息调用一个超级方法。

```
class SomeNetworkError extends Error {
  constructor(message, status) {
    super(message);
    this.status = status;
  }
}
try {
  throw new SomeNetworkError("Network Error", 4000);
} catch (error) { console.error(error instanceof Error); // true
  console.log(`> ${error}`); // > Error: Network Error
  console.error(error); // SomeNetworkError: Network Error
  console.error(error.stack); *// stacktrace here*}
```

如果您注意到，扩展 Error 类并调用 super 会自动获得。SomeNetworkError 类的 toString 方法。它打印了一个很好的信息。类似地，您也可以扩展其他标准错误类。

```
class ArithmeticRangeError extends RangeError {
  constructor(message) {
    super(message);
  }
}
try {
  const zero = 0;
  if (zero === 0) {
    throw new ArithmeticRangeError("zero cant be 0");
  }
} catch (error) {
  console.log(error instanceof RangeError); // true
  console.error(error.toString()); // RangeError: zero cant be 0
}
```

## 4.高级错误处理

使用循环处理错误:如果您想在错误时中断循环，请将 try-catch 排除在循环之外。否则将 try-catch 放入循环中继续

*出错时中断:*

```
try { 
  const numbers = [10, 2, 0, 5];
  numbers.forEach((num) => {
    if (num === 0) {
      throw new ArithmeticRangeError("zero cant be 0");
    }
    console.log(num);
  });
} catch (error) {} 
```

*出错时继续或跳过:*

```
const numbers = [10, 2, 0, 5];
numbers.forEach((num) => {
  try {
    if (num === 0) {
      throw new ArithmeticRangeError("zero cant be 0");
    }
  } catch (error) {}
  console.log(num);
});
```

*无试抓，逻辑处理:*

```
// filter zero, no need handle zeroconst numbers = [10, 2, 0, 5].filter((num) => num !== 0);
numbers.forEach((num) => {
  console.log(num);
});// Use some to break loopconst numbers = [10, 2, 0, 5];numbers.some((num) => {
  const isZero = num === 0;
  if (isZero) return true;
  // logic here
  console.log(num);
});
```

如您所见，根据您的需要，您可能不需要总是抛出错误。逻辑上可以处理。

**try-catch 中的多个错误:**

```
try {
  let name ;
  /// some operation
  if (name === "") throw new RangeError("Cant be blank");
  if (name.match(/\W/)) throw new TypeError("name cant be non alph-numric");
  throw new Error("Some other error");
} catch (error) {
  if (**error instanceof RangeError**) console.log("RangeError");
  else if (**error instanceof TypeError**) console.log("TypeError");
  else console.log("Other Error");
}
```

**先承诺后兑现中的多个错误:**

```
new Promise((resolve) => {
  let name;
  // some logic
  resolve(name);
})
  .then((name) => {
    if (name === "") throw new RangeError("Cant be blank");
    else return name;
  })
  .then((name) => {
    if (name.match(/\W/)) throw new TypeError("name cant be non alph-numric");
    else return name;
  })
  .catch((error) => {
    if (error instanceof RangeError) console.log("RangeError");
    else if (error instanceof TypeError) console.log("TypeError");
    else console.log("Other Error");
  });
```

## **5。错误处理编码实践**

以上所有代码都是非常标准和简单的错误处理用例。然而，当你在项目中工作时。代码可能没有这里给出的那么简单。所以我们需要写一些样板代码。下面，我列出了我在项目中遵循的一些模式。

1.  创建**枚举**类或错误**常量**
2.  从头开始使用**定位**
3.  通用 **util 模块或文件**处理逻辑并产生错误
4.  尽量减少的尝试性使用，而不是编写更多的单元测试用例
5.  在 API 调用中捕获并抛出一个定制错误
6.  **使用**类型脚本**减少错误**
7.  **尽量减少使用幻数/字符串**
8.  **避免更高层次的**嵌套对象****
9.  **避免全球**物体污染****
10.  **尽可能多地使用 **E** [**错误界限**](https://reactjs.org/docs/error-boundaries.html) (反应)**
11.  **正确记录，使用**控制台。错误**进行错误记录。**
12.  ****日志级别**最小化日志消息**
13.  **不要在日志中打印**凭证****
14.  **在网络应用中，使用**比控制台更多的视觉效果**。**

## ****6。结论****

**如您所见，有许多模式和实践可以遵循来改进您的代码库。这将减少代码中的错误。和应用程序的整体性能。**

**希望你喜欢这篇文章。让我们写一个干净安全的代码。**