# 面向初学者的 Web Workers

> 原文：<https://javascript.plainenglish.io/web-workers-for-beginners-57d7a389cded?source=collection_archive---------1----------------------->

![](img/e2ec16d0e2ee34edeb98014efea0fefe.png)

## JavaScript 是一种单线程语言，这意味着它按顺序执行代码，并且必须在执行完一段代码后才能进入下一段代码。

对于 HTTP 请求，JavaScript 为我们提供了非阻塞的 XMLHttpRequest。但是如果我们在程序中有一个 CPU 密集型代码，它将阻塞主线程直到它完成，使得 UI 在那段时间内没有响应。一种解决方法是使用 setTimeout 或 setInterval。我们可以将 CPU 密集型代码分解成更小的部分，并附加超时。虽然它可能适用于较小的程序，但随着代码复杂性的增加，决定在哪里添加超时函数变得越来越困难。我们需要的是一个独立的线程来完成这些计算，从而使主线程可以自由地处理 UI 事件、操纵 DOM 或执行其他任务。

## **网络工人前来救援**

HTML5 向我们介绍了 Web Worker。Web Workers 是运行在不同线程上的 JavaScript 程序，与主线程并行。它允许您将长时间运行和计算密集型任务放在后台，而不会阻塞用户界面，使您的应用程序响应更快。Worker 利用类似线程的消息在主脚本和后台脚本之间传递来实现并行性。

## 网络工作者的类型

通过为并行 JavaScript 程序创建单独的线程，工作线程允许我们实现多线程。有两种类型的网络工作者。

⦁ **专用工作器**:专用 Web 工作器由主进程实例化，只能与之通信。

⦁ **共享工作器**:共享工作器可以被运行在同一个起点上的所有进程访问(不同的窗口、浏览器标签、iframes 或其他共享工作器)。

> 本文将只涉及专门的工作者，我将通篇把他们称为' **web** **workers** 或' **workers** '。

## 入门指南

Web Workers 在浏览器中的一个孤立线程中运行。因此，它们执行的代码包含在单独的 **JavaScript** 文件中，并通过异步 HTTP 请求合并到您的页面中。为了创建一个工作线程，我们在主页上添加了以下几行。

```
var worker = new Worker(‘calculate.js’);
```

如果“ **calculate.js** ”文件存在并且可以访问，浏览器将生成一个新线程来异步下载该文件。下载完成后，它将被执行，并且 worker 将开始。如果所提供的文件路径返回 404，那么 worker 将会无声地失败。

要启动 worker，我们必须调用一个 **postMessage** ()方法。

```
worker.postMessage(message, [transfer]);
```

这里的**消息**是传递给工作页面的对象。一个**转移**是一个可转移对象的可选数组(我们将在后面的章节中讨论可转移对象)。

web worker 和主页面之间的通信通过两种方法进行，即 **postMessage** ()方法和“**message”**事件处理程序。

postMessage 可以接受一个字符串或 JSON 对象作为它的单个参数。新的浏览器支持 JSON 对象作为方法的第一个参数，而旧的浏览器只支持字符串。当从 **postMessage()** 发送一条消息时，另一个页面将接收该事件作为“**消息**”事件。对于错误处理，传递一个'**错误**'事件。

在上面的例子中，**main.html**用 **postMessage** 调用了两个 web workers(质数和阶乘)。我对质数和斐波那契方法使用了不同的 web workers，这两种方法都象征着大量的计算。

```
primeWorker.postMessage({cmd:'first50'});

fibonacciWorker.postMessage({cmd:'first100'});
```

工作文件本身正在监听' **message'** 事件，一旦收到事件，它们就进行计算，并借助 **self.postMessage** 方法将结果发送回来。我还添加了第三个按钮来改变背景颜色，它象征着用户界面的变化。当您从脚本中调用 prime 或 factorial 时，会产生一个独立的并行 web worker 线程，它将处理这些方法，并且您可以通过单击 change background 按钮在 UI 中并行地进行更改。

在 main.html 的文件中，我还添加了一个**错误**事件监听器。

```
fibonacciWorker.addEventListener('error', errorEvent, false);
```

如果在工作线程执行时发生错误，将触发 ErrorEvent。这个接口包含三个有用的属性，用于找出错误所在: **filename** —导致错误的工作脚本的名称， **lineno** —错误发生的行号，以及 **message** —对错误的有意义的描述。

你一定注意到了我在 **prime.js 中使用了" self "，在 **fibonacci.js** 中使用了"** this" ，这是因为在 worker 的上下文中，" self "和" this "都引用了 worker 的全局范围。

在这里，如果我们没有使用 web workers，那么在 fibonacci 和 prime 函数进行计算的时候，浏览器将会冻结，因此在那个时候您将无法与 UI 进行交互。

## 导入脚本和库

工作线程可以使用 **importScripts** ()方法导入外部脚本。它接受零个或多个 URIs 作为导入资源的参数。浏览器加载每个列出的脚本并执行它。来自每个脚本的任何全局对象然后可以被工作者使用。如果无法加载脚本，则抛出`NETWORK_ERROR`，后续代码不会执行。

```
importScripts(‘foo.js’); /* imports just “foo.js” */importScripts(‘foo.js’, ‘bar.js’);importScripts('//example.com/hello.js'); 
```

## 分包商

工作线程可能会产生更多的子工作线程。这对于在运行时进一步分解大型任务非常有用。然而，子工作者有他们自己的一些条件:

⦁子工作器必须与父页面托管在相同的源中。

子工作器中的 uris 是相对于其父工作器的位置来解析的。这使得员工更容易跟踪他们的依赖关系在哪里。

但是当产生子工作器时，我们必须小心不要占用太多的用户系统资源。

## 嵌入式工人

如果您想要即时创建工作脚本，或者不想为工作脚本创建单独的文件。借助 Blob 和使用数据块，您可以将它们全部嵌入到一个页面中。

使用 Blob()，您可以通过创建一个字符串形式的工作器代码的 URL 句柄，将工作器“内联”到与主逻辑相同的 HTML 文件中。

```
let blob = new Blob([
    "onmessage = function(e) { postMessage('msg from worker'); }"]);

*// Obtain a blob URL reference to our worker 'file'.*
let blobURL = window.URL.createObjectURL(blob);
```

神奇的事情发生在**窗口。URL.createObjectURL()** 。该方法创建一个简单的 URL 字符串，可用于引用存储在 DOM 文件或 Blob 对象中的数据。Blob URLs 是唯一的，并且在应用程序的整个生命周期中都有效(例如，直到文档被卸载)。

现在是聪明的部分。JavaScript(数据块)不会解析没有 src 属性或具有不标识可执行 MIME 类型的 type 属性的脚本元素。因此，我们可以在这个脚本标记中编写我们的工作代码，然后使用 Blob 为其创建一个 URL。

在上面的例子中，在脚本 id“worker-1”中，我添加了 web worker 的代码，然后在 JavaScript 解析脚本中为其创建了一个 Blob URL。

## 与员工之间传输数据

当我们使用 **postMessage** 时，我们在 web workers 和主页面之间传输数据。这些数据是复制的，而不是在两个页面之间共享的，也就是说，对象在被传递给工作者时被序列化，然后在另一端被反序列化。大多数浏览器将此功能实现为[*结构化克隆*](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm) *。*这对于小数据传输似乎没问题，但对于大数据量，这是一个明显的开销。

为了减轻这种性能损失，我们可以使用一个[可转移的](https://developer.mozilla.org/en-US/docs/Web/API/Transferable)对象。使用可转移对象，数据可以从一个上下文转移到另一个上下文。它极大地提高了向工作人员发送数据的性能。如果你来自 C/C++世界，可以把它看作是按引用传递。然而，与按引用传递不同，调用上下文中的“版本”一旦转移到新上下文中就不再可用。例如，当将一个 ArrayBuffer 从主应用程序转移到 Worker 时，原来的 ArrayBuffer 被清除，不再可用。它的内容被转移到 Worker 上下文。

要使用可转移对象，使用以下命令

```
worker.postMessage(arrayBuffer, [arrayBuffer]);
```

第一个参数是数据，第二个参数是应该传输的项目列表。

## 可供网络工作者使用的功能

由于多线程的特性，网络工作者只能访问 JavaScript 特性的一个子集。以下是功能列表:

航海家⦁物体

⦁位置对象(只读)

⦁ XMLHttpRequest

⦁ setTimeout()/clearTimeout()和 setInterval()/clearInterval()

⦁应用程序缓存

⦁使用 importScripts()导入外部脚本

⦁创造其他网络工作者

⦁ Web Worker 限制

网络工作者无法使用一些非常重要的 JavaScript 特性:

dom(它不是线程安全的)

窗口对象⦁

文档对象⦁

父对象⦁

## 用例

网络工作者有很大的权力，但是就像俗话说的那样“权力越大，责任越大”。同样，我们在使用它们的时候也应该谨慎。在后台任务必须与主 UI 任务并行执行或者所执行的任务必须对用户隐藏的情况下，这应该是优选的。它的一些其他用例可能是:

⦁预取和/或缓存数据以备后用。

⦁代码语法高亮显示或其他实时文本格式化。

⦁拼写检查器

⦁进步网络应用

⦁分析视频或音频数据。

web 服务的⦁后台 I/O 或轮询。

⦁处理大型数组或庞大的 JSON 响应。

<canvas>中的⦁图像滤波。</canvas>

⦁更新本地 web 数据库的许多行。

> 请注意:由于安全问题，您不能在浏览器上本地运行您的 web workers。我用了 Chrome 的网络服务器，效果很好。

感谢您抽出时间阅读这篇文章。在下一篇文章中，我将讨论服务工作者，一种不同但非常有用的 web 工作者。