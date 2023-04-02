# 用依赖倒置原则解耦 JavaScript 代码

> 原文：<https://javascript.plainenglish.io/decoupling-code-in-javascript-with-the-dependency-inversion-principle-6d23342b4aaa?source=collection_archive---------0----------------------->

## 使用可靠的原则构建健壮的软件。

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

Two monitors with background code.

SOLID 是由 Robert C. Martin 定义的 mnemotechnic 首字母缩略词，(通常被称为[Bob 叔叔](https://en.wikipedia.org/wiki/Robert_Cecil_Martin))是为了使软件设计更易于维护、理解和灵活而创建的五个设计原则。

这些原则结合在一起，使得开发人员能够轻松地构建更易于扩展和维护的软件。它们让开发人员可以轻松地重构代码，避免代码味道。它是现代敏捷软件开发的重要组成部分，如果你想开发出高质量且易于维护的软件，了解并应用它们是至关重要的。

我们在互联网上有很多关于它们的信息，但是当你试图在 javascript 中应用它们时，信息就少了。另一方面，我找到的例子几乎都是重复同样的案例，有时候直观上不太好理解。

软件应用程序设计的 5 个基本原则是:

S —单一责任原理(SRP)
O —开/闭原理(OCP)
L —李斯科夫替代原理(LSP)
I —界面分离原理(ISP)
**D —依赖倒置原理(DIP)**

在我将要写的关于这五个原则的系列文章中，我将谈论与我更相关的一个原则**:依赖倒置原则或 DIP，以及如何在 JavaScript 中实现它。**

## **从属倒置原则**

**依赖性反转原理或 DIP 指的是一种特定形式的解耦软件模块。当遵循这一原则时，从高级策略设置模块到低级依赖模块建立的传统依赖关系被颠倒。**

**这一原则的基础是:**

*   **高层模块不应该依赖低层模块。两者都应该依赖于抽象。**
*   **抽象不应该依赖于细节。细节应该依赖于抽象。**
*   **DIP 是关于在客户端代码使用的接口后面绑定类。**
*   **DIP 声明实现接口的类对于客户端代码是不可见的。**

**一般来说，它通过从外部提供元素的依赖关系来指示**解耦元素，而不是直接创建它们，这将创建附着**。**

**我们可以通过多种方式为实例提供必要的依赖关系，例如:**

*   **方法注入**
*   **构造函数注入**
*   **资产注入**

**但是，如何在 JavaScript 中实现 DIP 呢？**

**JavaScript 中依赖注入的另一个实现是带有 ES6 模块的**，能够使用和封装相同的代码，然后将它导入我们需要的地方。****

****在下面的例子中，我们有一个名为`DownloadToConsole`的类。在这个类中，我们有一个方法:`downloadDataFromAPI`，它从一个外部 API 下载数据，然后将其记录在控制台中。但是这个实现有什么问题呢？如果我们想使用另一种形式来获取数据，如 [Axios](https://github.com/axios/axios) 或 [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ，而不是 Fetch，该怎么办？****

****如果我们在所有的类中直接使用 Fetch 实现，我们将不得不用一个实现替换所有使用它的类中的另一个实现，因为**我们直接使用**，bud DIP 告诉我们应该使用接口，而不是实现。****

****让我们看一个例子:****

## ****DownloadToConsole.js****

****1.直接使用 Fetch。****

```
**const url = "https://jsonplaceholder.typicode.com/posts"class Example { constructor() {
  ...
 } downloadDataFromAPI(params) {

  //1.
  **fetch**(url, {
   method: 'GET'
  }).then(r => r.json())
    .then(r => {
      console.log("Posts:" + r);
   });
 }
}**
```

****在 JavaScript 中，我们没有接口，但是我们可以使用模块封装来实现类似的行为:例如，导出一个类或一个方法。****

****在这种情况下，为了简单起见，我们将导出一个用于下载数据的方法。此方法在内部使用 Fetch****

****现在，在一个 **utils.js** 文件中创建一个`doGet`方法来封装如何获取数据。****

## ****实用工具****

****2.这里我们可以使用 Fetch、Axios 或 XMLHttpRequest。****

```
**export const **doGet** = (url) => { //2.
 return **fetch**(url, {
        method: 'GET',
        mode: 'same-origin',
        ...
 }).then(r => r.json())};**
```

## ****DownloadToConsole.js****

****我们可以简单地导入`doGet` 依赖关系并使用它:****

```
**import { **doGet** } from './utils.js'const url = "https://jsonplaceholder.typicode.com/posts"class Example{ constructor() {
  ...
 } downloadDataFromAPI(params) {

  **doGet**(url)
   .then(r => {
    console.log("Posts:" + r);
   });
 }}**
```

****现在，代替 Fetch，如果我们想要使用 Axios，我们将只改变 **utils.js** 中的`doGet`实现来使用 Axios，并且一切将仍然像以前一样工作，而不必再做任何修改。****

## ****实用工具****

```
**export const **doGet** = (url) => {
  return **axios**.get(url);
};**
```

## ****结论****

****为了说明这个原理，我通过导出一个方法来尽可能简化它的使用，在这个方法中，下载数据的逻辑将使用 Fetch、Axios 或其他方法来实现。****

****在我们导入该方法的类中，我们不关心它在内部是如何实现的，这允许您将下载代码从实现中分离出来。****

****此外，在我们的例子中，通过封装代码，我们实现了不重复自己。([干](http://Don't repeat yourself (DRY, or sometimes do not repeat yourself))理)****

## ****参考****

****[](https://medium.com/javascript-in-plain-english/comparing-different-ways-to-make-http-requests-in-javascript-39ab0f090788) [## 比较用 Javascript 发出 HTTP 请求的不同方式

### 简明、有用的 JavaScript 课程——让它变得简单。

medium.com](https://medium.com/javascript-in-plain-english/comparing-different-ways-to-make-http-requests-in-javascript-39ab0f090788) 

坚实的原则:

[https://en.wikipedia.org/wiki/SOLID](https://en.wikipedia.org/wiki/SOLID)

谢谢你阅读我。希望你觉得有用！

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！******