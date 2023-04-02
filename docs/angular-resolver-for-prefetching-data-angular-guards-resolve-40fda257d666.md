# 用于预取数据的角度解析器

> 原文：<https://javascript.plainenglish.io/angular-resolver-for-prefetching-data-angular-guards-resolve-40fda257d666?source=collection_archive---------0----------------------->

## 使用角度解析器预取数据

![](img/b000c884b1d650e29ee4c932ccb43d50.png)

Guard Types in Angular, Angular Resolve Guard

# 什么是角旋变器？

当用户从一条路线重定向到另一条路线时，使用角度解析器来预取一些数据。新的可用页面将已经具有需要在页面中呈现的数据。

在某些场景中，我们可能需要为要显示的组件预加载数据，以便在初始呈现时组件本身包含数据，而不是先显示空组件，然后查询 API 来提取数据。

# 为什么我们在角路由中使用旋变器？

![](img/e46db0bbf419c56e5cf243e31106c0c5.png)

使用角度解析器的常见用例场景是，正在加载的路由正在寻找一些异步数据。异步数据需要显示在组件中。

当我们对要呈现的组件的异步数据有一些需求时，有两种方法。

1.  **在组件加载后加载数据**:在第一种方法中，我们最初可以不使用任何数据来呈现组件，一旦组件被加载，我们可以调用 Ajax 来提取组件数据。然后组件被重新呈现以显示 Ajax 调用中的可用数据。这类似于最初呈现一个空白模板，然后向该模板添加数据。组件最初呈现后，数据可能会在短暂延迟后可用。
2.  **在组件加载之前加载数据**:另一种方法是在呈现组件之前准备好数据。如果需要从异步调用中提取数据，组件将在第一次呈现组件之前首先等待数据可用。这样，我们就不会渲染空模板了。在组件的初始渲染之前，路由器首先等待数据可用。

# 如何创建角度解析器服务

![](img/25ec5803bc4f400727e4c3c35e131ba3.png)

How to create Angular Resolve Service

为了实现解析器服务，我们需要:

1.  我们需要从“ **@angular/router** ”导入解析接口
2.  创建一个实现这个**“解析”接口**的服务类
3.  **覆盖“resolve()”函数**来指定我们需要等待的 HTTP 请求。这个函数可能返回一个承诺或可观察的
4.  将该服务类添加到**“根”模块**。
5.  将“ **ActivatedRouteSnapshot** ”注入到被加载的组件中，以使用“resolve”函数中指定的 AJAX 请求中的可用数据

# 实现解析服务

![](img/6c5d3aeeb2a1cd369df9fb91f30780ce.png)

Implementing Resolver Service in Angular

让我们创建一个" **DataService** "来提取"**employeelistcomponent**"的数据。该服务将从一个简单的 API 调用中获取公司的员工列表。下面是实现…

让我们从实现服务开始，该服务将在组件加载到屏幕之前解析需要对组件可用的数据。

[https://gist.github.com/Mayankgupta688/67fca46dbbfc47c9289c45dd6be41f1f](https://gist.github.com/Mayankgupta688/67fca46dbbfc47c9289c45dd6be41f1f)

在上面的代码中，我们创建了一个“data service”**来实现“Resolve”接口**。因为我们正在实现接口，所以我们需要**定义一个“解析”函数**，它可以**从该函数返回一个承诺/可观察或对象数据**。现在，我们可以使用这个服务在组件加载到应用程序页面之前预取数据。

# 在角度模块中实施路线

![](img/2bd74a94dd489c2d6a6e1fca7ec76d79.png)

Adding routs to Angular Module

一旦我们创建了服务，我们需要配置我们的路由来指定需要为每个组件预取的数据。让我们看看，我们如何修改路线…

[https://gist.github.com/Mayankgupta688/64c5f3e7f7ff49cbbd6fa97f2234bc01](https://gist.github.com/Mayankgupta688/64c5f3e7f7ff49cbbd6fa97f2234bc01)

上面的代码定义了应用程序的路由。当我们为应用程序定义路由时，我们提供一个键“resolve ”,它包含一个用于提取数据的对象。“Resolve”键被分配给一个以“employee”为键、以“DataService”为值的对象。

**“data service”的“resolve”函数被调用**，在加载组件之前，首先**解析返回的承诺/可观察值**。从 API 中提取的数据被分配给对象“**雇员**”。可以使用下面的代码在组件内部提取数据。

# 订阅解析的数据

[https://gist.github.com/Mayankgupta688/66b3059fb0352b2f29088315a368c2f5](https://gist.github.com/Mayankgupta688/66b3059fb0352b2f29088315a368c2f5)

为了在应用程序中获得解析后的数据，我们需要将“ **ActivatedRoute** ”注入到构造函数中。因为“数据服务”解析函数正在返回可观察值。我们可以**订阅“这个。_routes.data"** ，这将解析并向我们返回上述指定路线的“解析”键中指定的所有数据。

上述代码将从“DataService”类中指定的 API 返回雇员列表。一旦数据被订阅，雇员列表将在“雇员”键中可用。

工作代码如下所示:

[https://codesandbox.io/s/angular-resolve-routing-qoh9h?file=/src/app/app.module.ts](https://codesandbox.io/s/angular-resolve-routing-qoh9h?file=/src/app/app.module.ts)