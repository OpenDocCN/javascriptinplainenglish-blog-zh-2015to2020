# 角坐标中的白标解

> 原文：<https://javascript.plainenglish.io/a-white-label-solution-using-powerful-features-in-angular-7-517444f6fc9?source=collection_archive---------1----------------------->

## 它允许你分享，重复使用和重新标记 Angular 应用程序

![](img/c0e15353f6081dbb9425ec7fbba96c1f.png)

在本文中，我将介绍一个使用动态路由和延迟加载模块构建白标 Angular 应用的解决方案。

## 什么是白标 App？

白标应用是由一家公司制作的应用，并为其他公司重新贴牌，使产品看起来像他们自己的。

使用白标 App 会省时省力(省钱)。大多数时候，变化是外观和感觉的定制，功能是基于现有的应用程序提供的。

## 我想达到的目标

在这个例子中，我想开发一个基于现有的 Angular 应用程序的白标应用程序。

我的目标是:

1.  白色标签应用程序有自己的登录页面，具有定制的风格。
2.  它可以与原始应用程序中的现有功能相同，也可以是其子集
3.  它可以作为一个独立的应用程序来构建和部署，分布式包应该只包含必要的代码。
4.  最大化代码共享。

## Angular 应用

为了简单起见，示例 Angular 应用程序包含一个带有 home 组件的着陆模块和两个功能模块。

```
| — app
    | — Landing modules
           |-- [+] home components
           |-- default-landing-routing.module.ts
           |-- default-landing.module.ts
    | — feature1 modules
           |-- [+] view1 components
           |-- feature1-routing.module.ts
           |-- feature1-landing.module.ts
    | — feature2 modules
           |-- [+] view2 components
           |-- feature2-routing.module.ts
           |-- feature2-landing.module.ts
    | - app-routing.module.ts
    | - app.module.ts
    | - app-component.html
    | - app.component.ts
```

新的白色标签应用程序是为“`abc`”公司制作的，它有自己的带有`AbcHome`组件的`abc-landing`模块。

为了识别原始应用程序与白标应用程序，添加了一个新的环境文件，该文件具有新的“`orgId`”属性。

```
// environment.ts
export const environment = { production: false, orgId: ‘default’};// environment.abc.ts
export const environment = {  production: false,  orgId: 'abc'};
```

为了让 Angular 了解新的`abc` 环境，我们需要将以下配置添加到 Angular.json 中

```
// angular.json
configurations": {
    ...,
    "abc": {
        "fileReplacements": [
            {
                "replace": "src/environments/environment.ts",
                "with": "src/environments/environment.abc.ts"
            }
        ]
    }
}
```

配置完成后，我们可以通过下面的 ng build 命令为新环境构建一个`abc` 应用程序。

```
//package.json
...
  "start-abc": "ng serve -c abc",
...
   "build-abc": "ng build -c abc",
```

当构建用于生产时，dist 文件夹将包含带有`abc` 配置的构建输出。

## 动态路由

动态路由是这个解决方案的关键。

首先，我们为`abc` 公司添加新的登陆模块。

```
| — abc Landing modules
           |-- [+] abc home components
           |-- abc-landing-routing.module.ts
           |-- abc-landing.module.ts
```

然后，我们在 App.module.ts 中定义路由

```
*const routes: Routes = [
{ path: '', data: {name: 'default', roles: ['all']}, redirectTo: 'home', pathMatch: 'full' },
{ path: '****home****', data: { name: 'home',* ***roles****: ['default']},****loadChildren****: () => import('./default-landing/default-landing.module').then(m => m.DefaultLandingModule) },
{ path: '****home****', data: { name: 'home',* ***roles****: ['abc']},****loadChildren****: () => import('./abc-landing/abc-landing.module').then(m => m.AbcLandingModule) }
];*
```

在这里，我们有两条“回家”路线，一条用于 origin 应用程序，另一条用于新的`abc` 白标应用程序。两条“回家”路径是如何工作的？

我们使用 APP_INITIALIZER 令牌来挂钩角度引导过程。

```
providers: [{ provide: APP_INITIALIZER, useFactory: initApp, deps: [Injector], multi: true }]
```

在工厂函数“`initApp`”中，运行时通过环境设置“`orgId`”和先前定义的路线数据中的“`roles`”属性加载路线。

Init the dynamic routing

## 延迟加载模块

您可能已经注意到，default-landing 和`abc-landing`模块都是延迟加载模块，因为使用了“loadChildren”语法，后跟一个使用浏览器内置`import('...')`进行动态导入的函数。

好处是延迟加载模块被构建到单独的小块中，而不是一个单独的包文件中。尽管特定于环境的模块都是在编译时构建的，但只有在导航到路由器时，它们才会被下载到浏览器。例如，当我们运行`abc` 配置时，只有`abc-landing`模块会被加载，而默认着陆模块永远不会被调用，因为它不在路线中！

## 代码共享

由于所有的登陆模块都是“活”在一起的，它们可以根据需要访问/共享所有的代码。在本例中，白标应用程序在其子路线中使用功能 1 和功能 2 模块，以及默认应用程序。

由于白色标签应用程序有一个不同的“入口组件”:AbcHomeComponent，可以根据需要定制外观和感觉。它可以有自己的页眉/页脚组件和样式。我们还可以选择在白标 App 中使用或不使用哪些功能模块。

## 摘要

在本文中，一个完全可定制的白标应用程序建立在现有代码基础之上。我们只是增加了一个新的登陆舱和一些配置。通过传递`— c`参数，origin 应用和新的 White label 应用都可以构建为独立的应用，并部署在不同的 web 主机中。

希望这篇文章对任何打算构建多前端 Angular App 的人有所帮助。[源代码](https://github.com/sunnyy02/ngWhite/)可以在 GitHub 上获得。

如果你喜欢这篇文章，你也可以看看下面的文章。

[](/angular-state-management-with-observable-service-pattern-27b18538f4c3) [## 具有可观察服务模式的角度状态管理

### Angular 状态管理是任何 Angular 应用程序的核心，但没有放之四海而皆准的解决方案。

javascript.plainenglish.io](/angular-state-management-with-observable-service-pattern-27b18538f4c3)