# 使用信标和页面可见性 API 保存您的分析数据

> 原文：<https://javascript.plainenglish.io/save-your-analytics-data-with-the-beacon-and-pagevisibility-apis-f7d6a01087c4?source=collection_archive---------13----------------------->

![](img/ba737dcb3acd2ee23ba1d2f57148b5e4.png)

Photo by [matthaeus](https://unsplash.com/@matthaeus123?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/beacon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果您想确保在用户离开您的网站或 web 应用程序的特定部分之前捕获他们的一些信息，您可以使用 Beacon 和 PageVisivibility web APIs 来确保数据被成功发送到您自己的后端服务。

# 发送请求的问题

发送分析或日志信息的一种方法是捕获在 web 浏览器中的用户导航离开页面或关闭浏览器之前发生的事件。我们可以向后端服务发送一个网络请求，其中包含以下信息:

这有几个问题:

*   浏览器不会等待请求完成，所以它可能会被取消
*   使用一种技术来等待请求完成，即同步完成，可能会影响下一页的加载时间(如果适用)
*   一些浏览器会忽略`unload`处理程序中的请求，而其他浏览器根本不会触发`unload`事件(稍后会有更多内容)

这就是为什么使用 Beacon API 是一个更好的选择，如果您想确保对后端服务的最终请求完成，即使用户完全关闭了他们的浏览器。

# 信标应用编程接口有什么不同？

使用 Beacon API，页面发送的那些最终请求肯定会被发送。它们也是异步发送的，所以对下一页的加载时间没有副作用。

由于这些原因，没有必要编写任何额外的代码来确保那些最终请求被发送到您的后端服务，从而使您的代码更简单。

简而言之，信标 API 将

*   可靠地确保传递给`sendBeacon`功能的数据被发送到指定的 URL
*   异步发送请求，因此不会影响后续页面的加载时间

# 使用信标应用编程接口

那么，Beacon API 是如何工作的呢？

嗯，就像在`window.navigator`对象上调用一个函数一样简单。我们可以重写前面的例子，改用 Beacon API。

虽然[浏览器支持非常好](https://caniuse.com/?search=beacon)，但是在尝试调用`navigator`对象之前，您可能需要检查该对象上是否存在`sendBeacon`函数。

`sendBeacon`功能将向指定为第一个参数的 URL 发送 **POST** 请求。第二个参数是要发送的数据。

数据的格式可以是`ArrayBuffer`、`ArrayBufferView`、`Blob`、`DOMString`、`FormData`或`URLSearchParams`。

# 不要使用卸载事件

我之前提到过，当标签页关闭或者用户导航到另一个页面时，一些浏览器不会触发`unload`事件。手机浏览器尤其如此。

从[谷歌开发者网站的这个页面](https://developers.google.com/web/updates/2018/07/page-lifecycle-api#legacy-lifecycle-apis-to-avoid):

> 许多开发人员将`unload`事件视为有保证的回调，并将其用作会话结束信号来保存状态和发送分析数据，但这样做是**极其不可靠的**，尤其是在移动设备上！`unload`事件在许多典型的卸载情况下不会触发，包括从移动标签切换器关闭标签或者从应用切换器关闭浏览器应用。

那么，当寻呼会话结束时，发送信标请求的最佳方法是什么呢？嗯，我们可以使用 PageVisibility API 来确定用户是否已经离开了当前选项卡。

# 使用页面可见性 API

顾名思义，PageVisibility API 可以用来确定一个文档在用户的显示器上是否可见，也就是说，用户的选项卡是否在焦点上。

您可以从`document.visibilityState`中访问当前状态，它将返回`prerender`、`hidden`中的一个，或者如果您正在开发工具`visible`中运行它，则最有可能返回一个。您可以使用此属性在代码中的任何特定点检查选项卡当前是否对用户可见。

这对于我们的情况来说没什么用，但是您可以通过一个事件来了解`visibilityState`的变化。因此，当这种情况发生变化时，您可以发送信标请求。

因此，现在当用户导航离开页面时，将触发`visibilityChange`事件，我们可以安全地将我们的分析或日志信息发送到我们的后端服务。

# 一个完整的例子

让我们来看一个完整的例子，您可以在本地运行并进行实验。

首先，使用 Express 的依赖项设置一个项目:

```
mkdir beacon-example
cd beacon-example
npm init -y
npm install express body-parser
mkdir public
touch public/index.html
touch app.js
```

然后，在`app.js`文件中，设置一个 Express 服务器来接收我们的信标请求

将 Express 服务器设置为运行:

```
node app.js
```

然后，在包含在`public`文件夹中的`index.html`文件中，我们可以添加一些 JavaScript 代码来发送信标请求。

虽然这有点粗糙，但希望你能理解如何收集关于用户如何与你的页面或 web 应用程序交互的信息，并使用`sendBeacon`请求将这些信息传回你的后端服务。

您可以通过用关于其他事件的信息更新`events`数组来扩展这个例子。例如，当用户单击页面上的任意位置时:

如果您前往运行后端 Express 服务的终端，您会看到如下所示的输出:

```
Received Beacon:  [
  { type: 'Session Start', time: 1603812976748 },
  { type: 'Click', x: 122, y: 151 },
  { type: 'Session End', time: 1603812979156 }
]
```

# 结论

在本教程中，我们已经了解了通过简单的 HTTP 调用向后端服务发送分析和日志信息的问题，以及为什么不应该使用`unload`事件来侦听会话的结束。

我们现在知道，Beacon API 提供了一种安全可靠的方式来将这种数据传递回我们自己的后端服务，而不会影响后续的页面加载时间。

此外，我们还看到了 PageVisibility API 如何提供一个安全事件，可以告诉我们用户何时离开了某个页面，即使是在移动设备上。

感谢阅读。如果你觉得这很有用，请[在 Twitter 上关注我(@codebubb)](https://twitter.com/codebubb) 获取更多教程。