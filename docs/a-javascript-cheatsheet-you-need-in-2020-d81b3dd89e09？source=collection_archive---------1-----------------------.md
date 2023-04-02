# 2021 年你需要的 JavaScript Cheatsheet

> 原文：<https://javascript.plainenglish.io/a-javascript-cheatsheet-you-need-in-2020-d81b3dd89e09?source=collection_archive---------1----------------------->

![](img/e6408c144d96b48ab36c82114438a7cf.png)

我很确定我不是唯一一个在面试中遇到一些令人不舒服的技术问题的人，我觉得我知道 T2 发生了什么，但 T4 却无法解释。

我已经对这些问题和概念做了一段时间的总结，并选择了最具挑战性的 20 个。尽情享受吧！

## 【Arrow 和常规函数有什么区别？

*   **This** value (context):在常规函数中， *this* 的值与定义它的类无关；相反，它取决于被 ***调用的对象***；在箭头函数*中，这个*值总是等于来自外部函数的*这个*值
*   **构造函数**:常规函数可以轻松构造对象，而箭头函数不能作为构造函数使用
*   **Arguments** object:是一个特殊的类似数组的对象，包含调用函数所用的参数列表，在 arrow 函数中 *arguments* object 是按词法解析的:它从外部函数中访问参数
*   **隐式返回**:常规函数使用 return expression 语句——否则它只会返回 undefined，而对于 arrow 函数，如果它们包含一个表达式并且函数的花括号丢失了，那么表达式就会隐式返回
*   [阅读更多](https://dmitripavlutin.com/differences-between-arrow-and-regular-functions/)

## ***这个*是怎么工作的？**

*   ***这个*** 关键字指的是一个对象，这个对象正在执行 javascript 代码的当前位
*   每个 javascript 函数在执行时都有一个对其当前执行上下文的引用，称为*——执行上下文在这里意味着函数是如何被调用的*
*   *要理解 ***这个*** 关键字，我们只需要知道函数是如何、何时以及从哪里被调用的，函数是如何以及在哪里被声明或定义的并不重要*
*   *[阅读更多&参见示例](https://codeburst.io/all-about-this-and-new-keywords-in-javascript-38039f71780c)*

## *什么是**回调和关闭**？*

*   ***回调**:可由另一个函数访问并在第一个函数之后调用的函数——如果第一个函数完成的话*
*   *[阅读更多关于回调的](https://www.freecodecamp.org/news/javascript-callback-functions-what-are-callbacks-in-js-and-how-to-use-them/)*
*   ***闭包**:每当在当前作用域之外定义的变量从某个内部作用域中被访问时，就会创建闭包——它让你可以从内部函数访问外部函数的作用域*
*   *要使用闭包，只需在另一个函数中定义一个函数并公开它*
*   *[阅读更多关于闭包的](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#:~:text=A%20closure%20is%20the%20combination,scope%20from%20an%20inner%20function.)*

## *什么是**匿名函数**？*

*   *声明时没有任何函数名标识符来引用它的函数——它通常在最初创建后就不可访问了。这些函数是在运行时创建的*
*   *[阅读更多](https://www.javascripttutorial.net/javascript-anonymous-functions/)*

## *什么是**高阶函数**？*

*   *函数 r **接收一个函数作为参数**或**返回一个函数作为输出***
*   *例如，Array.prototype.map、Array.prototype.filter 和 Array.prototype.reduce 是该语言内置的一些高阶函数。*
*   *[更多在此](https://blog.bitsrc.io/understanding-higher-order-functions-in-javascript-75461803bad)*

## *JS 中的**“严格”模式**是什么？*

*   *严格模式支持代码中更严格的错误检查，并使调试更容易*
*   *您通过添加“使用严格”来启用严格模式；在文件的开头*
*   *[阅读更多](https://www.educative.io/edpresso/what-is-use-strict-in-javascript?utm_source=Google%20AdWords&aid=5082902844932096&utm_medium=cpc&utm_campaign=kb-dynamic-edpresso&gclid=CjwKCAjwoc_8BRAcEiwAzJevtZyt8ueI8zRKbsnF5_b0OYQXvsk4kA5GMACgxLhBfJQH-XNfPt_WmRoCC0wQAvD_BwE)*

## ***承诺 vs 异步/等待**？*

*   ***作用域**:在一个承诺中，只有承诺链是异步的——不阻塞执行；对于 async/await，整个包装函数是异步的*
*   ***逻辑**:在一个承诺中，可以在同一个回调中处理同步工作，使用 Promise.all 可以处理多个承诺；使用 async/await，同步工作需要放在回调之外，并且可以用更简单的变量处理多个承诺*
*   ***错误处理**:承诺:*然后，捕捉，最后*；异步/等待:*尝试，捕捉，最后**
*   *[参见此处示例](https://levelup.gitconnected.com/async-await-vs-promises-4fe98d11038f)*

## *JS 中**可变** vs **不可变***

*   *不可变和可变的区别:如果一个项目是可变的，当改变引用变量的值时，它也会影响原始引用变量的值*
*   ***原始数据类型**如数字、字符串和布尔类型**是不可变的**——不可能通过改变引用来改变这些类型的值——你可以组合它们并从中派生出新的值，但当你分配一个特定的值时，该值将始终保持不变；您可以让变量名指向一个新值，但是以前的值仍然保存在内存中*
*   ***可变**是一种可以通过引用改变的变量——在 JS 中只有**对象和数组**是可变的:你可以改变它们的属性；例如:使单个对象值在不同时间具有不同的内容*
*   *[阅读更多内容并查看示例](https://gomakethings.com/mutable-vs.-immutable-in-javascript/)*

## *什么是**类型语言**？*

*   *这些值与值相关联，而不是与变量相关联——它可以是静态的*或动态的**
*   ***静态**:变量只能保存一种类型，就像在 Java 中一样:声明为 string 的变量只能接受一组字符，除此之外什么也不能*
*   ***动态**:变量可以包含多种类型——就像在 JS 中一样:一个变量可以包含数字、字符等*

## *什么是**吊装**？*

*   *JavaScript 的默认行为是将所有声明移动到当前作用域的顶部(当前脚本或当前函数的顶部)*
*   *JavaScript 只提升*声明*，不提升*初始化**

## *JS 中的**事件委托**是什么？*

*   *一种简单的技术，通过这种技术可以向父元素添加单个事件处理程序，从而避免向多个子元素添加事件处理程序*
*   *使用事件委托，可以向一个元素添加一个事件处理程序，等待一个事件从一个子元素中冒泡出来，并很容易地确定事件来自哪个元素*
*   *[例题](https://www.sitepoint.com/javascript-event-delegation-is-easier-than-you-think/#:~:text=JavaScript%20event%20delegation%20is%20a,handlers%20to%20multiple%20child%20elements.)*

## ***调用**，**应用&绑定有什么区别？***

*   *通过使用它们，你可以明确地确定这个应该引用什么*
*   ***当您想使用事件来访问一个类中另一个类的属性时，绑定会特别有用***
*   ***主要区别在于， **apply** 立即执行函数，而 **bind** 返回函数***
*   *****调用**和**应用**非常相似——它们调用一个具有指定*上下文和可选参数的函数****
*   ***不同的是**调用**需要参数一个一个传入，而**应用**将参数作为数组***
*   ***[阅读更多内容并查看示例](https://www.taniarascia.com/this-bind-call-apply-javascript/)***

## *****主机**和**原生对象**有什么区别？***

*   ***根据环境和语言的不同，对象可以分为这两大类***
*   ***主体对象:特定于环境—例如。浏览器提供某些对象，如窗口，节点提供节点列表等。***
*   ***原生/内置对象:JS 提供的标准对象——有时也称为全局对象；JS 主要由这些分类的本地对象(字符串、数字等)构成***

## ***什么是**库里功能**？***

*   ***Currying 是函数式编程中的一个过程，在这个过程中，我们可以将一个具有多个参数的函数转换为一系列嵌套函数——它返回一个新函数，该函数需要内联下一个参数***
*   ***将函数从可调用的 f(a，b，c)转换为可调用的 f(a)(b)(c)的函数变换***
*   ***它让我们很容易得到部分变量，避免多次传递同一个变量***
*   ***它根据函数的参数数量创建嵌套函数，因此每个函数都接收一个参数:如果没有参数，就没有 currying。***
*   ***[阅读更多](https://javascript.info/currying-partials)***
*   ***[阅读更多 2](https://dev.to/suprabhasupi/currying-in-javascript-1k3l)***

## ***什么是**事件循环**？***

*   ***在大多数浏览器中，每个浏览器标签都有一个**事件循环，以使每个进程相互隔离，避免一个具有无限循环或繁重处理的网页阻塞整个浏览器*****
*   ***该环境管理多个并发事件循环，以处理 API 调用***
*   ***Web 工作者也在他们自己的事件循环中运行***
*   ***你主要需要关注的是你的代码将在一个**单事件循环**上运行，并在编写代码时牢记这一点以避免阻塞它***
*   ***任何花费太长时间将控制返回给事件循环的 JavaScript 代码都将**阻塞页面中任何 JavaScript 代码的执行**，甚至阻塞 UI 线程，并且用户不能四处点击、滚动页面等等***
*   ***[阅读更多](https://flaviocopes.com/javascript-event-loop/#:~:text=The%20event%20loop%20continuously%20checks,executes%20each%20one%20in%20order.)***

## ***什么是**事件传播**？***

*   ***事件传播是一种机制，它定义了事件如何通过 DOM 树传播或传播以到达其目标，以及之后会发生什么***
*   ***在现代浏览器中，事件传播分两个阶段进行:**捕获**，以及**冒泡**阶段***
*   ******捕获*** :事件从窗口向下通过 DOM 树传播到目标节点——仅在第三个参数设置为 **true** 时，与用 **addEventListener()** 方法注册的事件处理程序一起工作***
*   ****冒泡:在这个阶段，事件沿着 DOM 树向上传播或冒泡，从目标元素一直到窗口，逐个访问目标元素的所有祖先——所有浏览器都支持，并且它适用于所有处理程序，不管它们是如何注册的，例如使用 **onclick** 或 **addEventListener()******
*   ****[阅读更多](https://www.tutorialrepublic.com/javascript-tutorial/javascript-event-propagation.php#:~:text=Event%20propagation%20is%20a%20mechanism,what%20happens%20to%20it%20afterward.)****

## ****Cookies、本地存储和会话存储有什么区别？****

*   ****浏览器**存储**:数据不会传输到服务器&只能在客户端读取；存储限制约为 5MB****
*   ******会话与本地存储**:会话只是临时的——数据存储到标签/浏览器关闭；本地没有截止日期****
*   ****储存缺点:不安全，仅限于绳子，对 XSS 有危险****
*   ******Cookie** :存储有截止日期的数据，存储限制约为 4KB，针对每个请求发送到服务器，在客户端和服务器端均可读取/写入&(如果只有 http，则客户端脚本无法访问)****

## ****什么是**内容安全策略(CSP)** ？****

*   ****内容安全策略(CSP)是一个 **HTTP 头**，它允许站点运营商对他们站点上的资源可以从哪里加载进行细粒度控制。****
*   ****使用此标头是防止跨站脚本(XSS)漏洞的最佳方法。****
*   ****由于将 CSP 改造成现有网站的难度很大，CSP 对于所有新网站都是**强制性的**，并且强烈建议所有现有的高风险网站都采用 CSP。****
*   ****[阅读更多](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)****

## ****什么是**跨站脚本(XSS)** ？****

*   ****跨站点脚本(XSS)是一种**攻击**，当攻击者使用 web 应用程序向不同的最终用户发送恶意代码时，通常以浏览器端脚本的形式出现****
*   ****当有人请求时，服务器提供的页面是不变的；相反，XSS 攻击利用了页面中的一个**弱点**，该页面包含一个在请求中提交的变量，该变量以原始形式出现在响应中****
*   ****[阅读更多关于攻击的信息](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Cross-site_scripting_XSS)****

## ****什么是**CORS**？****

*   ******跨源资源共享** ( [CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS) )是一种**机制**，它使用额外的 [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP) 头来告诉浏览器让运行在一个[源](https://developer.mozilla.org/en-US/docs/Glossary/origin)的 web 应用程序访问来自不同源的选定资源****
*   ****当一个 web 应用程序请求一个与它自己的**来源**(域、协议或端口)不同的资源时，它执行一个跨来源的 HTTP 请求****
*   ****[阅读更多](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)****

****如果你喜欢这篇文章，也可以看看这些:****

****[](/html-css-concepts-you-might-have-missed-7f49893daaaf) [## 解释了 20 个 HTML 和 CSS 概念

### 我一直对网页开发的视觉方面充满热情，在谈到…

javascript.plainenglish.io](/html-css-concepts-you-might-have-missed-7f49893daaaf) [](/5-essential-third-party-tools-for-web-developers-cc4b3d04f924) [## Web 开发人员必备的 5 个第三方工具

### 如今，软件生态系统充满了不可或缺的工具，无论何时我们想要构建什么，我们都依赖这些工具…

javascript.plainenglish.io](/5-essential-third-party-tools-for-web-developers-cc4b3d04f924) 

如果你觉得遗漏了一些问题，并认为它们应该在那里，或者如果你想分享你的反馈，请在评论中告诉我！

希望这篇文章有所帮助，非常感谢您的阅读！编码快乐！

帮助我建立这个总结的一些来源:

[](https://www.fullstack.cafe/blog/front-end-developer-interview-questions) [## 50 个常见前端开发者面试问题【2019 版】| FullStack。咖啡馆

### 前端开发包括为 web 应用程序构建网页和用户界面。前端开发人员…

www.fullstack.cafe](https://www.fullstack.cafe/blog/front-end-developer-interview-questions) [](https://tsh.io/blog/frontend-interview-questions/) [## 前端面试问题——2020 年开发者小贴士| TSH.io

### 随着对前端开发的高需求的持续，技术来来去去和需求的变化，典型的…

tsh.io](https://tsh.io/blog/frontend-interview-questions/) [](https://www.bestinterviewquestion.com/front-end-developer-interview-questions) [## 2020 年前 20 个以上前端开发人员面试问题

### 一个前端开发人员应该具有功能知识和工作能力的所有方面涉及…

www.bestinterviewquestion.com](https://www.bestinterviewquestion.com/front-end-developer-interview-questions) [](https://frontendmasters.com/books/front-end-handbook/2018/practice/interview-q.html) [## 前端面试问题

### 前端开发人员手册是了解前端开发整个范围的有用资源…

frontendmasters.com](https://frontendmasters.com/books/front-end-handbook/2018/practice/interview-q.html) [](https://h5bp.org/Front-end-Developer-Interview-Questions/questions/javascript-questions/) [## JavaScript 问题★前端工作面试问题

### 一个有用的前端相关问题列表，你可以用来面试潜在的候选人，测试自己或…

h5bp.org](https://h5bp.org/Front-end-Developer-Interview-Questions/questions/javascript-questions/) [](https://dev.to/devabhijeet/all-front-end-interview-questions-asked-during-my-recent-job-hunt-1kge) [## 我最近找工作时问的所有前端面试问题。

### 这份自述是我最近在新冠肺炎找工作时被问到的所有问题的汇编。我还附上了一份清单…

开发到](https://dev.to/devabhijeet/all-front-end-interview-questions-asked-during-my-recent-job-hunt-1kge) [](https://www.edureka.co/blog/interview-questions/javascript-interview-questions/) [## 2020 年 50 大 JavaScript 面试问答| Edureka

### 这个 JavaScript 面试问题博客将为你提供一个关于 JavaScript 的深入知识，并为你做好准备…

www.edureka.co](https://www.edureka.co/blog/interview-questions/javascript-interview-questions/)****