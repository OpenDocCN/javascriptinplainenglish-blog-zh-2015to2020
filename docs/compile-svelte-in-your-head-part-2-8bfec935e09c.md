# 在你的头脑中编译苗条

> 原文：<https://javascript.plainenglish.io/compile-svelte-in-your-head-part-2-8bfec935e09c?source=collection_archive---------7----------------------->

## 第二部分

在[第 1 部分](https://lihautan.com/compile-svelte-in-your-head-part-1/)中，我提到了`$$invalidate`函数，我解释了`$$invalidate`函数的工作原理如下:

但这并不是`$$invaldiate`功能的确切实现。所以在这篇文章中，我们将看看`$$invalidate`是如何在 Svelte 中实现的。

在撰写本文时，Svelte 处于 [v3.20.1](https://github.com/sveltejs/svelte/blob/v3.20.1/CHANGELOG.md#3201) 。

# **3 . 16 . 0 版之前**

在 [v3.16.0](https://github.com/sveltejs/svelte/blob/master/CHANGELOG.md#3160) 中，也就是在 [#3945](https://github.com/sveltejs/svelte/pull/3945) 中，有一个很大的优化改变了`$$invalidate`函数的底层实现。基本概念没有改变，但是在改变之前理解`$$invalidate`和单独了解优化改变会容易得多。

让我们解释一下你将会看到的一些变量，其中一些已经在[第一部分](https://gist.github.com/compile-svelte-in-your-head-part-1)中介绍过了:

## $$.ctx

它没有正式的名字。您可以称之为 **context** ，因为它是模板渲染到 DOM 所基于的上下文。

我把它叫做[实例变量](https://lihautan.com/compile-svelte-in-your-head-part-1/#instance-variable)。因为它是一个包含所有变量的 JavaScript 对象，您可以:

*   在`<script>`标签中声明
*   变异或重新分配
*   模板中引用的

属于组件实例的。

实例变量本身可以是原始值、对象、数组或函数。

`instance`函数创建并返回`ctx`对象。

在`<script>`标签中声明的函数将引用在`instance`函数闭包中作用域的实例变量:

苗条的 REPL

每当创建一个组件的新实例时，就会调用`instance`函数，并在新的闭包范围内创建和捕获`ctx`对象。

## $$.肮脏的

`$$.dirty`是一个对象，用于跟踪哪个实例变量刚刚发生了变化，需要更新到 DOM 上。

例如，在下面的细长组件中:

苗条的 REPL

最初的`$$.dirty`是`null` ( [源代码](https://github.com/sveltejs/svelte/blob/v3.15.0/src/runtime/internal/Component.ts#L124))。

如果您点击了**“+敏捷”**按钮，`$$.dirty`将变成:

如果您点击了**“升级”**按钮，`$$.dirty`将变成:

`$$.dirty`对 Svelte 很有用，这样就不会不必要地更新 DOM。

如果你查看了编译代码的 **p (u *p* date)** 函数，你会看到在更新 DOM 之前，Svelte 检查变量是否在`$$.dirty`中被标记。

在 Svelte 更新 DOM 之后，`$$.dirty`被设置回`null`以指示所有的更改都已经应用到 DOM 上。

## $ $无效

`$$invalidate`是苗条身材反应性背后的秘密。

每当变量被

Svelte 将使用`$$invalidate`函数完成分配或更新:

`$$invalidate`功能将:

1.  更新`$$.ctx`中的变量
2.  在`$$.dirty`中标记变量
3.  计划更新
4.  返回赋值或更新表达式的值

[源代码](https://github.com/sveltejs/svelte/blob/v3.15.0/src/runtime/internal/Component.ts#L130-L136)

关于函数`$$invalidate`的一个有趣的注意事项是，它包装了赋值或更新表达式，并返回表达式计算的结果。

这使得`$$invalidate`可链接:

当一条语句中有很多赋值或更新表达式时，这看起来很复杂！🙈

`$$invalidate`的第二个参数是赋值或更新表达式。但是如果它包含任何赋值或更新子表达式，我们用`$$invalidate`递归地包装它。

如果赋值表达式改变了一个对象的属性，我们把这个对象作为`$$invalidate`函数的第三个参数传入，例如:

因此，我们将变量`"obj"`更新为`obj`，而不是第二个参数`"hello"`的值。

## 计划 _ 更新

调度 Svelte 用目前为止所做的更改来更新 DOM。

Svelte，在写的时候( [v3.20.1](https://github.com/sveltejs/svelte/blob/v3.20.1/CHANGELOG.md#3201) )用[微任务队列](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)批量更改更新。实际的 DOM 更新发生在下一个微任务中，因此在同一个任务中发生的任何同步`$$invalidate`操作都会被批量处理到下一个 DOM 更新中。

为了安排下一个微任务，Svelte 使用了 Promise 回调。

在`flush`中，我们为每个标记为脏的组件调用 update:

[源代码](https://github.com/sveltejs/svelte/blob/v3.15.0/src/runtime/internal/scheduler.ts#L14)

所以，如果你像这样写一个细长的组件:

苗条的 REPL

`givenName`和`familyName`的 DOM 更新发生在同一个微任务中:

1.  点击**“更新”**调用`update`功能
2.  `$$invalidate('givenName', givenName = 'Li Hau')`
3.  将变量`givenName`标记为脏，`$$.dirty['givenName'] = true`
4.  安排一次更新，`schedule_update()`
5.  因为这是调用堆栈中的第一次更新，所以将`flush`函数推入微任务队列
6.  `$$invalidate('familyName', familyName = 'Tan')`
7.  将变量`familyName`标记为脏，`$$.dirty['familyName'] = true`
8.  安排一次更新，`schedule_update()`
9.  自`update_scheduled = true`以来，无所事事。
10.  **—任务结束—**
11.  **—微任务开始—**
12.  `flush()`为每个标记为脏的组件调用`update()`
13.  通话`$$.fragment.p($$.dirty, $$.ctx)`。

*   `$$.dirty`现在是`{ givenName: true, familyName: true }`
*   `$$.ctx`现在是`{ givenName: 'Li Hau', familyName: 'Tan' }`

14.在`function p(dirty, ctx)`中，

*   如果`$$.dirty['givenName'] === true`，则将第一个文本节点更新为`$$.ctx['givenName']`
*   如果`$$.dirty['familyName'] === true`，则将第二个文本节点更新为`$$.ctx['familyName']`

15.将`$$.dirty`重置为`null`

16\. …

17。—微任务结束—

## TL；博士；医生

*   对于每次赋值或更新，Svelte 调用`$$invalidate`来更新`$$.ctx`中的变量，并在`$$.dirty`中将变量标记为 dirty。
*   实际的 DOM 更新被分批到下一个微任务队列中。
*   为了更新每个组件的 DOM，组件`$$.fragment.p($$.diry, $$.ctx)`被调用。
*   在 DOM 更新之后，`$$.dirty`被重置为`null`。

# **v3.16.0**

v3.16.0 中的一个大变化是 PR [#3945](https://github.com/sveltejs/svelte/pull/3945) ，即**基于位掩码的变更跟踪**。

不使用对象将变量标记为脏:

Svelte 给每个变量分配一个索引:

并使用[位掩码](https://en.wikipedia.org/wiki/Mask_(computing))来存储脏信息:

这比以前编译的代码要紧凑得多。

## 位掩码

对于不理解的人，请允许我快速解释一下它是什么。

当然，如果你想了解更多，可以随意阅读更详细的解释，像[这个](https://blog.bitsrc.io/the-art-of-bitmasking-ec58ab1b4c03)和[这个](https://dev.to/somedood/bitmasks-a-very-esoteric-and-impractical-way-of-managing-booleans-1hlf)。

表示一组`true`或`false`的最简洁的方式是使用位。如果该位是`1`，则为`true`，如果是`0`，则为`false`。

一个数可以用二进制表示， **5** 就是二进制的`0b0101`。

如果 **5** 用 4 位二进制表示，那么它可以存储 4 个布尔值，第 0 位和第 2 位为`true`，第 1 位和第 3 位为`false`(从右向左读取，从[最低有效位](https://en.wikipedia.org/wiki/Bit_numbering#Least_significant_bit)到[最高有效位](https://en.wikipedia.org/wiki/Bit_numbering#Most_significant_bit))。

**一个数可以存储多少个布尔值？**

这取决于语言，Java 中的 16 位整数可以存储 16 个布尔值。

在 JavaScript 中，数字可以用 64 位的[来表示。但是，当对数字使用](https://2ality.com/2012/04/number-encoding.html)[位操作](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)时，JavaScript 会将数字视为 32 位。

为了检查或修改存储在数字中的布尔值，我们使用[位操作](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)。

我们在按位运算中使用的第二个操作数就像是一个[掩码](https://en.wikipedia.org/wiki/Mask_(computing))，它允许我们锁定第一个数中的一个特定位，该位存储我们的布尔值。

我们称这个掩码为**位掩码**。

## **纤细的位掩码**

如前所述，我们给每个变量分配一个索引:

因此，我们现在不是将实例变量作为 JavaScript 对象返回，而是将其作为 JavaScript 数组返回:

通过**索引**、`$$.ctx[index]`而不是**变量名**访问变量:

`$$invalidate`函数的工作原理相同，除了它接受的是**索引**而不是**变量名**:

`$$.dirty`现在存储一个数字列表。每个数字携带 31 个布尔值，每个布尔值指示该索引的变量是否脏。

要将变量设置为脏，我们使用按位运算:

为了验证一个变量是否脏，我们也使用了位运算！

通过使用位掩码，`$$.dirty`现在被重置为`[-1]`，而不是`null`。

**花絮:** `-1`是二进制的`0b1111_1111`，这里所有的位都是`1`。

## 解构$$。肮脏的

Svelte 所做的一个代码大小优化是，如果变量少于 32 个，总是析构 **u *p* date 函数**中的`dirty`数组，因为我们无论如何都会访问`dirty[0]`:

## TL；博士；医生

*   `$$invalidate`和`schedule_update`的基本机制不变
*   使用位掩码，编译后的代码更加紧凑

# 被动声明

允许我们通过[标签声明](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)、`$:`声明反应值

苗条的 REPL

如果您查看编译后的输出，您会发现声明性语句出现在`[instance](https://gist.github.com/compile-svelte-in-your-head-part-1/#instanceself-props-invalidate)` [函数](https://gist.github.com/compile-svelte-in-your-head-part-1/#instanceself-props-invalidate)中:

尝试对反应声明重新排序，并观察编译输出中的变化:

苗条的 REPL

一些观察结果:

*   当有反应声明时，Svelte 定义一个定制的`$$.update`方法。
*   `*$$.update*` *是* [*的无运算功能*](https://en.wikipedia.org/wiki/NOP_(code)) *默认。(参见*[*src/runtime/internal/component . ts*](https://github.com/sveltejs/svelte/blob/v3.20.1/src/runtime/internal/Component.ts#L111)*)*
*   Svelte 也使用`$$invalidate`来更新反应变量的值。
*   Svelte 根据声明和语句之间的依赖关系对反应性声明和语句进行排序
*   `quadrupled`依赖`doubled`，所以`quadrupled`被求值`$$invalidate` d 在`doubled`之后。

由于所有的反应性声明和语句都归入了`$$.update`方法，而且事实上 Svelte 将根据它们的依赖关系对声明和语句进行排序，这与声明它们的位置和顺序无关。

以下组件仍然工作:

[苗条的 REPL](https://svelte.dev/repl/fc6995856489402d90291c4c30952939?version=3.20.1)

**接下来你可能会问，** `**$$.update**` **是什么时候被调用的？**

还记得在`flush`函数中被调用的`update`函数吗？

我放了个`NOTE:`评论说后面会很重要。现在这很重要。

就在我们调用`$$.fragment.p()`更新 DOM 之前，`$$.update`函数在同一个微任务中被调用**。**

上述事实的含义是

**1。所有反应性声明和语句的执行都是批处理的**

正如 DOM 更新是如何批处理的一样，反应式声明和语句也是批处理的！

[苗条的 REPL](https://svelte.dev/repl/941195f1cd5248e9bd14613f9513ad1d?version=3.20.1)

当`update()`被呼叫时，

1.  类似于上述流程，`$$invalidate`两者**【给定名称】**和**【家庭名称】**，并且安排更新
2.  **—任务结束—**
3.  **—微任务开始—**
4.  `flush()`为每个标记为脏的组件调用`update()`
5.  跑步`$$.update()`

*   由于**【given name】****【family name】**发生了变化，评估为`$$invalidate`**【name】**
*   随着**【姓名】**的改变，执行`console.log('name', name);`

6.调用`$$.fragment.p(...)`来更新 DOM。

如你所见，尽管我们已经更新了`givenName`和`familyName`，我们只计算`name`并执行`console.log('name', name)` **一次**而不是两次:

**2。反应声明和语句之外的反应变量值可能不是最新的**

因为反应性声明和语句是在下一个微任务中批处理和执行的，所以不能期望值被同步更新。

苗条的 REPL

相反，你必须在另一个反应声明或语句中引用反应变量:

## **反应性声明和报表的排序**

Svelte 试图尽可能地保持反应性声明和陈述的顺序。

然而，如果一个反应性声明或语句引用了由另一个反应性声明定义的变量，那么，**将被插入后一个反应性声明**之后:

## **不反应的反应变量**

苗条编译器跟踪所有在`<script>`标签中声明的变量。

如果一个反应性声明或语句所引用的所有变量从未发生变异或重新分配，那么该反应性声明或语句将不会被添加到`$$.update`中。

例如:

苗条的 REPL

因为，`count`永远不会变异或重新分配，Svelte 通过不定义`$$self.$$.update`来优化编译后的输出。

如果你想了解更多，请在 Twitter 上关注我。

当下一部分准备好的时候，我会把它发布在 Twitter 上，在那里我会涵盖[逻辑块](https://svelte.dev/tutorial/if-blocks)、[插槽](https://svelte.dev/tutorial/slots)、[上下文](https://svelte.dev/tutorial/context-api)以及许多其他内容。

**⬅ ⬅先前在** [**第一部**](https://lihautan.com/compile-svelte-in-your-head-part-1/) **。**

*最初发表于*[*https://lihautan.com*](https://lihautan.com/compile-svelte-in-your-head-part-2/)