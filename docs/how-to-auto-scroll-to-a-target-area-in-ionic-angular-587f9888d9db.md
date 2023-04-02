# 如何自动滚动到离子角度的目标区域

> 原文：<https://javascript.plainenglish.io/how-to-auto-scroll-to-a-target-area-in-ionic-angular-587f9888d9db?source=collection_archive---------3----------------------->

![](img/74afeb6db963b2b4c2356373662c12ac.png)

Photo by [Silvan Arnet](https://unsplash.com/@silvanarnet?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我相信你一定遇到过这样的情况，用户在你的 web 应用程序中按下一个按钮，你希望应用程序滚动到你的应用程序中的一个特定部分。动画看起来很棒，但是你是如何达到这种效果的呢？

由于 Ionic Angular 是用 HTML 和 TypeScript 构建的，所以这种效果可以通过借用 HTML 和常规 JavaScript 的类似概念来实现。

对于这个特定的项目，我将使用以下版本构建 Ionic 应用程序:Ionic CLI 版本 6.11.8，Ionic 版本 5。

## 第一步

在 ID 为的页面上设置目标。在这个例子中，我在`home.page.html`的第 25 行将目标设置为 ID 为`#theTarget`的`<ion-item>`。除此之外，我还在第 12 行添加了一个按钮，我们稍后会用到它。

## 第二步

在`home.page.ts`中，您需要在第 1 行导入并使用`ViewChild`和`IonContent`，并在第 13 行声明它。然后，您可以创建一个函数，并使用下面第 21 行中的`.scrollToPoint`

## 第三步

如果你注意到了第 21 行，我在位置上增加了一点偏移。这是通过给`scrollToPoint`增加一点点偏移量来实现的，如下所示:

```
this.target.nativeElement.offsetTop - 40
```

这可以根据你的感觉调整到适合你的应用程序。

就是这样！

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！