# 不使用 TypeScript 的 7 个非常好的理由

> 原文：<https://javascript.plainenglish.io/7-really-good-reasons-not-to-use-typescript-166af597c466?source=collection_archive---------0----------------------->

![](img/aaae4c3c35216b559fd1a8021407e9ef.png)

人人都爱 TypeScript。它“解决”了 JS 的许多问题，它是 JS 的“超集”,它会使你的代码不容易出错，读起来也很舒服。有很多很好的理由使用 TypeScript，但是我会给你 7 个非常好的理由不使用。

# 这很危险

哇哦。如果 TypeScript 添加类型定义并在编译时检查它们，这有什么风险呢？以及 IDE 集成将警告您任何类型不匹配？正因为如此。TypeScript 将*仅*在编译时检查类型，并且*仅*检查可用的类型。任何网络调用、系统库、特定于平台的 API 和非类型化的第三方库都没有办法与 TypeScript 通信。当你习惯于检查你的类型，而不需要完全理解代码和平台时，错误和缺陷就会显现出来。

使用 JS，您不需要对类型做任何假设，您只需检查变量的*具体值*以确保它是您所期望的。或者，如果您不关心它的类型，在这种特殊的情况下，你不关心。在 TS 中，你依靠编译器帮你做，但它只能检查这么多。你可以把这两种方法结合起来，但这有什么意义呢？如果您将花时间编写定义，然后花时间编写代码来确保这些定义在运行时得到维护，那么为什么首先要有它们呢？

# 很乱

另一个悖论是:本应给代码库带来清晰和可读性的语言反而模糊了代码库。为了说明我的意思，请查看我在流行的开源库中找到的一些例子:

这一行来自 Redux 库，这 4 行所做的就是将`nextReducer`赋值给`currentReducer`。

下一个例子来自 RxJS 库。我不知道你怎么想，但是如果我不得不与一个应该帮助我的工具斗争，我不认为这是一个好工具。

# 这并不能解决问题

据说 TypeScript 解决了 JavaScript 的问题。但事实并非如此。动态类型在 JavaScript 中从来都不是问题，但是许多其他问题，比如`NaN === NaN`为假、分号可选或不可选、换行符将对象定义改变为作用域、代替 OOP 的语法糖，确实是问题。TypeScript 没有解决这些问题，而是引入了另一个标准，进一步分化了 JS 社区。

即使假设 JS 中缺少类型是一个问题，TS 也没有解决这个问题。你知道是什么吗？Java，C，C#等*编译的*语言。它们可以安全地保证编译时和运行时的强类型。解释语言不具备这种能力。

# 它不是超集，而是子集

TypeScript 是将*编译成* JavaScript 的东西，根据定义它不能是超集。它限制了您可以用 JavaScript 做什么，掩盖了它的优点，同时提供了一种虚假的安心。如果你真的想成为一名伟大的开发者，不要满足于一个令人欣慰的谎言，试着去理解 JavaScript 的真正力量和它的灵活性。

# 它是开源的，但仅此而已

使用 TypeScript 的许多理由表明它是*开源的*。没错，TS 编译器是在 MIT 许可下发布的。但是它仍然被微软这个垄断巨头所控制，它的开源进步只不过是一种营销手段。不要把开源和民主混为一谈:微软仍然可以对 TS 做任何你想做的事情，而你只是在这里看着。另一方面，JS 是由一个国际委员会管理的，没有社区的同意，它不会做任何改变。

# 但是大公司用它…

我不能相信有些人认为这是一个理由。大公司也使用遗留代码库，进行税务欺诈，歧视女性。为什么突然他们使用 TypeScript 是一个很好的例子？

# 但是它有更多的功能…

不再是了。没错，当 TS 在 2012 年第一次被[引入时，它有类这样的特性，在 JS 中仍然没有。但从那以后，JS 已经走过了很长的路，现在 TS 正在努力跟上。JS 缺什么，有巴别塔插件来做。](https://en.wikipedia.org/wiki/TypeScript)

感谢您的阅读，希望您喜欢这篇文章！关于 JavaScript 高级概念的更多信息，请查看我的其他帖子:

[](https://medium.com/javascript-in-plain-english/to-infinity-and-beyond-with-javascript-proxy-api-8d4f7a26c8dc) [## 使用 JavaScript 代理 API 无限超越

### 代理 API 是一个高级的概念，但是如果你想掌握 JavaScript，代理 API 是你绝对需要的…

medium.com](https://medium.com/javascript-in-plain-english/to-infinity-and-beyond-with-javascript-proxy-api-8d4f7a26c8dc) [](https://medium.com/javascript-in-plain-english/implementing-the-builder-pattern-in-javascript-without-classes-eaf41f93b9c0) [## 在没有类的情况下用 JavaScript 实现构建器模式

### 在 JavaScript 中使用高级设计模式的力量，而没有类的开销

medium.com](https://medium.com/javascript-in-plain-english/implementing-the-builder-pattern-in-javascript-without-classes-eaf41f93b9c0) [](https://medium.com/javascript-in-plain-english/deep-dive-into-es6-symbols-3b44f4ba7eb3) [## 深入探究 ES6 符号

### 在这篇文章中，我将解释什么是 ES6 符号，为什么你可能想要使用它们，以及它们是如何工作的。

medium.com](https://medium.com/javascript-in-plain-english/deep-dive-into-es6-symbols-3b44f4ba7eb3) [![](img/446049aa060bbaea15a64e1a907b1030.png)](http://eepurl.com/gYiA29)

## 简单英语的 JavaScript

通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**