# JavaScript 中生命的 4 个实际用例

> 原文：<https://javascript.plainenglish.io/4-practical-use-cases-for-iifes-in-javascript-6481dcb0ba7d?source=collection_archive---------2----------------------->

## 让我们用生命编写更安全的代码

![](img/ce8de6f5864736e903aa49f076e60999.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你知道生命的概念，对吗？

简单地说，从它的全名，立即调用函数表达式来看，它是一个一旦定义就运行的函数。

IIEFs 以保护变量作用域而闻名。但这实际上意味着什么呢？IIEFs 的实际使用案例是什么？

你会在这篇文章中知道答案。让我们深入研究一下。

# 关闭

在生命中定义的变量不能从外部访问。或者当你使用`let`或`const`声明一个变量时，它只能在封闭块中被访问。

但是，有时您可能需要修改这些变量的值。可能吗？是的，它是。

方法如下:

你知道闭包，对吧？闭包使您能够从内部函数访问外部函数的作用域。创建闭包只不过是在另一个函数中定义一个函数并公开它。

当把结束和生活结合起来时，你会得到两个好处。

第一个是保护变量的范围以防止意外行为。

第二，你可以从外部修改函数内部的变量。听起来我们正在打破第一个好处。但是我们没有。因为您不能直接修改变量值，只能通过公开的函数来修改。而且很安全。

```
const user = (function() {
  let name = ‘anonymous’; return {
    getName: _ => name,
    setName: newName => name = newName
  };
})();console.log(user.getName()) // anonymous
user.setName(‘Amy’);
console.log(user.getName()); // Amy
```

`name`变量是私有的，这意味着我们只能在`user`生命中访问它。然而，由于我们在这里使用了闭包，我们可以通过暴露`setName()`方法从外部修改它。

# 别名全局变量

使用许多 JavaScript 库可能会导致冲突，因为其中一些库可能会导出同名的对象。

假设您正在使用 jQuery。我们都知道它把出口`$`作为它的主要对象。因此，如果您的依赖项中有任何库也使用`$`作为它的导出对象，就会发生冲突。

幸运的是，您可以通过应用别名技术来使用 IIFEs 解决这个问题:

```
(function ($) {
  // You’re safe to use jQuery here
})(jQuery);
```

通过将您的代码包装在一个以`jQuery`作为参数的 IIFE 中，我们将确保`$`符号现在引用的是`jQuery`，而不是其他库。

# 安全变量范围

[ES6](https://medium.com/javascript-in-plain-english/9-es6-features-every-javascript-developer-should-know-b1f2915e7add) 引入了`let`和`const`来更安全地定义变量。使用`var`可能会导致意外的结果，因为它的作用域很脆弱。

但是如果生产环境还不支持 ES6 呢？还是不知何故不能利用`let`和`const`？

别担心。你还有生命，*立即调用函数表达式*，达到同样的目的。

```
(function () {
  var greeting = ‘Good morning! How are you today?’;
  console.log(greeting); // Good morning! How are you today?
})();console.log(greeting); // error: Uncaught ReferenceError: greeting is not defined
```

正如你在上面的例子中所看到的，在生命范围内发生的事情，都留在生命范围内。你不能从外部使用生命内部定义的变量。

# 循环索引

在循环中执行异步任务可能会导致意外的结果。

以使用`setTimeout()`为例:

```
for (var i = 0; i < 3; i++) {
  setTimeout(_ => console.log(`We’re at ${i}`), 100);
}
```

您期望的结果应该是:

```
We’re at 0
We’re at 1
We’re at 2
```

但它实际上是:

```
We’re at 3
We’re at 3
We’re at 3
```

为什么？因为示例中的`console.log()`调用只会在每个`100ms`之后运行。循环可能在该时间之前结束，这意味着`i`索引实际上已经到达`3`。结果，所有的`console.log()`都会打印出`i`、`3`的最终结果。

我们可以通过将`setTimeout()`电话融入生活来解决这个问题:

```
for (var i = 0; i < 3; i++) {
  (function(index) {
    setTimeout(_ => console.log(`We’re at ${index}`), 100);
  })(i);
}
```

现在结果是正确的:

```
We’re at 0
We’re at 1
We’re at 2
```

不过，这是 ES6 的时代，我们可以使用`let`关键字来简化代码:

```
for (let i = 0; i < 3; i++) {
  setTimeout(_ => console.log(`We’re at ${i}`), 100);
}
```

# 最终想法

生命是保护瞄准镜的好方法。您可以使用 IIFEs 来防止全局变量的定义问题、别名变量、保护私有数据，以及避免使用导出相同对象名的许多库的冲突。

虽然您可以用 ES6 特性替换一些 life 用法，但是您仍然应该学习 life，以便清楚地理解 JavaScript 中的作用域是如何工作的。此外，您不能立即将遗留项目应用到 ES6。所以，生活仍然扮演着重要的角色。

希望你喜欢这篇文章。

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/9-tips-for-writing-scalable-javascript-code-e6bcfc791882) [## 编写可伸缩 JavaScript 代码的 9 个技巧

### 您应该从一开始就准备好扩展您的项目

medium.com](https://medium.com/javascript-in-plain-english/9-tips-for-writing-scalable-javascript-code-e6bcfc791882)