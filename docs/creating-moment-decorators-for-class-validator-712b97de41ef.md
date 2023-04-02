# 为类验证器创建时刻装饰器

> 原文：<https://javascript.plainenglish.io/creating-moment-decorators-for-class-validator-712b97de41ef?source=collection_archive---------5----------------------->

Moment.js 是即将被淘汰的，但是如果您的项目正在使用它，并且您缺少一两个验证器，您仍然可以扩展它。

![](img/0682c86758ecc94b86828d425e2f8dcf.png)

Picture courtesy of [NeONBRAND](https://unsplash.com/@neonbrand)

Class-validator 是一个非常流行的基于 decorators 的 JS/TS 验证库。它为[提供了许多内置的验证器](https://github.com/typestack/class-validator#validation-decorators)，但是也可以很容易地扩展。

正如介绍中所述，我知道 Moment.js 现在已经被弃用了(实际上已经弃用了相当长一段时间)，因为它存在长期的问题(膨胀、缺乏对树抖动的支持、可变性等)。我在本文中的目标不是讨论在您的项目中使用 Moment.js 是否是一个好主意；我想，到现在为止，答案已经很清楚了:不是。但事实是，我们并不总是有选择，如果有其他优先事项，我们不能现在就放弃它。

在本文中，我将解释为类验证器创建定制 Moment.js 验证器/定制装饰器是多么容易。为了展示这一点，我将分享我在当前项目中需要的`IsDuration`和`MinDuration`验证器的代码。顾名思义，这些验证器分别确保要检查的值是有效的 Moment duration 对象，或者是等于或大于配置的最小值的持续时间。

# 通用验证功能

首先，让我们定义一个简单的函数来检查给定值(字符串或 null)是否是有效的[持续时间](https://momentjs.com/docs/#/durations/)对象:

如您所见，这个函数非常简单。它接收假定为持续时间的值，并检查它是否真的是持续时间。关于这个逻辑的细节，请查看这个 [SO 问题](https://stackoverflow.com/questions/38692972/how-to-validate-a-moment-js-duration)，因为它解决了一些问题。

这个函数的测试很简单:

# IsDuration 验证程序

现在，让我们将函数包装成一个实际的力矩验证器:

`isDuration`函数是验证函数(重用了我们之前的函数)，而`IsDuration`函数是一个类型脚本属性装饰器。

如果你不知道，TS 装饰其实是..一个功能。所以这个函数接收标准的`ValidationOptions`配置对象 os Moment 并返回验证器的定义。当它调用 moment 的`ValidateBy`函数时，它定义了验证需要如何执行，以及如果验证失败，返回什么消息。

以下是这个验证器的测试:

使用这个验证器就像在要验证的属性中添加`@IsDuration()`一样简单。

# 智力验证器

下一个验证器扩展了前一个验证器，以确保持续时间值等于或大于某个最小值。

实现如下:

第二个验证器稍微复杂一点，因为它接受一个输入值，我们称之为`minDurationInMs`(以毫秒为单位的最小持续时间)。我们将该值作为参数传递给装饰函数，装饰函数将其作为约束传递给`ValidateBy` 函数，并使用`args?.constraints[0]`检索/传递给`minDuration`函数。

同样，这样的验证器很容易测试:

并且易于使用:

# 结论

在本文中，我已经向您展示了使用 TypeScript 和简单函数创建定制的类验证器验证装饰器是多么容易。

它们使用的是现在正式的 dead Moment.js 库，但是您可以为任何库或者您可能有的需求创建类似的验证器。

今天到此为止！

PS:如果你想学习大量关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的其他很酷的东西，那么不要犹豫[去拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！