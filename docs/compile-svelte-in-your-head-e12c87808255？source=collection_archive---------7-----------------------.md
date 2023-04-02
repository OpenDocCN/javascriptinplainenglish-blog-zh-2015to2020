# 在你的头脑中编译苗条

> 原文：<https://javascript.plainenglish.io/compile-svelte-in-your-head-e12c87808255?source=collection_archive---------7----------------------->

## 第 1 部分:Svelte 如何查看您的代码并将其编译成普通的 JavaScript

![](img/4eac6b465cf50393db8178ba6ae26e5b.png)

# 背景

前阵子， [@swyx](https://twitter.com/swyx) 回到新加坡，在 [Shopee Singapore](https://careers.shopee.sg/about/) ( [我们正在招人！](https://grnh.se/32e5b3532))。

他在[可识别的原创](https://reactknowledgeable.org/)中对[编译你脑中的苗条](https://www.swyx.io/speaking/svelte-compile-lightning/) ( [视频](https://www.youtube.com/watch?v=FNmvcswdjV8))进行了惊人的分享。

我喜欢他的演示，标题很吸引人，所以我恳求他用这个吸引人的标题作为这一系列关于苗条编译器的文章。这将是关于如何 Svelte 看到你的代码，并将其编译成普通的 JavaScript。

# 介绍

让我们回顾一下如何在没有任何框架的情况下编写 web 应用程序:

**创建元素**

**更新元素**

**移除一个元素**

**给一个元素添加样式**

**监听元素上的点击事件**

这些是你必须编写的代码，不需要使用任何框架或库。

这篇文章的主要思想是展示 svelite 编译器如何将 svelite 语法编译成我上面展示的代码语句。

# **纤细的语法**

这里我将向你展示一些简单语法的基础。

> *如果你希望了解更多，我强烈推荐尝试一下* [*Svelte 的互动教程*](https://svelte.dev/tutorial/basics) *。*

这是一个基本的苗条元素:

[苗条的 REPL](https://svelte.dev/repl/99aeea705b1e48fe8610b3ccee948280)

要添加样式，您需要添加一个`<style>`标签:

苗条的 REPL

在这一点上，编写苗条的组件感觉就像编写 HTML，这是因为苗条的语法是 HTML 语法的超级集合。

让我们看看如何将数据添加到组件中:

苗条的 REPL

我们把 JavaScript 放在花括号里。

为了添加一个点击处理程序，我们使用了`on:`指令

[苗条的 REPL](https://svelte.dev/repl/1da1dcaf51814ed09d2341ea7915f0a1)

为了改变数据，我们使用[赋值操作符](https://www.w3schools.com/js/js_assignment.asp)

[苗条的 REPL](https://svelte.dev/repl/7bff4b7746df4007a51155d2006ce724)

让我们继续来看看如何将简单的语法编译成我们之前看到的 JavaScript

# **在你的头脑中编译苗条的身材**

Svelte 编译器分析您编写的代码，并生成优化的 JavaScript 输出。

为了研究 Svelte 如何编译代码，让我们从尽可能小的例子开始，慢慢地构建代码。通过这个过程，您将看到 Svelte 根据您的更改逐渐增加输出代码。

我们要看的第一个例子是:

苗条的 REPL

输出代码:

您可以将输出代码分成两部分:

*   `create_fragment`
*   `class App extends SvelteComponent`

**创建 _ 片段**

苗条的组件是苗条的应用程序的构建块。每个纤细的组件都专注于构建最终 DOM 的片段。

`create_fragment`函数为这个苗条的组件提供了如何构建 DOM 片段的指导手册。

再看`create_fragment`函数的返回对象。它有一些方法，例如:

**- c()**

**的简称创建**。

包含创建片段中所有元素的指令。

在这个例子中，它包含了创建`h1`元素的指令

**- m(目标，锚)**

**座**的简称。

包含将元素装入目标的说明。

在本例中，它包含将`h1`元素插入到`target`中的指令。

**- d(分离)**

**销毁**的简称。

包含从目标中移除元素的指令。

在这个例子中，我们将`h1`元素从 DOM 中分离出来

> *为了更好地缩小，方法名称是简短的。* [*见此处有什么不能缩小的*](https://alistapart.com/article/javascript-minification-part-ii/#section3) *。*

每个组件都是一个类，你可以通过[这个 API](https://svelte.dev/docs#Client-side_component_API) 导入和实例化。

在构造函数中，我们用组成组件的信息初始化组件，比如`create_fragment`。Svelte 只会传递需要的信息，并在不需要的时候删除它们。

尝试移除`<h1>`标签，看看输出会发生什么:

苗条的 REPL

[https://gist.github.com/efc401d42d1161c8cbb83ae722a4eab1](https://gist.github.com/efc401d42d1161c8cbb83ae722a4eab1)

苗条将通过`null`而不是`create_fragment`！

`init`函数是 Svelte 设置大部分内部组件的地方，比如:

*   组件道具、`ctx`(后面会解释什么是`ctx`)和上下文
*   组件生命周期事件
*   组件更新机制

最后，Svelte 调用`create_fragment`来创建元素并将其安装到 DOM 中。

如果你注意到了，所有的内部状态和方法都被附加到了`this.$$`。

因此，如果您曾经访问过组件的`$$`属性，那么您就是在接触内部。你已经被警告了！🙈🚨

# **添加数据**

既然我们已经看到了一个瘦组件的最少部分，让我们看看添加一个数据会如何改变编译后的输出:

[苗条的 REPL](https://svelte.dev/repl/c149ca960b0444948dc0c00a9175bcb3?version=3.19.1)

请注意输出中的变化:

一些观察结果:

*   您在`<script>`标签中写的内容被移到代码的顶层
*   `h1`元素的文本内容现在是一个模板文本

现在有许多令人惊奇的事情正在发生，但是让我们先不要着急，因为这在与下一次代码更改进行比较时会得到最好的解释。

# **更新数据**

让我们添加一个函数来更新`name`:

苗条的 REPL

…并观察编译输出的变化:

一些观察结果:

*   `<h1>`元素的文本内容现在被分成两个文本节点，由`text(...)`函数创建
*   `create_fragment`的返回对象有一个新方法，`p(ctx, dirty)`
*   一个新的功能`instance`被创建
*   您在`<script>`标签中写的内容现在被移到了`instance`函数中
*   对于眼尖的人来说，在`create_fragment`中使用的变量`name`现在被`ctx[0]`代替了

那么，为什么要改变呢？

苗条编译器跟踪所有在`<script>`标签中声明的变量。

它跟踪变量是否:

*   可以变异？`count++`例:
*   可以重新分配？`name = 'Svelte'`例:
*   模板中引用了。例:`<h1>Hello {name}</h1>`
*   是可写的？例:`const i = 1;`对`let i = 1;`
*   …以及更多

当瘦编译器意识到变量`name`可以被重新分配时，(由于`update`中的`name = 'Svelte';`，它将`h1`的文本内容分解成几个部分，这样它就可以动态更新部分文本。

事实上，您可以看到有一个新的方法，`p`，用于更新文本节点。

**- p(ctx，dirty)**

**u *p* 日期**的简称。

**p(ctx，dirty)** 包含根据组件的状态(`dirty`)和状态(`ctx`)的变化来更新元素的指令。

**实例变量**

编译器意识到变量`name`不能在`App`组件的不同实例之间共享。这就是为什么它将变量`name`的声明移到一个名为`instance`的函数中。

在前面的示例中，无论有多少个`App`组件实例，变量`name`的值都是相同的，并且在所有实例中保持不变:

**实例($$self，$$props，$$invalidate)**

但是，在这个例子中，变量`name`可以在组件的一个实例中被改变，所以变量`name`的声明现在被移到了`instance`函数中:

`instance`函数返回一列*实例*变量，这些变量是:

*   模板中引用的
*   变异或重新分配(可以在组件的一个实例内更改)

在 Svelte 中，我们称这个实例变量列表为 **ctx** 。

在`init`函数中，Svelte 调用`instance`函数来创建 **ctx** ，并使用它来创建组件的片段:

现在，我们引用通过 **ctx** 传递的变量`name`，而不是访问组件外部的变量`name`:

ctx 是一个数组而不是地图或对象的原因是因为与位掩码相关的优化，你可以在这里看到关于它的讨论

**$ $无效**

苗条身材反应系统背后的秘密是`$$invalidate`功能。

每一个变量

*   重新分配或变异
*   模板中引用的

将在赋值或变异后插入`$$invalidate`函数:

`$$invalidate`函数将变量标记为脏，并为组件安排一次更新:

**添加事件监听器**

现在让我们添加一个事件监听器

[苗条的 REPL](https://svelte.dev/repl/5b12ff52c2874f4dbb6405d9133b34da?version=3.19.1)

观察不同之处:

一些观察结果:

*   `instance`函数现在返回 2 个变量而不是 1 个
*   在**挂载**期间监听点击事件，并在**销毁**中处理

正如我前面提到的，`instance`函数返回模板中引用的**变量以及**变异或重新分配的**变量。**

由于我们刚刚在模板中引用了`update`函数，它现在作为 **ctx** 的一部分返回到`instance`函数中。

Svelte 试图生成尽可能紧凑的 JavaScript 输出，如果不必要，不返回额外的变量。

**倾听和处置**

每当你在 Svelte 中添加[一个事件监听器](https://svelte.dev/tutorial/dom-events)时，Svelte 会注入代码来添加一个[事件监听器](https://developer.mozilla.org/en-US/docs/Web/API/EventListener)并在从 DOM 中移除 DOM 片段时移除它。

尝试添加更多事件侦听器，

[苗条的 REPL](https://svelte.dev/repl/efde6f2aaf624e708767f1bd3e94e479?version=3.19.1)

并观察编译后的输出:

Svelte 没有声明和创建一个新变量来删除每个事件侦听器，而是将它们全部分配给一个数组:

缩小可以压缩变量名，但是不能去掉括号。

同样，这是 Svelte 试图生成紧凑 JavaScript 输出的另一个很好的例子。当只有一个事件监听器时，Svelte 不会创建`dispose`数组。

# **总结**

简单语法是 HTML 的超集。

当你编写一个简单的组件时，简单的编译器会分析你的代码并生成优化的 JavaScript 代码输出。

输出可分为 3 个部分:

**1。创建 _ 片段**

*   返回一个片段，这是关于如何为组件构建 DOM 片段的说明手册

**2。实例**

*   写在`<script>`标签中的大部分代码都在这里。
*   返回模板中引用的实例变量列表
*   在实例变量的每次赋值和变异后插入`$$invalidate`

**3。class App 扩展了 svelet component**

*   用`create_fragment`和`instance`功能初始化组件
*   设置组件内部
*   提供[组件 API](https://svelte.dev/docs#Client-side_component_API)

Svelte 努力生成尽可能紧凑的 JavaScript，例如:

*   仅当部分文本可以更新时，将`h1`的文本内容分解成单独的文本节点
*   不需要时不定义`create_fragment`或`instance`功能
*   根据事件侦听器的数量，将`dispose`生成为数组或函数。
*   …

# **结束语**

我们已经介绍了 Svelte 编译输出的基本结构，这仅仅是开始。

如果你想了解更多，请在 Twitter 上关注我。

当下一部分准备好的时候，我会把它发布在 Twitter 上，在那里我会涵盖[逻辑块](https://svelte.dev/tutorial/if-blocks)、[插槽](https://svelte.dev/tutorial/slots)、[上下文](https://svelte.dev/tutorial/context-api)以及许多其他内容。

*原载于*[*https://lihautan.com*](https://lihautan.com/compile-svelte-in-your-head-part-1/)*。*