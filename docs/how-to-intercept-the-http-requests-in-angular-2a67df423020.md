# 如何在 Angular 中拦截 HTTP 请求(第 1 部分)

> 原文：<https://javascript.plainenglish.io/how-to-intercept-the-http-requests-in-angular-2a67df423020?source=collection_archive---------3----------------------->

![](img/0dfc858a03d5e16860e1f655f90f384f.png)

Photo by [Denys Nevozhai](https://unsplash.com/@dnevozhai?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/interchange?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# **内容**

本文由第一部分和第二部分两部分组成。

第 1 部分-简介，角拦截器的一些优点。

第 2 部分——实现拦截器的实用说明。

# **简介**

从 Angular 4.3 开始，一个新的特性就出现了，它可以全局地修改所有传出的 http 请求和传入的响应。换句话说，我们可以预处理所有的 http 请求和响应。有了这个新特性，Angular 开发人员可以减少不必要的工作，用更少的精力和更少的代码来完成任务。

在进入编码和实现部分之前，让我列出 http 拦截器可以做什么，这样您就可以很好地了解我们为什么需要拦截器以及哪里需要拦截器。

通过 HttpInterceptor 接口，

## **1。您可以添加新的头和修改 http 请求的现有头。**

[了解更多信息](https://stackoverflow.com/questions/48683476/how-to-add-multiple-headers-in-angular-5-httpinterceptor)

## 2.对于某些 http 错误，我们可以多次重试请求。

网络错误是常见的，我们可以多次重试请求以获得成功。

[了解更多..](https://stackoverflow.com/questions/51905963/retry-http-requests-in-angular-6\)

**3。我们可以创建一个假的后端并模拟响应。**

[了解更多信息..](https://blog.bitsrc.io/how-to-mock-a-backend-in-angular-using-httpinterceptor-667794d45e8d)

**4。我们可以缓存响应。**

[了解更多信息..](https://medium.com/@vikeshm/data-caching-angular-http-interceptor-2d87f95e2340)

**5。可以进行响应的转换。**

当 API 以我们不喜欢的格式发送数据时，我们可以在这里将其转换为我们喜欢的格式。(例如:XML >>>> Json)

6。获取数据时可以显示加载程序。

7。我们可以操纵和改变网址

比方说，我们想改变*。com 的*部分 URL 转换成*。lk* 由于某种原因。这里也可以。

# 结论

拦截器在 Angular 中是一个很好的工具，在这里你可以用更少的努力做更好的事情，而不需要复制你的代码并创建一个惊人的应用程序。一切都取决于你的创造力。

让我们实现一个拦截器类，并用它做一些小把戏。阅读本文的下一部分:

[如何在 Angular 中拦截 HTTP 请求(第二部分)](https://medium.com/javascript-in-plain-english/how-to-intercept-the-http-requests-in-angular-part-2-a5aface03744)

享受这一天！！！