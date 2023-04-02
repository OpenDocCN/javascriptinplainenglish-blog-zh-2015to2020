# 角度:依赖注入与静态方法

> 原文：<https://javascript.plainenglish.io/angular-dependency-injection-vs-static-methods-2191fc08e078?source=collection_archive---------0----------------------->

![](img/2001a7a460d5583a65f1eca10efe9420.png)

Photo by [Robert Anasch](https://unsplash.com/@diesektion?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在日常生活中，问题往往源于选择。我们经营一项活动的机会越多，就越难选择。这也发生在开发过程中，今天我们比以往任何时候都有许多工具可以让我们以不同的方式获得相同的结果。

今天，我们将快速解释何时以及为什么对静态类使用依赖注入。

## **依赖性注射液**

依赖注入(DI)是一种重要的应用程序设计模式。【Angular 有自己的 DI 框架。特别是一种编码模式，在这种模式下，一个类向外部源请求依赖，而不是自己创建依赖。

请记住，当我们谈论依赖关系时，我们指的是类执行其功能所需的服务或对象。

*提示:*

Angular 团队建议您在多个文件中分离组件和服务。

这就是为什么它可能令人困惑，因为如果您选择将所有内容压缩在一个文件中，如果您不想陷入运行时空引用错误，您必须在组件之前定义服务。

按照使用 Angular CLI 生成新的“MagicianService”类的命令，路径为“src/app/magic”。

```
ng generate service magic/magician
```

**为什么选择依赖注射？**

依赖注入框架不能做任何大多数开发人员还不熟悉的事情。

然而，主要目标是通过注入依赖关系而不是直接在组件中实例化依赖关系来避免紧密耦合的组件。
所以，分离服务和组件，最后一种更灵活，因为您可以将不同的实现注入到同一个组件中。

事实上，直接投资的一个非常有用的特性是令牌。

注入令牌是 Angular 的一个特性，它提供了一种机制来将令牌链接到一个值，并将该值注入到一个组件中。

**注意……**

Angular 开发人员的典型行为是在 AppModule(根模块)中记录所有的提供者。这很有用，因为通过这种方式，所有组件都可以访问服务的同一个实例，从而创建一个 Singleton。

> 服务是注射器范围内的单个项目

[文档](https://angular.io/guide/singleton-services)中的这句话是什么意思？

在根模块中记录所有提供程序会产生同步组件效果，即如果某个组件更改了服务中的某个属性，所有其他组件都可以访问该属性的相同值。

**但** …

当您在不同的子模块中两次实例化同一个服务时会发生什么？

在这种情况下，特定子模块下的所有组件将具有相同的服务和所有值属性。然而，这与注入到另一个子模块中的服务完全无关，从而失去了跨应用程序的同步效果，仅将其保持在子模块分支中。

## 为什么是静态方法类？

具体来说:

*   仅仅是一个功能吗？
*   它没有依赖关系吗？
*   你希望它是纯洁的吗？

在这些简单的情况下，您应该使用静态类。

**深陷**

与实例方法不同，类的静态方法在类本身上是可见的，它们不是类的实例。

通常，当我们谈论静态方法时，它从参数中获取输入，对其执行操作，并独立于您的应用程序返回结果。

我说的是 utils 函数，这是一个对所有应用程序都有用的会话服务，以及所有那些不关心执行情况的情况。

此外，静态方法对性能没有任何影响，调用它们也不会实例化该类。

看一下这个用法的例子

```
import { MagicianService } from '../services/magic/magician.service';export class PerformanceComponent { constructor() {
      // Note that I didn't inject the service or instantiated it like new MagicianService()
      const bunniesNumber = MagicianService.getBunniesNumber(1,5);

      console.log(bunniesNumber); // => 3 }
```

**你应该考虑的事情**

当我们把一个方法赋给类函数本身和它的“原型”时，静态方法中的 **this** 的值就是类构造函数本身。

让我们看一个例子

```
export class SessionService { static test() {
      alert(this === SessionService);
   }}
```

如果您调用“test”方法，警报将打印“true”。

```
SessionService.test(); // => true
```

## 结论

选择哪种方式是最好的总是很困难的。

我们应该一个案例一个案例地分析，但是作为一个主要的规则，建议如果一个方法变得越来越复杂并且依赖是必要的，那么你最好通过依赖注入来开始你的代码。

相反，如果你的函数保持简单，它不必知道你的业务逻辑的一部分，并且属于类而不是任何特定的对象，你应该使用静态方法和属性。

如果你喜欢这篇文章，请按👏想按多少次就按多少次。另外，如果你有任何问题，请随时提问。

非常感谢你的阅读！

*更多内容请看*[***plain English . io***](http://plainenglish.io/)