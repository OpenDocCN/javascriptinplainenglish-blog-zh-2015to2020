# 作为前端开发人员，您需要了解的 6 个浏览器 API

> 原文：<https://javascript.plainenglish.io/6-browser-apis-you-need-to-know-as-a-front-end-developer-76752633280b?source=collection_archive---------1----------------------->

## 全屏 API，支付请求 API，以及更多详细解释的示例代码

![](img/92265b06a0b538fefe4b34bc7c5a51d8.png)

浏览器 API(或 web APIs)是浏览器内置的 API。它们允许开发人员执行复杂的操作，而无需处理复杂的底层代码。有许多用于操作 DOM、发出网络请求、管理客户端存储和检索设备媒体流等的浏览器 API。

目前，新的 web APIs 被引入，现有的 API 被频繁地弃用(例如:Battery API、WebVR API 等)，并且还有一些 API 尚未标准化。然而，仍然有一些有用的浏览器 API 被广泛应用于前端 web 开发领域。

本文向您展示了几个这样的浏览器 API，作为前端开发人员，您必须了解这些 API。无论您正在使用什么样的前端框架，对这些有一个很好的理解都将显著提高您的前端开发技能。

# 1.获取 API

Fetch API 是这个列表中的第一个。它用于执行来自浏览器的网络请求。在 Fetch API 引入之前，确实有一些浏览器内置的网络请求机制，但问题是它们都不是标准化的，因此开发人员在编写代码来执行网络请求时必须考虑所有可能的解决方案。引入 Fetch API 的目的是引入一个执行网络请求的标准。这个目标已经实现，因为目前所有主流浏览器都支持 fetch API。

Fetch API 提供了`Request`和`Response`的通用定义，因此它是通用的。即，可以在各种各样的环境中使用，例如服务工作者、缓存 API 等。每当需要发出网络请求时。它还隐藏了底层细节，并提供了比传统的 [XMLHTTPRequest API](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) 更多的对网络请求的控制。

使用 fetch API 真的很简单。它需要的是一个强制的端点 URL 和一个[可选的](https://developer.mozilla.org/en-US/docs/Web/API/Request#Properties) `[init](https://developer.mozilla.org/en-US/docs/Web/API/Request#Properties)` [对象](https://developer.mozilla.org/en-US/docs/Web/API/Request#Properties)。下面的 JS 片段展示了如何使用 fetch API 来执行各种类型的 HTTP 网络请求。

Fetch API Demo

[](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) [## 获取 API

### 获取 API 提供了获取资源的接口(包括通过网络)。对…似乎很熟悉

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 

# 2.画布 API

Canvas API 用于使用 JavaScript 和 HTML 绘制图形。它广泛应用于照片处理、视频处理、数据可视化和游戏开发等领域。(我最近体验到的 canvas API 的一个很酷的用法是将手绘数字签名放在用户协议网页上。)

Canvas API 的基本元素是`canvas` HTML 元素。一旦定义了一个`canvas`元素并用`getContext()`方法获得了它的上下文，就可以使用 Canvas API 对它执行图形操作。

以下代码片段显示了如何使用 Canvas API 来绘制形状。关于 Canvas API 高级特性的文档和全面深入的教程可以在官方 Canvas API 文档中找到。W3Schools 的 Canvas API 参考对任何入门者来说也是一个很好的资源。

[](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) [## 画布 API

### Canvas API 提供了一种通过 JavaScript 和 HTML canvas 元素绘制图形的方法。除其他外，它…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) 

# 3.全屏 API

有时你希望用户的注意力完全集中在某个特定的元素上。你不希望用户被浏览器地址栏和其他网站内容分散注意力，而是希望他完全专注于一件特定的事情。

这种场景的一个很好的例子是在电子商务商店中检查产品。当用户放大、缩小和旋转产品的 3D 模型时，你希望用户的全部注意力都集中在产品模型上。全屏显示产品模型是消除干扰的最佳解决方案之一，而全屏 API 就是这么做的。

全屏 API 是一个 web API，它允许特定的 DOM 元素全屏显示。将它整合到你的网站上是非常容易的，你只需要知道两种方法。

*   `**Element.requestFullScreen()**` -要求浏览器将指定的 DOM 元素及其所有后代(子元素)置于全屏模式，同时浏览器 UI 的其余部分从屏幕上移除。返回一个`Promise`，一旦全屏模式被完全激活，该问题就会解决。
*   `**document.exitFullScreen()**` -要求浏览器返回窗口模式。这个方法也返回一个`Promise`，一旦全屏模式完全关闭，这个问题就会解决。

[](https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API) [## 全屏 API

### Fullscreen API 添加了一些方法来以全屏模式显示特定的元素(及其后代),并退出…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API) 

# 4.剪贴板 API

开发人员在 web 应用程序中经常执行的一个常见任务是在点击图标/按钮时向/从剪贴板复制/粘贴。例如，如果您的 web 应用程序提供 API 密钥、密钥等，您可能希望他们通过点击按钮方便地复制这些值，而不是让他们突出显示这些值并按 CTRL+C 或 CMD+C。在这种情况下，我们可以使用剪贴板 API。

剪贴板 API 被实现为浏览器的`navigator`接口的属性。剪贴板 API 提供了以下方法。

*   `**clipboard.write()**` -将一组`ClipboardItem`对象写入剪贴板
*   `**clipboard.writeText()**` -将文本字符串写入剪贴板
*   `**clipboard.read()**` -将当前剪贴板内容作为`ClipboardItem`对象的数组读取
*   `**clipboard.readText()**` -以文本格式读取当前剪贴板内容

> 当页面处于活动选项卡中时，会自动授予页面“写入剪贴板”权限。然而，要从剪贴板中读取内容，用户应该显式地授予 `clipboard-read`权限。

Clipboard API demo

[](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API) [## 剪贴板 API

### 剪贴板 API 提供了响应剪贴板命令(剪切、复制和粘贴)以及…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API) 

# 5.付款申请 API

正如各种研究指出的那样，大多数在线购物车的放弃都发生在结账阶段，因为结账表格很难填写，而且很费时，通常需要多个步骤才能完成。

浏览器中引入了支付请求 API，以标准化网上结账体验，并减少在线支付的步骤。它会记住经常使用的支付信息(如卡信息、送货地址等)，让用户在更短的时间内进行在线支付。

使用付款申请 API 在您的网站上实施付款有许多优势。

*   使用付款请求 API 时，您可以让客户立即访问他们保存的付款信息，从而加快结账速度并降低流失率。
*   客户将在不同的网站上体验相同的 UX 支付方式，并会对此很熟悉。您的客户不再因为无法忍受您网站上 UX 的定制付款方式而放弃付款。
*   开发人员必须为集成编写更少的代码。一旦一个`PaymentRequest`对象被初始化，API 就会处理剩下的事情。您可以通过提供的 API 方法访问所需的内部信息。

> **注:**付款申请 API 仅适用于 HTTPS 环境。

付款请求 API 公开了如下几种方法。

*   `[**PaymentRequest()**](https://developer.mozilla.org/en-US/docs/Web/API/PaymentRequest)` -初始化一个新的`PaymentRequest`对象。在这样创建的`PaymentRequest`对象上调用后续方法
*   `**canMakePayment()**` -在您开始支付流程之前，检查`PaymentRequest`对象是否能够进行支付。它返回一个承诺，该承诺用一个布尔值来表示它是否是。
*   `[**show()**](https://developer.mozilla.org/en-US/docs/Web/API/PaymentRequest/show)` -发起支付请求并打开支付表单。这也返回一个用`[PaymentResponse](https://developer.mozilla.org/en-US/docs/Web/API/PaymentResponse)`对象解析的承诺。
*   `[**abort()**](https://developer.mozilla.org/en-US/docs/Web/API/PaymentRequest/abort)` -取消当前付款流程并关闭付款表单
*   `[**retry()**](https://developer.mozilla.org/en-US/docs/Web/API/PaymentResponse/retry)` -重试失败的支付

下面的代码片段包含了在您的网站上实现付款申请 API 所需要知道的一切。你可以点击这里观看现场演示[。在出现的支付表单中，提交一些样本卡信息，并在控制台中查找您提供的已记录的支付信息。](https://pavindulakshan.github.io/WebApiDemo/09_PaymentRequestApiDemo.html)

Payment reequest API demo

[](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API) [## 付款申请 API

### 支付请求 API 为商家和用户提供了一致的用户体验。这不是一种新的方式…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API) 

# 6.地理定位 API

地理定位 API 使用户能够让 web 应用程序知道他/她的位置。出于隐私原因，在检索位置之前，用户被要求明确的许可。对于检索用户位置的 web 扩展，它们需要将`“geolocation”`放在它们的`manifest.json`中的`permissions`下。

地理定位 API 主要提供两种方法。

*   `getCurrentPosition()` -返回设备的当前位置
*   `watchPosition()` -注册一个处理函数，该函数返回正在更改的设备位置上的更新后的当前用户位置

> **注意:**地理定位 API 仅在 HTTPS 环境下有效。

Geolocation API demo

[](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API) [## 地理定位 API

### 地理定位 API 允许用户向 web 应用程序提供他们的位置，如果他们需要的话。为了隐私…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API) 

# 结论

Web APIs 为开发人员提供了执行复杂操作而无需编写任何复杂代码的能力。尽管 web APIs 已被弃用或频繁更改，但仍有一些稳定的 API 在前端 web 开发中经常使用。对这些 API 有一个很好的理解肯定会让你的前端开发生活变得更加简单和高效。

感谢阅读！️️❤️