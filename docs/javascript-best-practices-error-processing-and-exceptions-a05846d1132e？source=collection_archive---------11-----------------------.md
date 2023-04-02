# JavaScript 最佳实践—错误处理和异常

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-error-processing-and-exceptions-a05846d1132e?source=collection_archive---------11----------------------->

![](img/788cc972685c06d9241c589f30aca439.png)

Photo by [mostafa meraji](https://unsplash.com/@mostafa_meraji?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但存在问题的代码很容易。

在本文中，我们将研究如何处理 JavaScript 程序中的错误。

# 错误处理的设计含义

如果我们在系统中处理错误，那么我们必须测试我们的错误处理代码。

这是为了确保我们做的一切都是正确的。

如果我们有返回值，那么我们测试返回值。

我们永远不应该忽略错误信息。

如果我们有一致的方法来处理错误，那么我们应该遵循它，这样我们就可以解决错误处理代码不一致和重复的问题。

# 例外

异常是代码将导致错误的代码中的错误传递给表面的方法。

如果我们不想让我们的程序崩溃，那么我们应该捕捉它们，这样我们就可以优雅地处理它们。

在 JavaScript 中，我们可以通过在`Error`的`Error`类或子类上使用`throw`关键字来抛出错误。

为了捕捉错误，我们可以使用`try...catch`来捕捉它们。

如果我们想不管是否抛出异常都运行代码，我们可以将它们放在`finally`子句中。

例如，我们可以编写下面的代码来抛出一个错误:

```
throw new Error('error');
```

为了捕捉由可能导致错误的代码引发的异常，我们编写:

```
try {
  errorThrowingCode();
  //...
} catch (ex) {
  // handdle errors
}
```

其中`errorThrowingCode`可能抛出错误。

在`catch`块之后可以添加一个`finally`子句。

# 使用异常来通知程序的其他部分不应该被忽略的错误

抛出异常的全部意义在于，我们不能忽视它们而不让用户看到它们。

因此，我们应该使用异常来通知程序的其他部分不应该被忽略的错误。

# 只有在真正异常的情况下才会引发异常

即使我们应该为不应该被忽略的事情抛出异常，我们也不应该到处使用它们。

如果我们有太多的异常，那么我们必须到处都有代码来处理异常。

我们不希望这样，所以我们应该考虑对那些必须处理的错误使用异常，以保证程序的正常运行。

异常处理使我们的代码更加复杂，因为我们必须捕捉它们，然后处理它们。

有更好的方法来处理非关键错误，比如检查值和做一些事情，而不是抛出异常。

# 不要用异常来推卸责任

如果错误可以在本地处理，那么我们应该这样做。

在其他地方处理它们只会让我们的代码更加复杂。

# 在异常消息中包含导致异常的所有信息

我们应该在异常中包含有用的信息，这样我们就有足够的信息来调试和修复导致异常出现的问题。

如果我们给每个人更多的信息，以便我们能够处理错误，这只会使处理错误变得更容易。

# 避免空的捕捉块

空的 catch 块是没有用的，所以我们不应该包含它们。

我们必须处理抛出的异常，而不是在捕捉到异常时什么都不做。

如果我们不想做任何事情，我们至少应该记录错误。

![](img/3b0c3778621f036554285c35a8865887.png)

Photo by [mostafa meraji](https://unsplash.com/@mostafa_meraji?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 了解我们的库代码抛出的异常

了解我们的库抛出的异常是很重要的，这样我们就可以在它们出现时处理它们。

未能捕捉到它们将导致我们的程序崩溃，这将不会是良好的用户体验。

# 考虑构建一个集中式异常报告器

创建一个集中的异常报告器使得记录和格式化异常变得轻而易举，因为所有这些都在一个地方完成。

总比在不同的地方分别处理好。

# 标准化我们项目中异常的使用

为了保持一致性，我们应该标准化代码中异常的处理方式，以减少开发人员的认知负担。

我们可以创建特定于项目的异常类，并定义允许抛出异常的代码。

此外，我们可以集中报告和记录。

# 结论

当我们抛出异常时，需要考虑很多事情。

我们必须考虑何时以及如何抛出异常。它们在哪里处理以及我们是否可以创建自己的错误类。

标准化会使我们的工作更容易。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****