# 强烈输入您的角度路线数据

> 原文：<https://javascript.plainenglish.io/strongly-typing-your-angular-route-data-ddef7719491f?source=collection_archive---------1----------------------->

随着 Angular 项目的增长，向路径中添加数据是很常见的。有些数据对所有路由都是强制性的，有些则不是。但是在任何情况下，强类型化你的路线都是有用的，就像你的代码库的其他部分一样。

![](img/b7c1b3f831cdedd1e731d1cec160044f.png)

Image courtesy of [Brendan Church](https://unsplash.com/@bdchu614)

在本文中，我将快速浏览角度路由器的基本数据类型，然后我们将探索如何强类型化我们的角度路由。

# 角度路由器 API 数据类型

Angular 路由器 API 为我们提供了多种数据类型。这里是一个真正的快速回顾。

最重要的当然是[路线](https://angular.io/api/router/Route)。路由对象定义了单条路由的配置，其中*可能*包括(除其他之外):

*   一条小路
*   一个组件
*   目标路由器出口
*   防护装置(可激活、可停用等)
*   解决方案(如果可以的话，忘掉那些！)
*   儿童路线
*   数据
*   …

如果你熟悉 Angular 路由器，那么你可能对所有这些都了如指掌。

data 属性是您可以用来嵌入附加数据的属性，它将作为 [ActivatedRoute](https://angular.io/api/router/ActivatedRoute) 的一部分由路由器提供。ActivatedRoute 可以很容易地注入到您的组件或服务中(例如，通过`firstChild?.routeConfig?.data`)；我已经在我最近写的滚动服务中利用了这个特性。

当然，在您的应用程序中几乎总是会有不止一条路线。所有这些路由都需要添加到一个 [Routes](https://angular.io/api/router/Routes) 对象中，这个对象只不过是一个 Route 对象的数组(类型简单地定义为`type Routes = Route[]`)。

一旦定义了 Routes 对象，您只需将它提供给[router module](https://angular.io/api/router/RouterModule)；要么通过`RouterModule.forRoot`要么通过`RouterModule.forChild`，这取决于你是在根模块中还是在一个惰性加载/子模块中。

一旦定义了路由器配置，就可以开始了。当 Angular 路由器需要激活不同的路由时，它会尝试将目的地 URL 的段与它所接收到的不同路由对象进行匹配。但这不是本文的主题，我们就讲到这里。

既然我们已经回顾了基本类型，让我们来介绍一下我们自己的类型吧！

# 强烈输入您的路线定义

为了强类型化我们的角度路由定义，我们可以扩展上一节中讨论的基本类型。

值得探究的是“数据”属性的类型。默认为[，定义如下](https://angular.io/api/router/Data):

```
type [Data](https://angular.io/api/router/Data) = {     [name: string]: any; };
```

正如您所看到的，这是一个简单的记录(即字典),带有字符串键和作为值的*或其他*。为了改进这一点，我们可以简单地扩展这个基本类型。这里有一个例子:

如上所述，我用额外的属性扩展了 Angular 提供的默认类型；一个是强制性的。

通过使用这个接口而不是默认接口，我们已经可以从 TS 编译器获得更多的帮助。在我的项目中，我甚至没有扩展默认的`Data`类型。超级严格…或者不严格，就看你自己了。

现在，我们如何使用这个自定义界面？我们还必须扩展`Route`类型的角度:

我们快到了。这一次，我们用自己的属性替换了`Route`接口的默认`data`属性。接下来的确是`Routes`型:

```
export type CustomRoutes = CustomRoute[];
```

现在我们有了它，我们可以用它来定义根和子/惰性加载模块中的路由数组:

如您所见，这一切并不复杂，好处也相当明显:

*   明确的期望
*   标准化
*   防呆的

# 结论

在本文中，我快速回顾了角度路由器的主要数据类型，并向您展示了如何强类型化路由定义。

如果你花时间做这件事，那么无论何时你忘记了什么或者需要重构路由器配置，编译器都会帮你解决。

喜欢打字稿！；-)

今天到此为止！

*如果你想了解大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题，那么不要犹豫，去* [*拿一本我的书*](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL) *并订阅* [*我的简讯*](https://mailchi.mp/fb661753d54a/developassion-newsletter) *！*

## **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**