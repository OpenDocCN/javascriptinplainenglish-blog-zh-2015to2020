# JavaScript 最佳实践—变量

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-variables-e2f05af4dc55?source=collection_archive---------8----------------------->

![](img/40258bae065e0c51a309cd0a36aaea5b.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将探讨在 JavaScript 代码中定义和使用变量的最佳方式。

# 总是使用`const`或`let`来声明变量

我们应该用`let`和`const`分别声明变量和常量。

这样，我们得到了块范围的变量和常量，所以我们不会遇到变量或常量在我们不期望的地方被访问的问题。

没有严格模式，使用`let`和`const`也会阻止我们创建全局变量。

全局变量污染了全局名称空间，我们的代码越复杂，我们的全局变量在不同地方有相同名称的可能性就越大。那么会因此发生意想不到的行为。

也防止我们在它被设置后重新分配它的值。`let`和`const`都防止我们用相同的名字声明一个变量两次。

例如，我们可以用`let`和`const`声明变量和常量，如下所示:

```
let a = 1;
const b = 2;
```

在上面的代码中，我们用 let 和常量`b`声明了一个变量`a`。

那么我们就不必担心它们在其他地方被声明，或者在`b`的情况下，它的值被重新分配给其他东西。

# 每个变量或赋值使用一个`const`或`let`声明

我们应该把我们的每个`let`和`const`声明放在它们自己的行中。

例如，不用编写以下代码在一行中声明多个变量:

```
const a = 1,
  b = 2,
  c = 3;
```

我们应该将`const`关键字放在每个声明的前面，如下所示:

```
const a = 1;
const b = 2;
const c = 3;
```

这样，我们知道上面代码中的每一行都是用`const`声明的。在第一个例子中，我们不确定它们是如何声明的。

此外，由于我们可以剪切、复制和粘贴整行，所以交换它们所在行中的变量更容易。

我们也可以用调试器单步执行每一行，而不是一次查看所有的声明。

# 将所有的声明组合在一起，然后将所有的声明组合在一起`let Declarations Together`

我们应该将所有的`const`声明组合在一起，然后将所有的`let`声明组合在一起。

如果我们把它们分散到各处，那么我们以后就很难找到它们了。

此外，我们可能需要根据之前分配的变量来分配一个变量。

例如，如果我们有以下代码:

```
const a = 1;
let foo;
const b = 2;
let bar;
const c = 3;
```

那么就很难读取和跟踪哪个变量被赋给了什么。相反，我们应该这样写:

```
const a = 1;
const b = 2;
const c = 3;let foo;
let bar;
```

更有条理。

![](img/3e18b4fbc12a6964b44b08ef947f8437.png)

Photo by [John Duncan](https://unsplash.com/@jrduncan11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在我们需要的地方分配变量，但是把它们放在一个合理的地方

我们应该在需要的地方分配变量，因为`let`和`const`变量是块范围的，这些是我们应该有的变量和常量。

用`var`声明的变量是函数作用域的，并且被提升了，所以它们可以在我们不希望它们被访问的地方被访问。

例如，我们应该如下放置变量:

```
const getGreeting = () => 'hi';const greet = (name) => {
  if (!name) {
    return '';
  } const greeting = getGreeting();
  return `${greeting} ${name}`;
}
```

在上面的代码中，我们在`greet`函数中使用了`greeting`常量。我们把它放在那里，这样我们就不必运行不必要的`getGreeting`函数。

我们只在`name`为真值时运行`getGreeting`函数，这样我们就不必在每次调用时都运行它。

因此，我们没有将`greeting`声明的代码放在 or `greet`函数的第一行，如下所示:

```
const getGreeting = () => 'hi';const greet = (name) => {
  const greeting = getGreeting();
  if (!name) {
    return '';
  } return `${greeting} ${name}`;
}
```

只有当我们需要使用变量和常量时，才声明它们会更有效。

# 结论

当我们声明变量时，我们应该总是使用`let`或`const`来只声明块范围的变量和常量。

`const`和`let`变量应分组到各自的组中。此外，应该只在需要时才声明它们。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****