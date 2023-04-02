# […].forEach(saveFromZombies)并不总是救人

> 原文：<https://javascript.plainenglish.io/foreach-savefromzombies-does-not-always-save-people-217108c6921d?source=collection_archive---------5----------------------->

## JavaScript 中 forEach 工作方式的奇怪之处

![](img/c4361ff5b675d7135ef4ab3ebb0e2235.png)

Walking dead!!

## 第一次遇到这个问题。

我像往常一样编码。我遇到了一个关于`forEach`如何工作的奇怪问题。下面的代码给出了一个例子。假设下面的代码是我正在使用的某个库中的代码:

我在我公司的应用程序中发现了这样的代码:

当然，我不想这样。这是违反干燥原则的。所以我将代码重构为:

更酷更简洁。

但是我被骗了，以为这样就可以了。看看这段代码给出了什么:

哎呀。所有人都死了。好的。现在你开始明白为什么了。

是装订的问题。让我们检查一下`this`在我们的代码中到底做了什么:

嗯，不奇怪。它输出:

好的。所以这篇文章的要点是:

> ***只是传入一个函数的引用，使用*** `***this***` ***引用某个地方而不是某个*** `***globalThis***` ***，变成*** `***forEach***` ***可能会导致 javascript 错误，因为*** `***this***` ***会指向一个全局*** `***this***` ***。***

那我们该怎么办？这里有一些事情让你知道:

# 1.显式调用该函数

是的，这个管用。这会救一些人的命。

普通`function`也一样:

# 2.[传](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#Syntax) `[thisArg](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#Syntax)` [作引数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#Syntax)

这些是`forEach`的参数:

你可以在`saveFromZombies`里面放入被指向的对象`this`。

# 3.使用绑定

这是一个显式绑定。您告诉 javascript 引擎，您希望将`saveFromZombies`绑定到`walkingDead`对象。

# 进一步地..

当然，我们可以也应该在处理`map`、`filter`问题时运用同样的原则，...还有更多。

# 摘要

*   我们已经看到了当我们将一个函数的引用作为对`forEach`的回调时`this`可能会丢失上下文。
*   解决方案是:(1)显式调用函数，(2)将`thisArg`作为参数传递，(3)使用`bind`。

快乐`forEach`编码！

[*最初发表于 https://9oelm.github.io/2019-10-12-[...].forEach(say hello)-does-not-always-say-hello/*](https://9oelm.github.io/2019-10-12--[...].forEach(sayHello)-does-not-always-say-hello/)