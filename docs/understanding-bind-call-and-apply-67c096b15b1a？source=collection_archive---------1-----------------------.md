# 理解 JavaScript 中的绑定、调用和应用

> 原文：<https://javascript.plainenglish.io/understanding-bind-call-and-apply-67c096b15b1a?source=collection_archive---------1----------------------->

## 通过海绵宝宝演示函数方法

![](img/1f483b561a3fc88cdb99acd6f5d31edd.png)[![](img/c19cb3069af1beba3c93258d9fcfe139.png)](https://www.webtips.dev/bind-call-apply-in-javascript)

就像在现实世界中给烧伤的地方浇冷水一样，我们也可以在数字世界中给我们的函数调用添加额外的信息。最近，我试图澄清围绕 JavaScript 的`[this](https://www.webtips.dev/javascript-this-keyword)` [关键字](https://www.webtips.dev/javascript-this-keyword)的混乱，我简单地提到了`bind`和`call`。但是这一次，我想通过一些关于`apply`的补充来更深入地了解他们。
我们按顺序来，按标题来，从 bind 开始。但是首先，我们需要一些代码来演示所有这三种方法，所以请看下面的内容:

Nothing out of the ordinary, just two aquatic animals

# 约束

`bind`在 JavaScript 中用于将特定上下文绑定到函数。当你有一个名为`funky`的函数，你这样调用它:`funky.bind(soul)`，你实际上是在创建一个新函数，其中`this`的上下文被设置为`soul`的值。请记住，这不会修改原始函数，也不会调用。

上面的代码示例演示了`bind`并没有改变实际的函数，而是创建了一个全新的函数。当我们第二次调用`greetPatrick()`时，由于绑定的上下文，我们得到了 Patrick 的详细信息，即使我们调用的是`spongbob.greet`。

# 打电话

与`bind`不同的是，`call`实际上会用指定的上下文立即调用函数。下面我们来看看:

在第 9 行，我们用`spongebob`上下文调用海绵宝宝，对于参数，我们传入一个字符串。这一行基本上相当于以下内容:

```
spongebob.greet('I'\m a good noodle!');
```

# 应用

`Apply`的功能与`call`相同。两者之间唯一的区别是，`call`接受一个**参数列表**，而`apply`接受一个**参数数组。**

注意`call`和`apply`的区别。一个用数组调用，另一个不用。如果我们有多个参数，它们将如下所示:

我想这就是三者之间的区别。让我们回顾一下所有的事情，并得出结论。

# 结论

*   当您想要将上下文绑定到稍后要调用的函数**时，请使用`bind`**
*   **如果您想立即调用功能**，请使用`call`或`apply`****

****当谈到`call`和`apply`时，宇宙最大的问题是****

> ****选择哪一个？这真的取决于你的选择。****

*****虽然如果我们看看哪一个表现更好，似乎* [*胜利者*](https://jsperf.com/call-apply-segu) *是* ***的召唤。*******

****[![](img/e66c4cd6d9849ac0bd245f3fc39b65c6.png)](https://medium.com/@ferencalmasi/membership)********[![](img/b0d8e0a0c2689a59aa62a677429b83b7.png)](https://www.webtips.dev/)****