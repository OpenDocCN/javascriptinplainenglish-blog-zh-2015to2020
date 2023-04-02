# 有角度的惰性装载:什么，如何，为什么

> 原文：<https://javascript.plainenglish.io/angular-lazy-loading-the-what-how-and-why-7bbf8979cbd?source=collection_archive---------6----------------------->

![](img/23786a79c9f1b54fbf14db8941a4cdb4.png)

Photo by [Martina Misar-Tummeltshammer](https://unsplash.com/@tiniiiii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/cat-yawning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着任何前端应用程序规模的增长，代码的组织和架构变得更加重要。对于一个只有一两个页面的简单应用程序，如果没有太多的业务逻辑，我们可以在没有太多负面影响的情况下摆脱糟糕的实践。在更大的范围内，这些影响会被放大，并对您的应用产生重大影响。Angular 中的惰性加载是您的工具箱中的一个工具，您可以使用它来创建一个随着应用程序的增长而轻松伸缩的应用程序。

# 为什么要加懒加载？

当您编译 Angular 应用程序时，Angular 编译器会创建一个 javascript 包，用于在客户机上运行您的应用程序。这些包包括您编写的所有转换的 javascript、HTML 和 CSS。随着应用程序中功能数量的增加，代码数量也会增加。这导致最终提供给最终用户的包的大小很大。大多数普通用户不知道一堆代码是什么，更不关心它有多大，但是他们会关心你的应用程序加载需要多长时间。javascript 包的大小与页面加载时间直接相关。较小的包、较快的页面加载和较大的包导致较长的页面加载。

为什么您应该关心页面加载需要多长时间？研究表明，大多数人等待页面加载的时间不会超过 5 秒，有些人甚至会等得更短。如果你的应用程序加载缓慢，那么你的客户就不太可能停留在你的网站上，这就意味着更少的转化。

# 什么是懒装？

所以现在我让你相信了。您的应用程序需要延迟加载。只有一个问题，你仍然不知道什么是懒加载，所以让我们来解决这个问题。

如果没有延迟加载，Angular 应用程序将作为一个主 javascript 包，其中包含您编写的所有代码以及它所依赖的任何导入代码。当您导航到您的应用程序时，服务器会将整个包提供给客户端，这取决于其大小和客户端的互联网连接速度，可能需要一点时间来加载。

惰性加载允许您将 javascript 包分割成更小的块，只有当用户需要相关代码时，才会增量加载。例如，假设我有一个正在构建的管理餐馆的应用程序。它有两个主要功能:员工管理和库存。该应用程序的登录页面是一个仪表板，显示了库存和人员的摘要，并链接到库存和人员管理的相应页面。

当有人第一次加载页面时，他们不需要特定于库存管理或员工管理的代码。他们需要的只是允许他们查看仪表板的代码。如果他们要单击一个链接，将他们导航到应用程序的 staff management 部分，那么应该会检索到该代码，但是仍然不需要加载特定于库存管理的代码。

通过只加载最初访问页面时所需的代码，我们可以显著加快页面加载速度，进而改善最终用户体验。

# 让我们试一试

到目前为止，我已经告诉你什么是延迟加载，为什么它很重要，但我仍然没有解释如何实现它。我将使用我前面谈到的餐馆管理应用程序来演示实现。示例应用程序不会是一个完全构建的应用程序，但足以向您展示如何添加延迟加载。

首先，我将使用 Angular CLI 创建一个新的 Angular 应用程序。

```
➜ ng new lazy-loading-tutorial && cd lazy-loading-tutorial
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? SCSS [ http://sass-lang.com/documentation/file.SASS_REFERENCE.html#syntax ]
```

我选择将路由添加到应用程序中，这样它会自动创建一个路由模块，我使用的是 SCSS。如果您正在学习，请随意使用您喜欢的样式表格式。

接下来，我将添加三个模块来组织我的应用程序:仪表板、人员和库存。然后服务应用程序以在浏览器中查看它。

```
➜ ng generate module Dashboard ➜ ng generate module Staff --routing ➜ ng generate module Inventory --routing
➜ ng serve
```

我用路由创建了 Staff 和 Inventory 模块，以便在目录中创建一个路由模块。仪表板模块是在第一次访问网站时加载的，因此不需要添加路线和延迟加载。

让我们创建我们的仪表板组件，并将其连接到应用程序路由，以便当有人访问我们的应用程序的根目录时加载它。

```
➜ ng generate component Dashboard --module=Dashboard
```

我向仪表板模块添加了一个组件，并修改了`app-routing.module.ts`文件中的 routes 数组，如下所示。

这里我们设置了一个路由，这样我们的应用程序的根路径被重定向到`/dashboard`路由，它将加载我们的 DashboardComponent。在 dashboard 组件中，我们将显示餐厅的状态摘要，其中包括当前员工和低库存。出于本教程的考虑，我将只向仪表板添加两个链接。一个将我们引向员工管理(`/staff`)，另一个将我们引向库存管理(`/inventory`)。

接下来，让我们为延迟加载设置人员管理模块。我将向 Staff 模块添加三个组件:StaffList、StaffDetail 和 Report，并在`staff-routing.module.ts`中为每个组件配置路由。完成后，这是我在`staff-routing.module.ts`文件中的 routes 数组:

这是`app-routing.module.ts`文件中的 routes 数组:

文件`staff-routing.module.ts`中的路径看起来与我们通常在没有延迟加载的应用程序中看到的没有任何不同。然而，我们添加到`app-routing.module.ts` routes 数组中的路由有很大的不同。这里我们使用了`loadChildren`属性和一个包含动态导入语句的函数来告诉 Angular 这个 URL 路径的子路径在哪里。

如果您正在阅读本文，并且有一个前 Angular 8 应用程序，则它看起来应该是这样的:

这个语法在 Angular 8 中已经被否决了，所以你应该留意你正在使用的版本，以便知道应该使用哪个版本。

至此，应用程序的人员管理端已经使用延迟加载实现了。如果您打开 network 选项卡并导航到`/staff`，您应该会看到在初始页面加载时加载主块之后加载了一个单独的 javascript 块。

继续自己连接应用程序的库存管理部分。如果您需要帮助，请在此参考已完成的练习[。应该有两个组件:一个 InventoryList 和 OrderInventoryItems 组件。InventoryList 将显示餐馆中所有库存的列表，但是我们的将只包含一个到 OrderInventoryItems 组件的链接。不要忘记为新的惰性加载模块更新`app-routing.module.ts`文件。](https://github.com/Full-Stack-HQ/lazy-loading-tutorial)

当您完成这些后，在路径之间导航时，查看一下 dev tools 控制台中的 network 选项卡。您应该会看到不同的块被分别加载。

# 保护模块负载

对于某些延迟加载的模块，如果最终用户没有查看它们的权限，可能有必要阻止它们被加载。也许应用程序的这一部分非常敏感，所以我们甚至不希望未经允许查看内容的人看到源代码。这可以通过创建一个实现`CanLoad`接口的服务来实现。

看起来是这样的:

我们在这里可以看到，我们只希望允许管理员用户能够管理我们餐厅的库存。我们还想确保他们看不到任何安全的业务逻辑，如果这个包被加载，就会发生这种情况。如果您在一个真实的应用程序中这样做，您会想要添加必要的逻辑来确定这个人是否是管理员，而不是总是返回`true`。

# 预加载惰性加载的模块

既然我们已经设置了延迟加载，我们的一些模块将只在导航到特定的路线时才被加载。这意味着在导航到该路径时可能会有一点延迟，因为它首先必须加载模块，然后它将执行您为正在显示的页面设置的任何启动逻辑。根据您的网络速度、模块的大小以及您拥有的启动逻辑的多少，页面可能会比没有实现延迟加载时花费的时间长一些。延迟加载的目标是加速初始页面加载，但我们不想牺牲应用程序其余部分的速度和用户体验。输入，预加载。预加载让我们获得了延迟加载的所有好处，同时也确保了用户体验没有差距。

我们可以配置角度路由器来预加载我们希望应用程序立即可用的任何模块。首先，角度加载我们需要加载应用程序的登录页面的主包。正常情况下，不会加载其他包，但是在主包加载后，Angular 会异步加载我们标记为在后台预加载的包。

预加载工作基于所提供的预加载策略。默认情况下，只有两种策略:不预加载任何模块，或者预加载所有延迟加载的模块。我们的应用程序已经没有预加载任何模块，但让我们继续配置它。对您的批准模块进行以下更改:

在这里，我们指定在预加载模块时，我们希望使用 PreloadAllModules 策略。当你进入浏览器时，你应该会看到我们的一个惰性加载模块和主模块一样被加载了。现在你们中精明的人可能想知道为什么只有一个懒惰加载的模块被预加载，而我刚刚说了所有的模块都应该被预加载。这是因为我们在上一节中配置了路由保护。我们的 CanLoad 保护优先于我们指定的预加载策略。

如果我们只想预加载我们已经配置为惰性加载的模块的子集，我们可以创建一个定制的预加载策略。我不会解释如何做到这一点，因为有棱角的医生有一个很好的解释[在这里](https://angular.io/guide/router#custom-preloading-strategy)。

# 延迟加载实现的常见问题

1.错误:BrowserModule 已经加载。

如果您是从零开始，这个错误是不应该看到的，但是如果您正在重构一个现有的项目来使用延迟加载，它更有可能发生。许多项目不是从一开始就考虑到延迟加载(或者对 NgModules 的理解)而构建的，因此很难进行重构来使用延迟加载。它们最终形成了一个依赖关系的网络，不需要的模块被导入，这反过来又带来了其他模块，等等。很难跟踪一个模块工作需要什么，不需要什么。

如果您看到此错误，请务必检查任何延迟加载的模块及其依赖项，以查看它们是导入 BrowserModule 还是 BrowserAnimationsModule。这两个都应该只在应用程序中导入一次，最好是在应用程序模块中。如果您有一个想要延迟加载的模块，并且它当前已经导入了 BrowserModule，您应该尝试导入 CommonModule。CommonModule 提供了您可能需要的功能，如`ngIf`和`ngFor`。BrowserModule 重新导出 CommonModule，因此如果您删除 BrowserModule，您很可能需要将 CommonModule 添加到您的 imports 数组中。

2.提供商的问题

当向应用程序添加延迟加载时，理解 Angular 中的依赖注入是如何工作的非常重要。关于这个主题的有棱角的[文件](https://angular.io/guide/dependency-injection)是一个极好的资源。如果拦截器、服务和其他可注入类在多个模块中提供，它们的行为有时可能与您期望的不同。对于延迟加载的模块来说尤其如此。

# 总结

到目前为止，您应该对什么是延迟加载以及如何将它添加到您的应用程序中有所了解。这是任何角度应用的重要组成部分，因为它提供了更好的用户体验。我还发现它允许更好的代码组织，这增强了开发人员的体验。这真的是双赢。

# 资源

1.  [教程源代码](https://github.com/Full-Stack-HQ/lazy-loading-tutorial)
2.  [惰性装载的角度文件](https://angular.io/guide/router#milestone-6-asynchronous-routing)

*原载于 2019 年 11 月 11 日*[*https://fullstackhq . io*](https://fullstackhq.io/angular-lazy-loading/)*。*