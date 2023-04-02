# 为什么应该在 JavaScript 中开始使用 globalThis 属性？

> 原文：<https://javascript.plainenglish.io/why-should-you-start-using-globalthis-in-javascript-fe811151a8b?source=collection_archive---------4----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/93104f50231140fdfc2d5c6a3db98197.png)

Here is an image with a f[emale software engineer coding on a computer.](https://www.pexels.com/es-es/foto/mujer-oficina-trabajando-ordenador-3861963/)

我们在各种各样的环境中使用 JavaScript，比如 web 浏览器、Node.js 或 Web Workers。每个环境都有自己的对象模型，并提供不同的语法来访问全局对象。在网络浏览器中，可通过<window>、<self>或属性访问全局对象。在 Web Workers 中，只有<self>可用。在 Node.js 中，这些都不可用，你必须使用<global>。</global></self></self></window>

在浏览器中，您可以使用<window>对象:</window>

```
window.setTimeout(() => console.log('Hello Rick'),1000);
```

但是<window>在节点环境下不起作用。Node.js 中的顶级作用域不是全局作用域，但是您可以使用等效的<global>关键字来访问全局对象:</global></window>

```
global.setTimeout(() => console.log('Hello Rick'),1000);
```

问题是，如果我们编写需要在不同环境下运行的代码，首先要检测它是在哪个情境下运行的，然后选择如何使用它。尽管如此，不同环境的列表可能会很长，并且检查哪个正在运行的方法可能会很棘手并且容易让人误解。

解决这个问题的一个(错误的)方法是创建我们的 [polyfill](https://medium.com/javascript-in-plain-english/use-the-latest-javascript-features-in-any-browser-f047f5c426a8) 函数，负责返回适当的全局对象。

下面是一个获取全局 this 对象的示例:

```
const getGlobalThis = () => {if (typeof globalThis !== 'undefined') return globalThis;
  if (typeof self !== 'undefined') return self;
  if (typeof window !== 'undefined') return window;
  if (typeof global !== 'undefined') return global;//Note: this might return the wrong result.
  if (typeof this !== 'undefined') return this;
  throw new Error('Unable to locate global this object');};*//Note: We use <var> to ensure 'globalThis' becomes*
*//a global variable when running in the global scope.*var globalThis = getGlobalThis();
```

但是在其他问题中，这不能在非浏览器环境中的<strict mode="">函数或 JavaScript 模块中运行(不包括支持 globalThis 的浏览器)。此外，getGlobalThis 可能会返回错误的结果，因为它依赖于上下文相关的<this>对象，并且可能会被类似 [Webpack](https://webpack.js.org/) 的捆绑器更改。</this></strict>

这就是新的 globalThis 特性的用武之地。globalThis 提案在撰写本文时处于第 4 阶段，它引入了一种集中式机制来在任何 JavaScript 环境中访问全局对象。因此，如果需要访问全局对象，必须使用 globalThis 属性。

最近流行的浏览器都支持该功能，Node.js 12+也支持。

我们可以使用 globalThis 属性编写上面的代码:

```
globalThis.setTimeout(() => console.log('Hello Rick'), 1000);
```

## 结论

需要注意的是，我们不能使用独占的环境函数，例如 alert()函数。然而，这是一个实用的特性，可以帮助开发人员更容易地创建应用程序，并避免在不同环境中可能被忽略的错误。

我希望你喜欢这篇文章。非常感谢你阅读我！

## **用简单英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，请用你的 Medium 用户名发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。