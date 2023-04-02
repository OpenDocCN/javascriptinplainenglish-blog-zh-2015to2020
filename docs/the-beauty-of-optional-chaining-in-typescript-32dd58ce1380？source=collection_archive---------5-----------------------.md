# 可选链接操作符(？。)在打字稿中

> 原文：<https://javascript.plainenglish.io/the-beauty-of-optional-chaining-in-typescript-32dd58ce1380?source=collection_archive---------5----------------------->

## 类型脚本重构

## 可选链接提供了一种重构我们的 TypeScript 和 JavaScript 代码的新方法。TypeScript 3.7 呈现猫王(？。)

![](img/bf5e6f700d9e4815e886df944dbe8155.png)

Photo by [Michèle Eckert](https://unsplash.com/@shelly94?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 可选的链接操作符支持除 IE、Firefox for Android、Opera for Android 和 Samsung Browser 之外的所有流行浏览器。

# 猫王接线员？。(也称为可选链接)

## 更简洁代码的简化 JavaScript 符号

JavaScript 发展很快，因此， **TypeScript 也随之发展很快**以我们编写 JavaScript 的方式推动新的特性和创新。

所有这些都是为了提高开发人员的生产力和改善开发人员的体验。

## 输入 TypeScript 3.7…

TypeScript 3.7 给了我们可选的链接操作符(`?.`),使得**能够缩短我们原本冗长而复杂的代码**。一旦开始使用这个操作符，重构的可能性就变得显而易见了。

学会了 Swift，它一直是我认为 TypeScript 中缺失的一个功能。

**我总是开始写它，意识到我在用错误的语言思考，然后在开始写包含多个`&&`、`!==`和`?`的更令人畏惧的版本时，想知道为什么它不是 JavaScript** 的一部分

我下意识地写可选链接的原因是**它只是对我讲常识。过去，必须用 TypeScript 编写替代方案是冗长、耗时和重复的。直到它最终被实现。我从未回头。**

> TypeScript 的伟大之处在于，您总是可以使用最新的特性，因为您最终只是转换成了标准的 JavaScript。

## 背后的逻辑是什么？

> 当使用点(。)符号来访问属性/方法，如果引用(对象)是 [nullish](https://developer.mozilla.org/en-US/docs/Glossary/nullish) ( `[null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null)`或`[undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)`)，表达式会短路，返回未定义的`.`

**它让我的代码更加……**

*   简单的
*   易读的
*   直觉的
*   简明的

## 为什么写…

```
const val = otherVal !== null && otherVal !== undefined && otherVal.prop !== null && otherVal.prop !== undefined && otherVal.prop.name;
```

## 当你能写的时候…

```
const val = otherVal?.prop?.name;
```

> 可选的链接简化了不必要的复杂性。

# 使用可选链接，您可以问一个问题

可选的链接操作符让我们能够编写高效、干净的代码，包括`null`或`undefined`检查，能够在执行任何操作之前安静地失败，同时不会分散对*快乐路径*的注意力。

当你在代码中写`?.`时，你实际上是在问(**在操作一个变量之前)** …

> “如果允许的话…”

哪个**更准确地说**是指…

> “如果你不为空或未定义…否则就当我没问，然后返回 undefined "

所以用上面的例子:

```
const val = otherVal?.prop?.name;
```

你是在用这段代码说“如果`otherVal`及其`prop`属性都**不为空**或**未定义，**请获取`name`属性，否则返回`undefined`”。

> 简单可选链接表达式中的**或**值，在无效检查失败时触发，总是**`**undefined**`。**

# **我什么时候应该使用可选链接？**

**在编写新的类型脚本代码时，有几种不同的情况可以考虑使用操作符:**

*   **当你想**访问**一个嵌套值时，只有当它存在时(不是 nullish)。**
*   **当你想**执行**一个函数时，只有当它存在时(不是 nullish)。**

> **在可选链接之前面对这些情况时，您可能已经用三元运算符编写了长格式的 nullish 检查。**

**因此，在重构现有的 TypeScript 代码时，使用可选链接的主要思想是用简化的`?.`替换长格式的`null`和`undefined`检查。**

## **可选链接是一个消音器**

**底层逻辑围绕**设计，以防止未捕获的错误，或者**通过在某种意义上处理它们，通过支持`undefined`值，而不是抛出错误，这可能在运行时表现为*未捕获的*。**

**所以你可以把操作者的行为看作是**消除**对变量操作的失败尝试，让应用程序照常运行。所以你应该意识到你可能在处理一个`undefined`值。**

**话虽如此，可选链接的行为可能会使您陷入困境，因为在处理`undefined`值时需要权衡实现的方法。**

## **权衡——显式还是隐式错误处理**

**有了可选链接，当使用三元运算符时，您可以用更可选的错误处理替换更显式的错误处理方法，其中 or 条件是强制的。你可以说错误是用`undefined`处理的，但这不是严格地处理错误，只是忽略它。**

# **如何用(？。)**

****深度对象访问—** 访问嵌套对象属性**

```
// making sure **employee**, and it's **address** isn't nullish before accessing postCodeemployee?.address?.postCode
```

****函数调用—** 如果对象/函数存在，则执行实例方法**

```
// making sure **employee** isn't nullish before executing the functionemployee?.sayHowCanIHelp();// making sure the function exists before executing itemployee?.sayHowCanIHelp?.();
```

****索引—** 防止无效的对象/数组索引**

```
// making sure **employees** and **employees[0]** isn't nullishcompany.employees?.[0]?.name
```

****简化映射—** 防止(先前)不可避免的三元运算符**

```
const val = myMap.get('Nope')?.val;// **This is an alternative to...**const val = myMap.has('Nope') && myMap.get('Nope') ? myMap.get('Nope').val : undefined;
```

# **真实世界示例-角度英尺。NgOnChanges**

**让我们抛开琐碎的例子，用可选的链接展示重构前后的一个更真实的代码例子。**

## **一些背景**

**我在我的项目中使用了 Angular，所以我在 Angular 组件中的一个函数中找到了一些带有大量可选链接的代码。**

**该组件实现了`NgOnChanges`生命周期钩子，该钩子在检测到组件的数据绑定输入的每个变化后执行。**

**这里可以使用可选链接，因为类型为`SimpleChanges` **的`ngOnChanges()`的`changes`参数需要深度对象访问**来检索改变的值，包括先前和当前的值。**

**此外，我在处理组件中的 Mapbox 标记时遇到了一个问题。经常会有这样的情况，当不是有意的时候，组件会让标记失效。**

**随后，当标记不存在时，组件会尝试与它交互。**

## **具有可选链接的 NgOnChanges 函数**

**由于知道 Mapbox 在涉及 Angular's Zone (NgZone)时会不经意间做出一些事情**，并且您永远无法保证某个标记与组件共存，因此**可选链接是一种确保应用程序在这些情况下不会中断的简单方法**。****

**NgOnChanges with Optional Chaining**

**`markerInstance`域是一个 Mapbox 标记，如果它存在的话**我只想处理它**。可选链接修复了这个问题，允许隐藏标记的未定义或空值，而不会引发错误。**

****看起来也很干净简单**。在可选链接之前，另一种选择怎么样…**

**NgOnChanges without Optional Chaining**

**如果没有可选的链接，由于**附加的显式条件检查** ( `if`语句)，函数逻辑看起来更复杂，可读性也更差。**

**因此，在可选链接之前编写该功能将产生一个具有 2-3 层深度的**嵌套函数，而不是 1 层深度。****

# **“但我不想让‘未定义’作为返回值”**

## **引入零化合并(？？)**

**为什么不把可选链接和新操作符`nullish coalescing`结合起来？**

**Nullish 合并启用一个**指定的默认**返回值。这与可选链接一起工作很好，因为它使用相同的**无效**检查来短路。如果左边表达式的结果是`null`或`undefined`，就会触发。**

```
const name = company.employees?.[0]?.name;// **with Nullish Coalescing...**const name = company.employees?.[0]?.name ?? 'Anonymous';
```

# **如何在您的项目中启用最新的 TypeScript & JavaScript 特性🚀**

**使用最新特性的能力取决于您安装的 TypeScript 编译器，以及`tsconfig.json`中的配置细节。**

## **安装 TypeScript 3.7**

```
$ npm i typescript@3.7 --save-dev
```

## **将“esnext”指定为 tsconfig.json 中的模块**

```
"module"                : "**esnext**",
"moduleResolution"      : "node",
"target"                : "**es5**",
"typeRoots"             : [
  "node_modules/@types"
],
```

# **所以总结一下**

**可选链接引入了一个干净简单的**替代结构，否则在处理属性访问链**时会涉及深度嵌套和不必要的复杂** `**if**` **语句**。****

**这是在对该变量进行操作时确保**非空**条件的可读性最好的方法。**

**感谢阅读！有任何问题，请在评论中告诉我。**

**喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！****