# 理解根和孩子角度

> 原文：<https://javascript.plainenglish.io/understand-angulars-forroot-and-forchild-f27fbc41cb7b?source=collection_archive---------1----------------------->

![](img/84fd9e46ed376c406830d291cdbbc2bc.png)

`forRoot` / `forChild`是一种我们大多数人从路由中知道的单体服务模式。路由实际上是它的主要用例，由于它在 it 之外并不常用，如果大多数 Angular 开发人员没有重新考虑它，我不会感到惊讶。然而，正如官方 Angular 文档所说:

*“理解* `*forRoot()*` *如何工作以确保一个服务是单例的，将在更深的层次上通知您的开发。”*

所以我们走吧。

# 供应商和注射器

Angular 带有依赖注入(DI)机制。当一个组件依赖于一个服务时，您不需要手动创建服务的实例。您*注入*服务，依赖注入系统负责提供实例。

依赖注入并不局限于服务。你可以用它来注入(几乎)任何你喜欢的东西，例如像`RouterModule`中的`Routes`这样的物体。

注入器负责创建要注入的对象，并将它们注入到请求它们的组件中。您告诉注入器**如何通过声明*提供者*来创建这些对象。例如，在提供者中，你可以告诉注入器使用一个给定值或者使用一个类来实例化。**

注入的对象在一个注入器中总是单一的，但是在您的项目中可以有多个注入器。它们由 Angular 创建:在引导过程中创建一个`root`注入器，为组件、管道或指令创建注入器。每个惰性加载的模块也有自己的。

您可能需要给定服务在不同模块或组件中的不同实例。对于其他一些应用程序来说，如果在给定时间应用程序中存在多个实例，这可能真的无关紧要，除了对性能而言。然而，对于某些服务，你需要确保它们是真正的单例，这意味着在整个应用程序中只有一个实例。

服务的提供者通常是服务类本身，您通常会使用`providedIn` 快捷方式在`root`注入器中提供服务。

当提供其他类型的对象时，您可能会遇到必须在模块中声明提供者的情况，例如:

在这种情况下，当处理懒惰加载的模块时，保持`SOME_OBJECT`单例变得棘手。

# 惰性加载模块

当您在相互导入的预先加载的模块中提供值时，模块的提供者将被合并。我们可以从服务中看到这一点，因为我们可以记录实例的数量:

如果您在两个模块中的一个`providers`数组中提供该服务，并在另一个模块中导入其中一个模块，那么注入器将被合并，您将仍然只有该服务的一个实例。

您现在可以检查控制台，您会看到两次消息“有 1 个服务实例”。

对于延迟加载的模块，这变得更加复杂。每个延迟加载的模块都有自己的注入器。在前面的例子中，如果您延迟加载 moduleA 而不是简单地导入它，它的注入器将创建一个新的`SingletonService`实例。您将在控制台中看到“有 2 个服务实例”。

# 为根/为孩子

Angular 支持用提供者导入模块的另一种方式。你可以传递一个实现`ModuleWithProviders`接口的对象，而不是传递模块类引用。

```
interface ModuleWithProviders {
  ngModule: Type<any>;
  providers?: Provider[];
}
```

例如，您可以决定在`AppModule`和子模块中使用不同的提供者来导入模块:

一个更优雅的解决方案是在`ModuleA`上定义静态方法:

并在导入模块时使用这些方法。我们将它们命名为`forRoot`和`forChild`，但从技术上讲，我们可以自由选择任何名称。

# 按指定路线发送

在路由的情况下，`RouterModule`提供`Router`服务。如果没有`forRoot` / `forChild`，每个特征模块将创建一个新的`Router`实例，但只能有一个`Router`。通过使用`forRoot`方法，根应用模块得到一个`Router`，所有的功能模块都使用`forChild`并且不实例化另一个`Router`。

因为`forRoot`和`forChild`只是方法，所以你可以在调用它们时传递参数。对于`RouterModule`,您传递一个附加提供者的值、路由和一些选项:

```
static forRoot(routes: Routes, config?: ExtraOptions) {
  return {
    ngModule: RouterModule,
    providers: [
     {provide: ROUTES, multi: true, useValue: routes},
     ...,
    ],
  ...
}static forChild(routes: Routes) {
  return {
    ngModule: RouterModule,
    providers: [
     {provide: ROUTES, multi: true, useValue: routes},
     ...,
    ],
  ...
}
```

综上所述，`forRoot` / `forChild`解决了一个在真正特殊的情况下可能发生的问题。

惰性加载的模块有自己的注入器，当试图保持一些提供的服务为单例时，这会导致问题。

您可以通过使用`ModuleWithProviders`接口导入模块来解决这个问题。`forRoot` / `forChild`只是一个方便的图案，以一种干净的方式来包装这个。从技术上来说，它不是 Angular 的一部分，但它是 Angular 团队为`RouterModule`选择的解决方案，并且使用相同的模式解决类似的问题是一个很好的实践。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**