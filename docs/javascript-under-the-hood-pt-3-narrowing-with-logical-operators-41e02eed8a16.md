# 引擎盖下的 JavaScript。3:使用逻辑运算符缩小范围

> 原文：<https://javascript.plainenglish.io/javascript-under-the-hood-pt-3-narrowing-with-logical-operators-41e02eed8a16?source=collection_archive---------14----------------------->

![](img/bcafc4685baed690c42bc4b637fe2b2f.png)

`||`(逻辑 OR)和`&&`(逻辑 and)是 JavaScript 中非常常见的运算符。但是你知道为什么它们以这样的方式工作，以及它们还可以怎样被使用吗？不仅如此，你知道他们的陷阱吗？如果你不知道它们是如何工作的，逻辑 OR 和逻辑 and 实际上会在你的代码中引起难以发现的错误。

> 如果你不知道它们是如何工作的，逻辑 OR 和逻辑 and 会在你的代码中产生难以发现的错误。

除了它们的流行用法，逻辑 OR `||`和逻辑 AND `&&`实际上可以相当容易和有效地用来代替简单的条件语句，并且经常可以将它们减少到一行。

在本文中，我们将讨论这两个操作符是如何工作的，如何/何时用它们来替换条件语句，并讨论它们的缺陷。

# 逻辑 OR 和逻辑 and 是如何工作的？

首先，如果我们看一下 [Mozilla 的操作符优先级表](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)(向下滚动一点就可以看到这个表)，我们可以看到这两个表都有**从左到右的结合性(通常称为“左结合性”)**，这实质上意味着 JavaScript 引擎首先读取操作符左侧的内容，然后读取右侧的内容。这在接下来的几个要点中很重要。(如果你想了解更多关于操作符结合性的内容，请查看这篇[维基百科文章](https://en.wikipedia.org/wiki/Operator_associativity)

在我们继续之前，我想给 [Execute Program 的](https://www.executeprogram.com/)关于类型脚本的课程一点信任。虽然本文不是关于 TypeScript 的，但是在他们的课程中会解释这些概念。

为了理解`||`和`&&`如何产生无法预见的错误，让我们简单回顾一下 JavaScript 中的错误值。

## JavaScript 中的 Falsey 值

*   `false`
*   `0`
*   `0n`(在 ECMAScript 2020 中与“BigInt”一起使用)
*   `'’`
*   `undefined`
*   `null`
*   `NaN`

所有其他的价值观都是真实的。

您能猜到为什么其中一些会导致我们正在讨论的操作符出现意外错误吗？

通常我们认为`&&`是一种处理布尔值的方法。

```
true && false
//falsefalse && false
//falsetrue && true
//true
```

`**&&**` **返回它看到的第一个 falsey 值。但是如果两个值都是真的，它返回右边的值**(记住`&&`使用左边的结合性)。

```
1 && 2
//2undefined && 2
//undefined2 && false
//falsenull && 2
//nulltrue && “hello”
//”hello”
```

> `&&`返回它看到的第一个 falsey 值。但是如果两个值都是真的，它返回右边的值

**逻辑或** `**||**`工作方式类似，但是**返回它看到的第一个真值。但是如果两个值都为假，它返回右边的值**

```
1 || 2
//1undefined || 2
//2“hello” || null
//”hello”false || null
//null
```

> ||返回它看到的第一个真值。但是如果两个值都为 falsey，它将返回右边的值

# 逻辑 OR 和逻辑 and 怎么做条件语句？

利用我们现在所知道的，让我们来回答这个问题。

首先，让我们使用一个简单的‘if else’语句将`1`加到`x`上，如果`x`为真，那么使用逻辑 AND 运算符来达到同样的目的。

现在让我们做同样的事情，但是把`x`改为`undefined`(一个假值)。

明白这是怎么回事了吗？

在第一个例子中，JavaScript 引擎检查逻辑 AND 的左边，发现`x`是真的，所以接下来检查右边。然后它再次看到`x`，这仍然是真的，所以逻辑 AND 返回`2`(因为`x === 2`)。然后 JavaScript 引擎在同一行看到`+`(它也有从左到右的关联性)，所以它把`+`(也就是`1`)右边的内容加到`&&`返回的内容(也就是`2`)上，然后返回`3`。

在第二个例子中，JavaScript 引擎检查逻辑 AND 的左边，看到了`x`，它现在是一个假值`undefined`。所以它就停在那里，并返回 falsey 值，`undefined`。

让我们试试逻辑 OR `||`(记住它返回它看到的第一个真值，如果两个都为假，则返回第二个值)。

在最后一个例子中，`||`返回`5`，因为它是一个真值。右边的表达式甚至看不到。

现在让我们使`x`变得虚假。

**这些概念很强大。**

**让我们更进一步**写一些稍微复杂一点的东西。

## 写一个函数，你知道它要么接受一个数组，要么为空。如果该值是一个数组，返回数组的长度，否则返回该值。

使用`&&`的第二个函数更短更简洁(尽管代价是有点难以理解)。

现在让我们使用逻辑 OR 运算符作为另一个示例的条件。

## 编写一个接受值的函数。如果该值为真，则返回该值，否则返回-1。

# 当||和&&有危险时

回想一下我们上面列出的错误价值观。**JavaScript 的一个大问题是**`**0**`**`**'’**`**是假值**。你可以想象这会引起多大的混乱！**

****让我们用** `**||**` **创建一个函数，但要以一种你团队其他人不会注意到的方式破解它。**(假设这个函数要么取一个数，要么取`undefined`。)**

**它返回了`-1`！但是`0`对我们人类来说是一个数字！**

> **JavaScript 的一个大问题是 0 和' '(空字符串)是假值。**

**处理这个问题的一个简单方法是，如果你认为一个变量可能以 0 或“”结束，选择使用一个简单的 if/else 语句。**

**示例:**

```
let defaultInt = 0if (defaultInt === 0) {
    return 1
} else {
    return defaultInt + 1
}
// 1
```

****实现相同目标的另一种更简洁的方法是使用 ES2020 nullish 合并运算符(？？).**在 [Mozilla 开发者页面](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)上是这样描述的:*“****nullish 合并运算符(******)****是一个逻辑运算符，当其左侧操作数为* `*null*` *或* `*undefined*` *时，返回其右侧操作数，否则返回左侧操作数。”***

**文档中的示例:**

```
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

**本质上，它会将 0 和''视为或多或少的“真”值，这不会像逻辑 or 运算符那样导致任何意外的行为。**

**因此，总的来说，逻辑 OR 和逻辑 and 在简化简单的条件表达式时非常有用。然而，需要很好地理解它们是如何工作的，以便利用它们，并且不给你的团队造成混乱。**

**你在编程中使用过逻辑运算符作为条件语句吗？它有没有给你带来什么看不见的 bug？**

**如果您喜欢这篇文章，并且想要更多的高级 JavaScript 概念，请查看本系列的其余部分！**