# JavaScript 错误——无用的承诺代码和操作

> 原文：<https://javascript.plainenglish.io/javascript-mistakes-useless-promise-code-and-operations-d4a22debed04?source=collection_archive---------7----------------------->

![](img/ecdb2ed98b71a3cd166bb96283ac35a7.png)

Photo by [Shivam Kumar](https://unsplash.com/@krishnadigitalcolorphotostudio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在这篇文章中，我们将会看到一些多余的代码，我们应该把它们从我们的代码中删除，放在我们的异步函数和其他地方。

# 没有不必要的`return await`

在 JavaScript 中，异步函数总是返回承诺。如果我们在那里返回一些东西，我们返回一个承诺，它的解析值就是我们在承诺中返回的值。

`return await`在承诺解决或拒绝之前，除了增加额外时间之外，不做任何事情。

因此，我们可以直接退回承诺。

`return await`唯一有效的用法是在`try...catch`语句中捕捉另一个返回承诺的函数的错误。

例如，下面的代码是不好的:

```
const foo = async () => {
  return await Promise.resolve(1);
}
```

正如我们不需要`await`来回报承诺一样。

我们可以直接写:

```
const foo = async () => {
  return Promise.resolve(1);
}
```

不过下面使用`return await`还是不错的:

```
const foo = async () => {
  try {
    return await Promise.resolve(1);
  } catch (ex) {
    console.log(ex);
  }
}
```

# 没有脚本 URL

拥有`javascript:` URL 是一种不好的做法，因为它让我们面临与`eval`相同的性能和安全问题。

它们都允许我们从字符串中运行任意代码，所以攻击者也可以这样做。

此外，从字符串运行代码的性能较慢，因为 JavaScript 解释器不能优化字符串中的代码。

例如，我们应该编写如下代码:

```
location.href = "javascript:alert('foo')";
```

相反，只需将它放入 JavaScript 代码中，如下所示:

```
alert('foo');
```

# 没有自我分配

自我分配是没有用的。因此，它不应该出现在我们的代码中。

这可能是我们在修改代码时没有发现的错误。

例如，以下内容是无用的:

```
a = a;
[a, b] = [a, b];
```

相反，我们应该这样写:

```
a = b;
[a, b] = [b, a];
```

# 没有自我比较

将变量与其自身进行比较总是会返回`true`。因此，在我们的代码中使用这些类型的表达式是没有用的。

这通常是键入或重构代码时出现的错误。这可能会在我们的代码中引入运行时错误。

唯一适合将一个值与其自身进行比较的时候是在比较`NaN`的时候，因为`NaN`与`===`运算符比较时不等于自身。

但是最好使用`isNaN`或`Number.isNaN`功能来检查`NaN`是否存在。

在检查`NaN`之前，`isNaN`试图将它的参数转换成一个数字，而`Number.isNaN`只是检查值而不做任何数据类型转换。

因此，我们不应该有如下代码:

```
const a = 1;
if (a === a) {
    x = 2;
}
```

# 不要使用逗号运算符

逗号运算符总是返回由运算符分隔的项目列表中的最后一个元素，从左到右计算它们。

因此，在我们的代码中使用它不是一个非常有用的操作符。

我们可能应该从代码中删除对它的任何使用。大概是这样的:

```
const a = (1, 2, 3);
```

将始终将`a`设置为 3。

在正常程序中，这个操作符没有任何有效的用例。

![](img/5f4ba94d517330d666daab0f4a20671a.png)

Photo by [Daiji Umemoto](https://unsplash.com/@daijiumemoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在抛出表达式中只抛出错误对象

在 JavaScript 中，当我们试图抛出错误时，我们可以在`throw`后放置任何值。

然而，抛出除了`Error`对象或从它派生的子构造函数之外的任何东西都不是好的做法。

`Error`对象自动跟踪`Error`对象的构建和起源。

例如，我们应该有这样的`throw`表达式:

```
throw "fail";
```

相反，我们应该写:

```
throw new Error("fail");
```

# 结论

`return await`除了`try...catch`块，大部分都没用。将变量或值与其自身进行比较是没有用的，就像将变量赋值给自身一样。

同样，逗号操作符也是无用的，因为它总是返回列表中的最后一个值。

抛出异常的错误应该总是抛出一个`Error`对象或者它的子构造函数的一个实例。

在 JavaScript 中编写 URL 也是一种不好的做法，因为我们在一个字符串中运行代码，这阻止了任何优化的完成，并且可能让攻击者任意运行我们的代码。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！为我们的新出版物献上一点爱心吧，请跟随他们:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****