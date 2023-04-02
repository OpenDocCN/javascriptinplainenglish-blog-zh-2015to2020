# 使用现有的 React 组件在 React Native 中构建实时预览

> 原文：<https://javascript.plainenglish.io/building-a-live-preview-in-react-native-using-your-existing-react-components-7fad9762811e?source=collection_archive---------6----------------------->

![](img/7dfa2b59f68bc5a423516588308d38da.png)

Photo by [Fabian Grohs](https://unsplash.com/@grohsfabian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我发现自己最近转换了一个基于 React 的网站，该网站为用户提供了他们正在构建的文档的实时预览，同时允许他们在打印之前轻松地更改文档的内容和样式。

预览效果真的很好，因为你可以快速地以你喜欢的方式设计文档，并将预览转换为 PDF，然后将文档发送给你需要的人，这比在 Word 中做这种工作有了很大的改进。

随着 React 网站向 React 原生应用的转换，我希望保留预览版，如果可能的话，重用预览版中使用的相同组件，而不是将大量模板移植到 React 原生应用。

这种方法虽然有点棘手，但可以让应用程序利用网页版上使用的所有模板，以及随着时间的推移添加的任何其他模板。

对于我的用例来说，这非常好，这意味着我可以编写一组组件来布局一个文档，并在 web、app 和 print 上呈现它，这使得支持多个平台变得更加容易。

## 示例反应组分

为了说明这个过程，我将创建一个示例 React 组件。该组件将在 React 网站中使用，以布局用户提供的一些信息，我们希望在 React 本机应用程序中重用这些信息。

This component simply returns the name and activity passed in and renders children (for things like inputs)

该组件应该以某种形式存在于可以导入 React 网站的组件库中，这一点很重要，如果该组件存储在本地，那么 React 和 React Native 在找到要导入的文件时会有问题。

Rendering the component in React, passing in a form as a child so it will render the form inside the layout

# 在 React Native 中渲染 React

来自`react-native-webview`的`Webview`组件可以呈现一个通过 props 传递给它的 HTML 字符串，所以我们将使用它来显示组件，但是我们不能只是传递给它组件，并希望它能理解我们需要先将其转换成 HTML。

`react-dom/server`模块有一个名为`renderToString`的函数，它接受一个 React 组件并返回一个 HTML 字符串，但由于我们正在使用的组件旨在在网页中呈现一个子树，它缺少它被标记为使用的 CSS 定义。

You’ll need to install react-native-webview and react-dom in order to use this example

您可以将从`renderToString`函数返回的 HTML 封装在一个 HTML 框架中，该框架添加标记以导入 CSS 框架，并添加`originWhitelist`属性以允许`Webview`下载这些标记正在使用的资源。

最终结果是在一个`Webview`中实时预览组件的输出，这个组件可以用 React 本地组件(如`TextInput`)来控制。

# 警告

值得指出的是，我所做的用例不需要实时更新，所以我无法验证这种技术的性能是否适用于需要即时更新的应用程序。

此外，如果您正在处理连接的组件，那么这种方法不太可能起作用，但是如果您重构组件以分离状态和结构，那么您应该能够使用这种方法。