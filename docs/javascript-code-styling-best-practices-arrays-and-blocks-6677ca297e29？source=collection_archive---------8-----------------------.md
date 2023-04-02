# JavaScript 代码样式最佳实践—数组和块

> 原文：<https://javascript.plainenglish.io/javascript-code-styling-best-practices-arrays-and-blocks-6677ca297e29?source=collection_archive---------8----------------------->

![](img/dfca05402e2339ff37fbe5bab74f06a2.png)

Photo by [Heng Films](https://unsplash.com/@hengfilms?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。很容易写出看起来乱七八糟、难以阅读但却能运行的代码。

在本文中，我们将研究一些格式化 JavaScript 代码的最佳实践，以便它们易于阅读和理解。

# 在开始后和结束数组括号前添加换行符

如果数组中有长表达式，我们应该在数组括号的开始和结束之前添加换行符。

例如，如果我们有一个如下所示的短数字数组，那么可以将数组全部放在一行中:

```
const arr = [1, 2];
```

但是，如果我们有更长的数组，如下所示:

```
const arr = [
  [1, 2, 3, 4, 5],
  [1, 2, 3, 4, 5, 6]
];
```

然后，我们应该像上面一样，在左括号之后，右括号之前有一个换行符，这样更容易阅读。

将所有的表达式放在一行会使代码溢出页面，因为数组中有多个长表达式。

既然数组中的每个表达式都在各自的行中，我们可以通过读取每一行来判断数组中的每个条目是什么。

# 在数组的括号内有一些间隔

我们应该在我们定义的数组的括号内留一些空间，这样我们就可以知道哪些条目在数组内。

如果没有空格分隔条目，就很难区分数组条目的开始和结束位置。

因此，不要写这样的话:

```
const arr = ['a','b'];
```

我们应该这样写:

```
const arr = ['a', 'b'];
```

以便更容易区分条目。

# 在长数组元素表达式之间添加换行符。

如果我们有很长的数组元素表达式，那么它们之间应该有一个换行符。

例如，在上面的例子中，我们有:

```
const arr = [
  [1, 2, 3, 4, 5],
  [1, 2, 3, 4, 5, 6]
];
```

两个条目之间有换行符。

这样，我们的数组条目表达式就不会溢出页面，并且更容易区分条目。

对于短数组表达式的数组，我们可以把它们都放在一行。

# 在开始块之后和结束块之前的块内添加空格

通常，我们希望我们的块在左括号后有一个换行符，在右括号前有一个换行符。

单行箭头函数是个例外，因为如果函数只有一行，我们希望利用它的隐式返回特性。

例如，对于像常规函数和`if`块这样的普通块，我们应该写如下:

```
const greet = (greeting) => {
  console.log(greeting);
}if (foo) {
  console.log('foo is true');
}
```

这样，我们可以将函数体与签名区分开，并且可以更容易地看到大括号在哪里分隔块。

一个例外是只有一行的箭头函数。例如，如果我们有:

```
const double = x => x * 2;
```

那么我们应该保持这样，因为我们不想给单行箭头函数添加大括号，这样我们就可以使用它的隐式返回特性。

如果我们不在一行箭头函数中放大括号，我们隐式地返回`x`乘以 2，这样我们可以保持它的简洁。

我们可以通过编写以下代码隐式返回对象:

```
const foo = () => ({
  a: 1,
  b: 2
})
```

对象周围的括号表示我们希望返回它，而不是将大括号视为函数体分隔符。

这样，我们就不用担心那些看起来不干净的块了。

![](img/f2092f98773082691883ebd0c5b04d11.png)

Photo by [Maid Milinkic](https://unsplash.com/@lensilium?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

对于数组，我们应该添加换行符来分隔长表达式。此外，我们应该有空间来分隔条目。

这样，我们的代码易于阅读和理解。我们在条目之间留有足够的空间，以便于区分。

数组中的长表达式也不会溢出我们的页面，所以我们不必水平滚动来阅读我们的代码。

在函数中，除了单行箭头函数，我们应该在函数体的前后添加换行符。

这是因为如果我们没有在块中放大括号，单行箭头函数会隐式返回。

## **简明英语团队的一份说明**

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****