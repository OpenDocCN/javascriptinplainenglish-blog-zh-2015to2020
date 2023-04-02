# StencilJS:通过三个简单的步骤异步显示 StencilJS 中数组的列表

> 原文：<https://javascript.plainenglish.io/stenciljs-displaying-a-list-from-an-array-in-stenciljs-asynchronously-in-3-easy-steps-d96edd00e13e?source=collection_archive---------4----------------------->

## 学习如何用 StencilJS 一步一步地构建 web 组件。易于安装，但理解基本的 JSX 命令有点问题。

![](img/387559a764843b7b883b51012d95abd7.png)

Photo by [Safar Safarov](https://unsplash.com/@codestorm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

T4:但是探索非常有趣。我最近在 web 组件方面做了很多工作，特别关注 StencilJS。

## 什么是 web 组件？

摘自[webcomponents.org](http://www.webcomponents.org)，

> Web 组件是一组 web 平台 API，允许您创建新的自定义的、可重用的、封装的 HTML 标记，以便在网页和 web 应用程序中使用。定制组件和小部件建立在 Web 组件标准的基础上，可以跨现代浏览器工作，并且可以与任何支持 HTML 的 JavaScript 库或框架一起使用。
> 
> Web 组件基于现有的 web 标准。支持 web 组件的特性目前正被添加到 HTML 和 DOM 规范中，让 web 开发人员可以轻松地用封装样式和定制行为的新元素扩展 HTML。

在任何网站中实现 web 组件都非常容易。例如，如果您已经构建了一个名为`my-picture`的 web 组件，那么要将您的 web 组件放在任何地方，您所要做的就是包含标签:

`<my-picture></my-picture>`

任何你想用它的地方`my-picture`

就这么简单。

## 为什么是 StencilJS？

除了构建应用程序或 Ionic PWA，StencilJS 还可以用于轻松开发 web 组件，这是一个非常完整和高效的框架。

StencilJS 是 Ionic 团队的另一个项目，对于我来说，如果你有兴趣了解更多关于 web 组件的知识，这是一个很好的开始。

我发现 StencilJS 非常轻便，易于操作，更不用说，安装和设置它几乎不需要什么工作或麻烦。你所需要的只是节点和 NPM，仅此而已。要启动 StencilJS 项目，您只需键入以下内容:

```
npm init stencil
```

就是这样。现在您有了一个完全成熟的 StencilJS 项目。

然而，您需要了解一些代码，并且有一个轻微的学习曲线来理解需要做什么来完成您需要它做的事情。需要注意的是，为了理解 StencilJS 中发生的事情，你需要能够理解很多 JSX，类似于 React。

最近，我遇到了一个简单直接的任务，如果要在 Angular/Ionic 上执行，但很难在 StencilJS 中解决。我在 JSX 和 React 上没有什么经验，Google 上的很多解决方案似乎都集中在用 React 解决同样的问题，而不是用 StencilJS。

如果您目前正在构建一个 StencilJS web 组件，并且像我一样到处寻找解决方案，那么您已经找到了合适的文章。

编写时的 StencilJS 版本:

```
“@stencil/core”: “1.8.7”
```

## **第一步:**

`componentWillRender()`功能

类似于 Angular 中的`ngOnInit()`，在用`render()`渲染页面之前会先执行`componentWillRender()`

为什么这一点很重要？事情是这样的，`render()`不接受异步等待和承诺。事情不是这样的。因此，您需要创建一个函数来运行您的异步 await，以便准备好一切，然后呈现它。

这就是为什么在执行`render()`函数之前，你需要创建一个`componentWillRender()`来将所有的加载内容分类，我假设这个函数已经内置到你的应用程序中了。

因此，像这样准备:

```
theList;async componentWillRender(){//do your asynchronous stuff here let getApi=await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)',{
method:'GET'}) this.theList=await getApi.json()}
```

## 第二步:

在渲染函数中添加`.map`来循环 web 组件中的所有内容。在本例中，我将在 div 中显示 List 数组中的所有内容:

```
render(){ 

      return <div> {this.theList.map((item:any={})=> <div> {item.id} {item.name} </div> )} </div>}
```

额外收获:如果你正在寻找开发离子风格，如离子按钮，离子项目，离子列表，离子工具栏等，你可以很容易地导入离子属性，并在 JSX 使用它们。关于如何做到这一点，请参见下面的参考资料。

## 第三步:

运行构建和启动，尝试一下。

```
npm run build
npm start
```

给你。三个简单的步骤。就是这样。我提到过使用模板很容易构建 web 组件吗？

参考资料:

[](https://ionicframework.com/docs/installation/cdn) [## 离子包.离子文件

### Ionic 提供了不同的软件包，可用于在测试中快速开始使用 Ionic Framework 或 Ionicons

ionicframework.com](https://ionicframework.com/docs/installation/cdn) [](http://stenciljs.com) [## 蜡纸

### 神奇的、可重用的 web 组件编译器。几秒钟内开始构建。通过有意设计的小工具、小 API…

stenciljs.com](http://stenciljs.com)  [## JSONPlaceholder

### JSONPlaceholder 是一个免费的在线 REST API，只要你需要一些假数据，就可以使用它。对于教程来说很棒…

jsonplaceholder.typicode.com](http://jsonplaceholder.typicode.com) [](https://www.webcomponents.org) [## webcomponents.org

### Bryan Ollendyke 是 HAX the web，web 组件创作系统的负责人，也是 ELMS: Learning…

www.webcomponents.org](https://www.webcomponents.org)