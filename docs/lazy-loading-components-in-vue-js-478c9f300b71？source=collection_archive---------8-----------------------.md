# 在 Vue.js 中延迟加载组件

> 原文：<https://javascript.plainenglish.io/lazy-loading-components-in-vue-js-478c9f300b71?source=collection_archive---------8----------------------->

## **通过组件的惰性加载，提高 Vue.js 应用的性能。**

![](img/66e4f1b09d488f8d3cdbb9b099f3b078.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Vue.js 应用中优化性能是一个需要考虑的非常重要的因素。具有较高性能分数的应用程序将确保用户获得最佳的用户体验。

也将保证应用本身的效率。

## **什么是懒装？**

延迟加载确保使用延迟加载语法附加的组件在变得可见之前不会被加载。

顾名思义，延迟加载是提高 Vue.js 应用程序性能的最佳方法之一。

通过以下方式延迟加载组件被认为是一种很好的做法:

*   切换的内容
*   侧板
*   制表符
*   初始页面加载时不可用或不可见的组件
*   情态动词

## **组件的动态导入**

我们可以动态加载组件，这意味着所有可用的组件都将被下载，而不考虑初始页面加载时不需要的组件。这可能会影响我们的应用程序性能。

为了动态导入组件，我们使用本机动态导入语法，如下所示。

下面的代码片段将动态导入照片组件。

```
*// dynamically import photos component* *import* Photos *from* “@/components/photos;”;
```

## **组件的延迟加载**

为了延迟加载一个组件，我们使用基于“const”的变量，并附加一个箭头函数，后跟动态导入语法。

我们如何惰性加载组件？

要对 Vue.js 组件应用延迟加载，我们需要将组件的默认导入更改为基于变量“const”的组件，如下所示。

请参见下面的示例。

```
 *// lazy-loaded component* const Photos = () => import(*/* webpackChunkName: “Photos” */* “@/components/photos”); 
```

你看到被评论的 Webpack 的块名了吗？。

在注释 Webpack 的块名称中提供的名称将告诉 Webpack 我们想要分配给块的名称。否则，如果没有提供，Webpack 将自动为块生成一个名称。

当您进行束分析时，这可能很重要。

Webpack 的神奇注释是在注释中使用时影响构建过程的特殊短语。其中一个这样的魔评就是 `*/* webpackChunkName: "component"*/*` *。简单地把这个注释放入动态导入函数，你的资源就会被预取。*

当您想提高应用程序的加载性能时，延迟加载可以极大地提高 Vue.js 应用程序的性能。

也要考虑到它也有它的缺点。惰性加载应该小心地实现，并且只加载我们上面看到的不太重要的组件。

对于我们的应用程序中较大的延迟加载组件来说，这也很重要。

## **结论**

简单回顾一下，我们已经看到了导入组件的两种不同方式以及提高 Vue.js 应用程序性能的最佳实践。

我们也看到了惰性加载也有它的缺点。仅延迟加载页面呈现时不一定需要的组件或对我们的应用程序很重要的组件是非常重要的。

感谢您通读这篇文章。如果你觉得这篇文章有帮助，请不要犹豫，分享出来。