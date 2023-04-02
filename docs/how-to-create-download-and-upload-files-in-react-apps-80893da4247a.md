# 如何在 React 应用中创建、下载和上传文件

> 原文：<https://javascript.plainenglish.io/how-to-create-download-and-upload-files-in-react-apps-80893da4247a?source=collection_archive---------0----------------------->

## 不需要服务器！

![](img/9837ed8701959f733a698003da11d0e4.png)

Photo by [Andrew Pons](https://unsplash.com/@imandrewpons?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

用 React 构建的丰富的单页应用程序(spa)。JS、Angular 和其他框架变得越来越流行。当 SPA 应用程序创建数据时，用户如何将数据作为文件下载到桌面？

上传工具:用户如何上传文件到 React。在浏览器上进行处理的 JS app？

TL；博士:[一个工作反应。通过 JSFiddle](https://jsfiddle.net/larrykluger/eo4dzptr/) 的 JS 示例。

# 从应用程序创建和下载文件

对于大多数 web 应用程序，文件是从服务器下载的。`[Content-type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)` 和`[Content-Disposition](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition)` 头用于声明文件类型，并向浏览器建议应该如何处理文件，包括建议的文件名。

在我们的例子中，我们希望在 SPA 中使用 JavaScript 创建或管理数据，然后允许用户将数据作为文件下载。不需要服务器！

以下是步骤:

# 选择并创建输出格式

JavaScript 变量和结构(对象和数组)以本机二进制格式处理。例如，考虑一个包含美国 2020 年选举摇摆州(宾夕法尼亚州、佛罗里达州、俄亥俄州、密歇根州、亚利桑那州、北卡罗来纳州、威斯康星州和爱荷华州)名称的 JavaScript 数组。数组在内存中的存储是特定于实现的:Firefox 的数组内存布局可能与 Chrome 的不同。

要将 swing states 数组输出为文件，首先选择一种输出格式。您可以决定将状态输出为文本文件中的报告，每个状态一行。您可以选择使用逗号分隔值(CSV)格式。或者 JSON，或者 XML。所有这些格式都是文本格式。

您可以选择二进制格式，比如通用二进制 JSON ( [UBJSON](https://ubjson.org/) )甚至 Excel 二进制文件格式。但是在 JavaScript 中很难操作二进制。我建议尽可能使用文本格式。

接下来，将原生 JavaScript 数据转换成所选的输出格式。

对于 JSON，使用`[JSON.stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)` 方法。我总是包含三到四个空格的缩进，以使输出可读。我认为能够轻松读取输出的价值超过了较大输出文件相对较低的成本。

虽然 JSON 支持使用数组作为顶层对象，但是不建议[这样做。最好将 JSON 创建为包含数组的对象:`{"states": ["Pennsylvania", "Florida", … ]}`](https://stackoverflow.com/a/3833393/64904)

对于 CSV，有多个 [JavaScript 生成器实现](https://stackoverflow.com/questions/14964035/how-to-export-javascript-array-info-to-csv-on-client-side)可用。XML 也不难创建。但是如果您将它存储在 DOM 中，请记住，在将它写入文件之前，您需要将该结构转换为字符串。

每个标量变量(不是对象、数组或其他结构的变量)都必须有一个字符串表示形式，以便以后在读取文件时进行解析。这对于字符串、整数和实数来说是简单而明显的。

对于任何其他类型的数据，您必须明确决定如何处理该数据类型。例如，JSON 原生支持`NULL`数据值。但是 CSV 没有:应该将`NULL`值存储为空字符串、字符串`"NULL"`、字符串`"**NULL**"`还是其他什么？这是你的选择。XML 通过`[xsi:nil](https://stackoverflow.com/questions/774192/what-is-the-correct-way-to-represent-null-xml-elements)`属性支持`NULL`值。

日期和日期时间 JavaScript 对象也是一个问题。一个常见的答案是使用 ISO 8601 标准将该值转换为文本，因为稍后可以对其进行解析以重新创建 date 对象。提示:记得考虑时区问题。

# 下载文件

有了 HTML5，以前的一些复杂问题就消除了。例如，锚元素的`download` 属性很容易用来设置建议的文件名。模式是这样的:

1.  用户通过连接到 JavaScript 方法的按钮启动下载。
2.  数据被转换成输出格式。结果是一个字符串。
3.  从字符串中创建一个`[Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)`。
4.  使用`[URL.createObjectURL](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL)`方法从`Blob`创建一个`Object URL`。
5.  隐藏锚元素的`href` 属性被设置为`Object URL`。
6.  锚元素的`click`方法被调用。通常情况下，当用户点击元素时会调用`click`方法。在这种情况下，我们以编程方式单击元素，因此用户只需在步骤 1 中启动下载。
7.  在`click`方法完成后，`Object URL`可以被释放。

一个[工作反应。下面是通过 JSFiddle](https://jsfiddle.net/larrykluger/eo4dzptr/) 的 JS 示例。点击**结果**标签。

# 专业提示

如果您正在下载应用程序的内部状态，以便将来可以上传和恢复它:

*   使用 JSON。
*   包括对象根级别的版本属性，以使数据格式能够在将来被更新，其中数据格式的版本容易被识别并被适当地解析。
*   不要试图“腌制”二进制格式的数据，将其转换为文本格式。
*   通过使用 BASE64 编码，图像或 PDF 文件等二进制数据可以存储在 JSON 数据结构中。

# 将文件上传到应用程序

我们可以将文件上传到浏览器中运行的应用程序，而不是将文件上传到服务器。应用程序可以在本地处理该文件。如果需要，该应用程序还可以通过 [Ajax](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX) 将文件上传到服务器。

在你的应用程序中创建一个[文件输入元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file)。在我的 JSFiddle 示例中，我通过一个带有我喜欢的按钮文本的按钮来触发 input 元素，但这不是必须的。

使用 input 元素的`multiple`属性来控制用户是否可以选择多个文件。使用`accept`属性控制可以选择的文件类型。 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file)提供了关于设置这些属性的附加信息。

将 onChange `attribute`设置为上传文件的处理方法。下面是文件输入元素和带注释的处理程序:

# 专业提示

*   不要对上传文件的内容做任何假设。例如，有人可以将二进制文件或图像重命名为“data.json”。
*   在我的例子中，我使用的是`[FileReader.readAsText](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsText)`方法。新的`[Blob.text](https://developer.mozilla.org/en-US/docs/Web/API/Blob/text)`方法也可以使用，但是它的浏览器支持不如`FileReader`对象完整。

# 摘要

从基于浏览器的代码中生成下载很容易。不需要涉及服务器！您的 SPA 应用程序可以生成和下载 CSV 文件、JSON 数据、文本报告等等。

将文件上传到您的 SPA 应用程序也很容易。将文件下载和上传到 SPA 应用程序而不是远程服务器，可以为您的用户提供更快、更出色的用户体验。在你的下一个应用中尝试一下。

# 资源

*   一个[工作反应。通过 JSFiddle](https://jsfiddle.net/larrykluger/eo4dzptr/) 的 JS 例子。
*   HTML5 [斑点](https://developer.mozilla.org/en-US/docs/Web/API/Blob)
*   html 5[URL . createobjecturl](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL)