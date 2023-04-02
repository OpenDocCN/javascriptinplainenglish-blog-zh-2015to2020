# 我在 Node.js 全球峰会 2020 上的经历:疫情中央的虚拟峰会

> 原文：<https://javascript.plainenglish.io/nodejs-global-summit-2020-virtual-summit-in-the-middle-of-a-pandemic-part-1-b9d5ae846f54?source=collection_archive---------7----------------------->

*Assalamu'alaikum* ，我是 **Pandhu Wibowo** ，这是我在**媒体**上的第一篇博文。我来自**印尼**，我是**软件工程师**。

2 天前，我加入了 **Node.js 全球峰会 2020** 。许多伟大的演讲者出席了这次活动。得到了很多知识，新朋友等等。我很高兴成为他们中的一员。这个活动持续了一整天。在印度尼西亚，开始时间是下午 6 点(2020 年 5 月 30 日)，结束时间也是下午 6 点(2020 年 5 月 31 日)。

![](img/9f5ed6094f57dcfa71907692932bc61b.png)

Source: [https://geekle.us/](https://geekle.us/)

对于这一部分，我不会重复分享的知识，但我只想与你分享我真的参加了活动的证据，哈哈。

我想我有这次活动的大部分幻灯片，我会与你们分享。

*   **web assembly 系统接口和 Node.js (Colin Ihrig) (** 很抱歉没能拿到幻灯片)

*“Web Assembly System Interface，或称 WASI，是一项相对较新的实验性技术，它通过一组系统调用 API，使沙盒 Web Assembly 应用程序能够访问底层系统。虽然 WASI 引起了很多兴奋，但仍有许多细节需要理清。Node.js 的最新版本为该平台增加了实验性的 WASI 支持。这个演讲介绍了 WASI 的基础知识，包括如何编写一个利用 WASI 的应用程序。演讲还探讨了未来的工作方向，以便在 Node.js 和 libuv 中更好地支持 WASI*

*   **Node.js 从单线程到 worker _ threads(Sebastian Curland)——**([链接](https://github.com/sebastian-curland/worker-threads-presentation/blob/master/Node_%20from%20single%20thread%20to%20worker%20threads.pdf))

“如果 Node.js 有一个致命弱点，那就是运行 cpu 密集型任务。嗯，至少那是在 worker_threads 模块登陆 Node 10 并发展成为我们今天拥有的稳定 API 之前。在本次会议中，我将回顾我们必须在工作线程之前运行 cpu 密集型代码的替代方案，并将介绍新的 API 及其特殊之处。”

*   **Node.js EventLoop 和 libuv(Nairi Harutyunyan)——**([链接](https://slides.com/nairihar/geekle-20#/) ) |另一个引用= > ( [链接](https://blog.insiderattack.net/event-loop-and-the-big-picture-nodejs-event-loop-part-1-1cb67a182810))

*“我会讲 Node.js EventLoop，更详细的讲讲 libuv 的神奇之处。深入探讨 EventLoop 队列，了解 nextTick、Promise、setTimeout、setImmediate 和所有其他异步 I/O 进程的工作方式。”*

*   **五十年代。接下来，Node.js 中的新 V8(Olivier lover de)——**([链接](https://olivier.codes/2020/04/12/ES2020-Summary-of-new-features-with-examples/) )( [示例](https://github.com/olivierloverde/es2020-examples))

*“深入探讨 ES2020 中的所有新功能，包括示例:BigInt、Promise.allSettled、Nullish 合并运算符、可选链接运算符、动态导入、String.prototype.matchAll、global this”*

*   **ES6 模块的路径(Markus Hosel) -** ( [链接](https://github.com/haezl/path-to-es-modules))

*“本讲座通过回顾 IIFEs 和 CommonJS，展示了 ES6 模块系统的重要性，并展示了 ES6 模块系统的优势。谈话会以编码示例为后盾。”*

*   **node . js 中的微服务架构——自动化测试(Karin Angel)**——([链接](https://www.youtube.com/watch?v=gdXL_pLNDq0))

*“作为一个大型开发团队中的开发人员，在微服务架构上构建一个由数十个服务组成的系统，最大的痛苦是在不破坏任何东西的情况下将代码推送到代码库。测试是解决这一难题的关键。或者，换句话说，建立一个健康的 CI-CD 过程，在缺陷到达客户之前发现它们。在这次演讲中，我们将探索不同的测试方法，这些方法将帮助我们交付有效的代码，并且不会破坏旧的功能。我们将讨论服务级别的不同测试方法——单元测试和 API 测试。我们将讨论集成测试的重要性，以及在我们使用生产实践中的测试将一段新代码部署到生产中后，我们可以做些什么。”*

我想这就足够了，我会在另一部分继续。

谢谢你来看我的第一篇博客。祝你有美好的一天！

*更多内容请看*[***plain English . io***](http://plainenglish.io/)