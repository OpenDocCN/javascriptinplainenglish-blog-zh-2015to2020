# 用 AbortController 取消 JavaScript 异步任务

> 原文：<https://javascript.plainenglish.io/canceling-javascript-async-tasks-with-abortcontroller-acda9f67f7e7?source=collection_archive---------2----------------------->

## 如何将 AbortController 用于 Fetch 或其他异步任务

![](img/d17d6f45443de88c2ab85e4770ada9e7.png)

当我们在 JavaScript 中使用 Fetch 或其他异步函数时，有时我们可能想要取消它们。JavaScript 提供了中止异步任务的不同方式。在本文中，我们将了解如何应用 AbortController 对象来中止获取请求和其他异步任务。

## 语法:

**new AbortController()**
返回一个新的控制器，其信号设置为一个新创建的 AbortSignal 对象。

**控制器。信号**T5 返回与该对象关联的 AbortSignal 对象。

**控制器。abort()**

## 中止异步任务

AbortController 是中止异步任务的标准对象，我们可以用它来停止异步任务。在下一个例子中，假设我们有一个需要很长时间处理的异步函数。在这种情况下，generateReport 函数需要 20 秒才能完成，但是如果您愿意，您可以随时终止执行，调用 AbortController.abort()方法。

```
let acontroller = new AbortController();
let signal = acontroller.signal;function generateReport(signal) {
  return new Promise((resolve, reject)=> {

   const error = new DOMException(
                  'aborted!',
                  'AbortError');//[3]if (signal.aborted){
      return reject(error);//[2]
   }const timeout = setTimeout(() => {
      resolve('you report!'); 
    },20000);signal.addEventListener('abort',() => {
      clearTimeout(timeout);
      reject(error);
    });});
}generateReport(signal).then((report)=>{
  console.log( report );
}).catch( e => console.log(e));acontroller.abort();//[1]
//aborted!
```

解释:

当调用 acontroller.abort() [1]时，generateReport()函数拒绝[2]承诺，并给出适当的错误[3]，而不做任何进一步的操作。

## 中止提取请求

我们可以使用 AbortController 中止 Fetch 请求，因为 Fetch 具有与 AbortController 的内置集成:

```
const url = 'https://server/api/d1';let aController = new AbortController();let signal = aController.signal;const response = 
    await fetch(url,{signal});const data = await response.json();aController.abort();
```

AbortController 也允许一次取消多个获取请求:

```
const url1 = 'https://server/api/d1';
const url2 = 'https://server/api/d2';let urls = [url1 ,url2'];let aController = new AbortController();let tasks = urls.map(url => fetch(url,{
  signal: aController.signal
}));let results = await Promise.all(tasks);//Abort all requests
aController.abort();
```

## fetch 与经典 XMLHttpRequest 的比较

取得

这里我们使用 AbortController.abort()方法中止一个获取请求:

```
const aController = new AbortController;fetch('https://server/api', {
    signal: controller.signal
}).then(response => {
    if (response.ok) {
        console.log(response.text());
    }
});
```

XMLHttpRequest

这里，在 XMLHttpRequest 中，XMLHttpRequest abort()方法也可以用来取消请求:

```
const url = 'https://server/api/d1';
const xhr = new XMLHttpRequest;xhr.addEventListener('load', () => {
    if (xhr.status === 200) {
        xhr.responseType = 'text';
        console.log(xhr.response);
    }
});xhr.open('GET',url);
xhr.send();//Abort request
xhr.abort();
```

## 在不同的环境中使用 AbortController

JavaScript 代码可以在三种主要环境中运行:

*   现代浏览器:您可以毫无问题地使用 AbortController，因为 AbortController 是由 WHATWG 创建来处理 DOM 的。
*   Node:如果想在 Node 中使用 AbortController，可以使用 WHATWG AbortController 接口的实现。您可以使用不同的 npm 包来实现这一点，例如， [node-fetch](https://www.npmjs.com/package/node-fetch) 或 npm [abort-controller](https://www.npmjs.com/package/abort-controller) 。
*   遗留浏览器:你需要使用 polyfills，例如，使用 [Babel](https://babeljs.io/) 。

# 结论

尽管 AbortController 是 DOM 的一个特性，而不是语言本身的一个特性，但它确实为异步任务提供了一个标准化的、简单的取消解决方案。请记住，即使我们取消了获取请求，这也不会停止后端的请求处理。

我希望你喜欢这篇文章。感谢阅读。