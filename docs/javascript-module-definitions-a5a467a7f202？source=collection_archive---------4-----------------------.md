# JavaScript 模块定义

> 原文：<https://javascript.plainenglish.io/javascript-module-definitions-a5a467a7f202?source=collection_archive---------4----------------------->

![](img/c448215d7ff6b2934199ed9e1ff5f1b2.png)

Photo by [Rick Mason](https://unsplash.com/@egnaro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

一个 JavaScript 模块可以被视为一个乐高积木，它在底部有一些*管*和在顶部有一些*钉*，这个简单的契约可以很容易地将一些模块连接在一起。模块内部可能有任何东西，模块内部不影响其他模块内部。

这种使模块可以连接到其他模块的契约被称为**模块定义**。目前我们有很多不同的模块定义(IIFE，commonJS，ESM，AMD，UMD，esnext…)，所以如果我们想发布我们的代码，我们需要特别理解它们。

# **史前史**

当 JavaScript 出现在浏览器上时，它被添加了 *<脚本>* 标签，并且每个函数或变量都是全局的。这对于较小的项目来说很容易，但是重用不同团队创建的代码却很困难，因为一些函数或变量可能会在不同的库之间被无意地共享或覆盖。

# **生活**

没有像标准模块定义这样的东西，所以一些聪明的开发者开始使用**life**(立即调用函数表达式)

```
(function () {…})();
```

因为函数体的作用域是局部的，所以函数内部的变量和函数不再是全局的。

当代码必须对应用程序的其他部分可用时，我们仍然被迫添加至少一个全局变量。类似于:

```
*window.myLibrary = {…}* 
```

但至少是故意的，更有控制力。

# **常见的**

2009 年一些开发者开始在服务器中使用 JS，需要一个真正的模块系统。于是 **CJS** 模块诞生了

```
var path = require(‘path’);module.exports = …module.exports.NAMED = …
```

答。js 文件在默认情况下是一个模块，所以没有函数或变量是全局的。它们可以通过 *module.exports* 暴露给其他模块，模块可以通过 *require()* 使用其他模块。

CommonJS 仍然是节点应用程序的标准模块定义，但是浏览器不支持它。

为了在浏览器上使用 CJS，出现了 bundler 概念。Browserify 是一个命令行工具，用于分析中的 *require()* 。js 文件，并将所有相关模块连接成一个单独的。js 文件。

这有助于网络开发者使用模块。捆绑的 js 还有额外的优势:例如，它更快，因为浏览器中需要下载的文件更少。

# **AMD**

异步模块定义是一种不同的方法

```
define(name, [dependencies], function(dependencies) { …})
```

模块是通过调用定义函数来定义的，其中模块指定了它的依赖项。当模块加载器加载了所有的依赖项后，它开始执行模块。

*RequireJS* 是实现 AMD 的加载器。它是一个浏览器库，有一些优点，比如不需要命令行界面，因为一切都在浏览器中完成。

# **UMD**

模块的爆炸式增长给库开发者带来了额外的工作，因此 **UMD** 被发明出来。通用模块定义基本上是一个 *if* 语句，根据谁在使用库，它使模块作为 CJS 模块、AMD 模块或 IIFE global 可用。

# **ESM**

但是这些模块定义都不是 EcmaScript 标准，所以在 2015 年 ES6 附带了 **ESM** (EcmaScript 模块)

```
import { format } from “date-utils”;export …export default …
```

它有一个主要的优点，你可以从一个库中导入一个函数，所以捆绑器可以删除不需要的代码。

> 在这一点上，重要的是要注意到，如果你想创建一个包含一组模块的库，你必须让项目容易地使用你的一个或一些模块，而不强迫他们加载你的所有包。这是主要原因，因为 momentjs 正在被 date-fns 取代。

# **System.register**

现在大多数现代浏览器都支持 ES 模块。SystemJS 是一个运行在浏览器中的模块加载器，它支持额外的特性，如编译或加载非标准模块。它有自己的模块定义 **System.register** 和一套附加的相关工具，包括一个捆绑器。

```
System.register([dependencies], function(export) { export(‘parseDate’, function () { … });});
```

# 浏览器中的捆绑器与模块加载器

目前，在 web 应用程序中使用模块有两种方法:

*   使用捆扎机(Browserify、Webpack、Rollup、package…)
*   使用浏览器加载程序(RequireJS、SystemJS 或简单浏览器本机 ESM)。

现代浏览器可以原生使用 ESM，但没有 IE 版本支持它。

现代捆绑器可以读取大多数模块定义，也可以生成它们。

像 [unpkg](https://unpkg.com/) 或 [pika](https://www.pika.dev/) 这样的一些倡议正在推动直接在浏览器中使用 ESM。此外,[网络包装标准](https://github.com/WICG/webpackage)正在定义中。

[如今，使用捆绑器可以产生最佳性能，但是浏览器(以及 HTTP/2)正在发展，可以更快更好地加载 ESM](https://v8.dev/features/modules)。所以在不久的将来可能会有所不同。

# 结论

有这么多模块定义的原因是历史原因，现在我们有了 ESM，就没有理由再出现更多的模块定义了(请)。

NodeJS 本机支持 CJS，从节点 12 开始，它还支持带有实验标志的 ESM。

如果你正在编写一个 JavaScript 库，你应该支持:

*   CJS 如果代码可以在节点运行
*   ESM，永远
*   AMD 和 global IIFE 支持没有捆绑器的旧应用程序

> 如果您想要支持节点应用程序、新的 web 应用程序和旧的 web 应用程序，我会考虑生成 UMD + ESM 格式