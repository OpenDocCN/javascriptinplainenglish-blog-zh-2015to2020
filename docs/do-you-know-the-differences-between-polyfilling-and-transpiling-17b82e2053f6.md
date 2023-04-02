# Polyfilling 和 Transpiling 有什么区别？

> 原文：<https://javascript.plainenglish.io/do-you-know-the-differences-between-polyfilling-and-transpiling-17b82e2053f6?source=collection_archive---------6----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/7a370f35b1091128f962252a9bf93531.png)

By undraw.com

今天，一个开始与我们合作的新开发人员问我什么是 Polyfill，它是否与 transpile 代码相同，因此为了回应这一点，我鼓励自己写这篇小文章。

一些最新的功能有时并非在所有浏览器中都可用。那么，我该如何使用所有这些新功能呢？我必须期待它们被实现吗？旧的浏览器会发生什么？

答案是否定的。有两种主要方法可以用来在浏览器中启用新的 JavaScript 特性:多填充和传输填充。

# 聚合填充

术语“polyfill”是指获取一个特性的定义，并提供一段等价的代码，以便在不支持它的旧浏览器上提供现代功能。

例如，考虑以下实用程序:Number.isNaN。

ES6 定义了这个实用程序，为 NaN 值提供比原来更好的检查。但是，如果您的浏览器不支持它(现在很少见)，您可以很容易地为这个实用程序创建一个 polyfill，并开始使用它，而不管您的浏览器是否支持它。

```
if (!Number.isNan){Number.isNaN = function isNaN(n){
   return n!== n; 
  }}
```

*if(！Number.isNan)检查防止在本身支持聚合填充的浏览器中应用聚合填充。*

聚合填充的问题在于，并非所有功能都是可聚合填充的，因此在实现它们时应该非常小心，因为如果聚合填充不完全符合规范，则使用聚合填充的所有代码都将是不正确的。

当您不能聚合填充一个特征时，您必须转换它。

# 运输文件

Transpiling(转换+编译)是将您的新代码转换为旧代码等价物的过程。

你不能用 polyfills 来“适应”新的语言语法；在这些情况下，最好的选择是使用转换代码的工具。您可以使用新的语法编写代码，然后使用 transpiler 将代码转换为旧的语言。但是，为什么要使用新的语法呢？

使用新语法编写总是更好，因为它使代码更易于维护和阅读。此外，当您使用该语言的 las 功能时，现代浏览器会利用浏览器优化。

有很多伟大的 transpilers，也许最著名的是[巴别塔](https://babeljs.io/)。

# 结论

polyfill 试图模拟特定的方法，因此您可以像浏览器(或节点引擎)已经支持它们一样使用它们，另一方面，transpiler 将修改您的代码，并用执行相同操作的其他代码替换代码，然后可以在旧的浏览器中执行。

因此，如果您的目标浏览器没有实现您需要使用的功能，您可以使用聚合填充。另一方面，transpiler 更复杂，它允许你使用新的语言语法和转换你的源代码。

感谢你阅读我！

如果你想对这个话题了解更多，想看实际例子，你可以读一读我前段时间写的一篇文章:

[*如何在任何浏览器中使用最新的 JavaScript 特性*](https://medium.com/javascript-in-plain-english/use-the-latest-javascript-features-in-any-browser-f047f5c426a8)