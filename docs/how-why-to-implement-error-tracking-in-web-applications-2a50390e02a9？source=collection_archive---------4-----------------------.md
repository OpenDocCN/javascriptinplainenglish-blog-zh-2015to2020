# 如何&为什么在 Web 应用程序中实现错误跟踪

> 原文：<https://javascript.plainenglish.io/how-why-to-implement-error-tracking-in-web-applications-2a50390e02a9?source=collection_archive---------4----------------------->

![](img/9cf0ea7bb379446f49cf022a210c0a95.png)

Photo by [Max Chen](https://unsplash.com/@maxchen2k?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 更快地发现并修复关键软件问题

软件可以在各种设备上运行:服务器、网络浏览器、手机甚至你的手表。应用程序越成长，就会出现越多的错误。想想像脸书和 Airbnb 这样的公司，他们有数百万用户在各种设备上使用他们的产品。在一个需要快速发展的行业中，在开发过程中引入新的软件错误是不可避免的。但是，您可以大大改进查找和修复 bug 的过程。

## 为什么我们需要错误跟踪和监控？

*   **大部分用户不会报错**。如果你有一个 B2B 应用程序，用户很可能会报告错误，因为他们可能会为你使用它付费。对于 B2C 应用程序(如网上商店、报纸),用户不太可能报告错误，只会停止使用你的应用程序，而不会通知你。特别是如果用户第一次使用你的应用程序，他们很容易安静地退出。
*   **用户报错往往不够**。“我什么也买不到，请解决这个问题！”不是一个非常有用的错误报告。作为开发人员，您对错误日志、环境(例如浏览器)和重现错误的步骤感兴趣。
*   **有些错误很严重，需要尽快解决**。如果用户在使用 Mozilla Firefox 浏览器时无法登录，这需要尽快得到确认和解决。
*   **错误监控可以让你更好地理解问题发生的原因**。一个模式可能是，40%的错误报告是由于使用了在旧浏览器中不工作的 JavaScript 特性而发生的。
*   **软件测试无法发现所有问题**。虽然良好的测试覆盖率可以帮助防止 bug 被发布到产品中，但是你不可能测试所有的东西。单元测试不会捕捉所有问题，因为单独测试每个部分不同于测试整个系统。端到端测试执行起来相当慢，所以您通常只测试快乐的路径。

> “不要指望你的用户会报告错误”——sentry . io

这应该让您知道为什么拥有错误跟踪和监控是一个好主意。让我们看看这些服务能为我们做些什么。

## 错误跟踪和监控服务可以提供哪些功能？

*   **未处理异常的自动错误捕获**
*   **用于服务器、客户端&移动设备**的 SDK:这些包括最常见的语言，如 Java、PHP、JavaScript、Swift 和 C#
*   **警报和工作流集成**与 Slack、Pagerduty 和 JIRA 等流行工具的实时通知
*   **导致每个异常的事件的面包屑**:这使得开发人员更容易重现问题，因为您可以看到用户在遇到这个错误时做了什么
*   **环境和用户背景**:你可以获得有用的信息，比如网络浏览器版本、操作系统、网址和用户
*   **当错误发生时请求用户反馈**:有时候，你想让你的用户在应用程序出错时直接给出反馈
*   一个基本的免费版本，它提供了足够的入门功能

既然我们已经知道了错误跟踪和监控会带来什么，那么让我们来看看一些流行的解决方案。

## 有哪些流行的错误跟踪和监控解决方案？

*   [**Sentry**](https://sentry.io/welcome/) 是一个开源的错误跟踪解决方案，帮助开发人员实时监控和修复崩溃

*   [**滚动条**](https://rollbar.com/product/) 为敏捷开发和持续交付提供错误监控和崩溃报告

如上所述，像 Sentry 这样流行的错误跟踪解决方案可以在包括 web 应用程序在内的大多数平台上工作。在本教程中，我们将在 Angular web 应用程序中使用 Sentry 进行错误跟踪。然而，一般步骤同样适用于其他框架和平台。

![](img/0a77c0a0a9b47b5b37d08ab297660f09.png)

## 如何在 web 应用程序中实现哨兵错误监控

1.  [创建哨兵账户](https://sentry.io/signup/)并创建项目。
2.  安装 JavaScript 的哨兵库:`npm install @sentry/browser`
3.  我们需要在您的页面加载期间尽快初始化 Sentry 浏览器 SDK。对于 Angular 应用程序，在 AppModule 中这样做是有意义的。
4.  此外，可以配置 Sentry 来捕捉通过[@ Angular/core/error handler](https://angular.io/api/core/ErrorHandler)报告的任何特定于 Angular 的异常。
5.  可选项:当发生错误时，通过调用`Sentry.showReportDialog({eventId})`请求用户反馈。

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule, ErrorHandler, Injectable } from "@angular/core";
import { AppComponent } from "./app.component";

import * as Sentry from "@sentry/browser";

Sentry.init({
  dsn: "https://<key>@sentry.io/<project>"
});
// Angular specific code
@Injectable()
export class SentryErrorHandler implements ErrorHandler {

  handleError(error) {
    const eventId = Sentry.captureException(error.originalError || error);
    Sentry.showReportDialog({ eventId }); // optional
  } 
}

@NgModule({
  declarations: [AppComponent, ...],
  imports: [BrowserModule, ...],
  providers: [
    { provide: ErrorHandler, useClass: SentryErrorHandler },
    ...
  ],
  ...
})
export class AppModule {}
```

除了错误监控之外，还有一些方法可以减少交付到产品中的 bug 数量，我想简单介绍一下。

## 如何减少交付生产的 bug 数量

*   [**自动化测试和持续集成**](https://medium.com/@ali.dev/why-software-qa-matters-not-only-for-testers-and-how-to-get-into-it-65669f77e746) 有助于减少回归，更有信心地交付代码，并捕捉早期问题。您应该同时拥有单元测试和端到端测试。[如果你想了解更多关于软件质量保证](https://medium.com/sedeo/why-software-qa-matters-not-only-for-testers-and-how-to-get-into-it-65669f77e746)的信息，请点击这里。
*   做 [**代码评审**](https://medium.com/palantir/code-review-best-practices-19e02780015f) 另一个开发者评审你写的代码。
*   **跨浏览器/跨平台测试**像 [Browserstack](https://www.browserstack.com/) 这样的服务允许你在多个浏览器和平台上测试你的应用。

## 结论

感谢阅读这篇文章。正如您所看到的，错误跟踪和监控很容易设置，但提供了很多价值。你用的是什么解决方案？请在评论中告诉我。