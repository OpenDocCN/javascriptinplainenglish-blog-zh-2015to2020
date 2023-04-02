# JavaScript 中的函数式编程特性

> 原文：<https://javascript.plainenglish.io/functional-programming-features-in-javascript-26abd12507a3?source=collection_archive---------7----------------------->

![](img/4862dc7339832c7868e30d8e8a5a25ce.png)

Photo by [Craig Lovelidge](https://unsplash.com/@_craigology?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。它还使用了许多函数式编程特性，使我们的生活更加轻松。

在本文中，我们将了解一些可以在代码中使用的 JavaScript 函数式编程特性。

# 什么是函数式编程？

函数式编程是一种编程范式，其中我们将计算视为数学函数的评估。这意味着我们避免改变状态和改变数据，

这是一种声明式编程范式，我们只告诉程序应该如何运行来创建我们的程序，而不是编写指令来告诉计算机如何一步一步地运行程序。

函数式编程强调使用纯函数，其中函数的参数决定其输出。

此外，应该消除函数中的副作用，因为它们会使函数变得不纯。

消除副作用也使函数更容易测试，因为我们只需要输入并检查它的输出，而不是检查副作用做了什么。

# 懒惰评估

惰性求值是仅在需要值时才求值。在 JavaScript 中，我们可以通过在函数中返回值，然后在需要时调用该函数来做到这一点。

例如，我们可以编写以下代码来实现这一点:

```
const lazy = () => 2 + 1
```

在上面的代码中，我们将值 3 作为`lazy`函数的返回值。

现在我们只在需要的时候通过调用`lazy`函数来获取值。

这与我们如下所述将 3 设置为变量相反:

```
let val = 2 + 1;
```

上面的代码会被立即计算，而不是等到`lazy`函数被计算后再返回值。

惰性求值避免了重复求值，这使得我们的代码比立即求值表达式更高效。

我们还可以通过返回函数来实现函数的惰性求值，而不是将函数直接赋给变量。

例如，我们可以编写以下代码来创建一个延迟求值的函数，如下所示:

```
const lazyAdd = (a, b) => () => a + b;
const add = lazyAdd(1, 2);
const result = add();
```

在上面的代码中，我们有`lazyAdd`函数，它返回一个返回两个参数相加的函数。

然后，当我们用两个数字调用`lazyAdd`函数时，我们返回一个将两个数字相加的函数，而不是直接返回计算后的表达式。

然后当我们调用`add`时，我们实际上返回的是加在一起的 1 和 2。

这再次延迟了对总和的评估，直到需要它的时候。它反对下面的代码:

```
const add = (a, b) => a + b;
const result = add(1, 2);
```

它立即计算两个加在一起的参数。

# 幺半群

在函数式编程中，幺半群是一组元素，当与名为`concat`的操作结合时，它们具有 3 个特殊的性质。

幺半群具有以下特征:

*   该操作必须将两个值合并为同一集合的第三个值。如果`a`和`b`是集合的一部分，那么`concat(a, b)`也应该是同一个集合的一部分。
*   该操作必须是关联的。也就是说，`concat(a, concat(b, c))`应该和`concat(concat(a, b), c)`一样。
*   器械包的操作必须有中性元素。如果中性元素与任何其他值组合，它不应该改变它。也就是`concat(a, neutral) === concat(neutral, a)`。

JavaScript 中有很多幺半群的例子。最常见的是字符串和数字。

例如，对于数字，我们可以检查它们是否满足上面的 3 个属性。

假设`a`是 1 而`b`是 2，并且`concat`操作符是`+`，我们可以通过编写以下内容来检查第一个属性是否满足:

```
a + b === 3
```

`a`和`b`都是数字，3 是数字，所以在同一个集合中，满足第一个条件。

然后，我们可以按如下方式检查关联性:

```
((a + b) + 3) === (a + (b + 3))
```

由于返回了`true`，我们可以按照我们想要的顺序对它们进行分组，所以`+`和数字是关联的。

我们可以通过选择 0 作为中性元素来检查一个集合是否有中性元素。

例如:

```
a + 0 === 0 + a
```

返回`true`，所以我们知道这组数字有一个中性元素，是 0。因此，我们知道当数字相加时有一个中性元素。

所以带数的加法运算是幺半群。

幺半群是有用的，因为它允许我们组合多个简单的操作来创建复杂的行为，而不必在代码中引入新概念。

它们总是返回相同集合中的元素，所以我们可以继续将这些元素和操作组合在一起。

结合性让我们可以忽略组合的顺序，仍然得到相同的结果，减少了我们在需要组合多个操作时的认知负荷。

![](img/884ffeb2aca52998a949aaad52babd87.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

JavaScript 展示了一些函数式编程的特征。这使得编写代码更像是定义和使用数学函数。

例如，事物可以被懒惰地评估以提高效率，它有像数字加法这样的幺半群。幺半群是有用的，因为多重运算容易合成。

函数式编程强调纯函数，更容易测试。

## 简明英语笔记

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****