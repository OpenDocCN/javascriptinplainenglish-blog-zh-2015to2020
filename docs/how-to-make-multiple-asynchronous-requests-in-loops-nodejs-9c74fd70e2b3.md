# 如何在节点中发出多个异步请求

> 原文：<https://javascript.plainenglish.io/how-to-make-multiple-asynchronous-requests-in-loops-nodejs-9c74fd70e2b3?source=collection_archive---------0----------------------->

## **我接受了一项任务，任务中提供了一组 URL，我必须下载每个 URL 的内容并存储在一个文件中**

![](img/f8c9c8df91c93bfacda9a2bb3f8b901e.png)

Pexels.com

我不知道你们中是否有人曾经面临过这个问题，但是我最近遇到了。我可以用承诺和承诺所有或可能。不同的 npm 模块。但是，问题是，回应是源源不断的。

所以，JavaScript IMO 是世界上最现代的复杂语言，它为我们所有的东西提供了一个解决方案。而对于这类问题，JavaScript 提供了**闭包**来节省我们的时间和精力。我用闭包来完成我的任务，我并不是说这是唯一的解决方案或最好的解决方案，但它确实是一个非常好的解决方案。所以，让我们从代码开始。

出于演示的目的，我使用了公共的 JsonPlaceHolder API。代码非常简单易懂。我所做的只是创建一个函数来处理请求部分，并在循环中调用该函数。

因此，每当调用**console result()**时，每次函数都会被执行，由于闭包属性，即使在循环结束后，函数也会记住它需要做什么，并将所有需要的内容存储在其**词法范围**和**闭包框**中，并继续执行其任务，即发出请求并记录结果。整个过程将是**异步的**，因此不会阻塞任何过程，因此不会浪费时间。

更棒的是，这将是未来的证明，数据来自**流**并且您需要 **fs** 。 **createWriteStream。**