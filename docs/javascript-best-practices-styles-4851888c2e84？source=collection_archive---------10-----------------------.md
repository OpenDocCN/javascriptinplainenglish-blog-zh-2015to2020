# JavaScript 最佳实践—样式

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-styles-4851888c2e84?source=collection_archive---------10----------------------->

![](img/9df6783d02fe460ffbf364d27a98c6c0.png)

Photo by [Ryan Stone](https://unsplash.com/@rstone_design?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 坚持严格的编码风格

为了一致性，我们应该坚持一致的编码风格。

这将减少混乱，并使我们的代码在不同环境之间可移植。

# 使用严格模式

严格模式阻止我们做很多不应该用 javascript 代码做的事情，比如给值赋值，比如`undefined`或`null`。

同样，我们不能多个参数同名。

在严格模式下也不允许使用`with`语句。

`arguments`和`eval`也不能被修改。

这些都是严格模式带来的非常好的变化。

所以要经常用。

幸运的是，模块默认启用了严格模式。

# 使用林挺

我们应该用林挺的 ESLint。

它将帮助我们坚持严格的编码风格。

此外，它还可以用来检查错误和安全问题。

# 阅读代码风格指南

Airbnb、Google 和其他公司都有自己的风格指南。

还有 Standard.js 风格指南，我们可以遵循。

有 ESLint 插件可以将这些规则添加到我们的林挺规则中。

比如 Airbnb 就有一个。

# 避免全局变量

我们绝对应该避免使用全局变量。

它们可能会被任何东西覆盖，所以我们应该避免它们。

一个例外是，我们需要使用内置在像`window`或`document`这样的浏览器中的全局变量，这样我们才能使用它们。

然而，我们绝对不应该定义我们自己的。

所以我们不写:

```
window.x = 1;
x = 2;
```

# 优先访问局部变量

我们应该尽可能定义局部范围内的变量。

这对于像`let`或`const`这样的块范围关键字是可能的。

它们只在街区内可用，所以我们不会对它们在哪里可用有任何混淆。

我们可以通过书写来使用它们:

```
let x = 1;
const y = 2;
```

# 顶部的声明

如果我们声明变量，我们会把它们放在最上面，这样更容易找到。

这也减少了我们意外地再次声明它们的机会。

# 初始化变量

我们应该初始化变量，这样我们以后就不会忘记这样做。

例如，我们应该写:

```
let x = 1;
```

这不是`const`的问题，因为它总是用一个值来声明。

# 让标识符变得可理解

如果我们在创建变量、对象、函数等等。，我们应该使名字容易理解，这样我们就可以和他们一起工作。

例如，不写:

```
let a, b, c, asdf;
```

我们写下这样的名字:

```
let numApples, numOranges;
```

# 使用===比较

我们应该使用`===`进行比较，因为在比较它们之前，它不会将变量的类型转换为不同的类型。

`==`自动数据类型转换和规则是复杂的，所以我们应该避免。

同样，出于同样的原因，我们应该使用`!==`进行不平等比较。

不值得救一个角色。

![](img/23522014e3e088b5bd9b24fb05784862.png)

Photo by [Pascal Mauerhofer](https://unsplash.com/@pascaloutdoor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该使用严格的样式和`===`进行比较。

启用严格模式也是一个好主意。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**