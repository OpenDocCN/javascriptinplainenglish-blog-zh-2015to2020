# JavaScript 最佳实践—声明和使用变量

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-declaring-and-using-variables-fc01a5881fae?source=collection_archive---------9----------------------->

![](img/6f1521b9d7b449732c23fa6372d3e658.png)

Photo by [Mitchell Orr](https://unsplash.com/@mitchorr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但存在问题的代码很容易。

在本文中，我们将研究声明和使用 JavaScript 变量的最佳实践。

# 隐式声明

在 JavaScript 中，如果关闭了严格模式，那么我们可以隐式声明变量。如果它是关闭的，我们可以引用变量而不用声明它，这样不好。

例如，我们可以写:

```
foo = 1;
```

关闭严格模式。这将声明一个全局变量`foo`，它可以在任何地方被访问。

这不好，因为它是在没有使用 do it 的情况下声明的，并且是全局的。

所以我们应该在脚本中打开严格模式，因为这是一个危险的特性。

# 用 let 或 const 声明所有变量

我们应该总是用`let`或`const`来声明所有变量。

使用`let`表示变量，使用`const`表示常量。

常量在声明后不能被重新赋值，我们必须在声明时设置一个值。

例如，我们可以写:

```
let x = 1;
const y = 2;
```

我们也可以写:

```
let x;
```

# 使用命名约定

常用后缀 o 的命名约定，当只需要一个变量时，我们不使用变量。

例如，如果我们使用`Num`作为后缀，那么就坚持用它来表示数字。

# 检查变量名

我们应该检查编辑器和 ide 中列出的变量。然后我们可以去掉不使用的变量。

# 初始化变量的准则

不适当的数据初始化是计算机程序中问题的一大来源。

如果我们正确地初始化它们，我们可以节省大量的调试时间。

如果一个变量从来没有被赋值，那么我们应该给它赋值。

如果它有一个过时的值，那么我们需要更新它。

如果它的一部分已经被赋值，而另一部分没有，那么我们需要设置那个部分。

我们可以通过设置属性或添加数组来实现。

# 在声明时初始化每个变量

我们不必在声明变量时为它们设置值，但是我们应该设置它，因为我们很可能以后会忘记。

此外，它们应该放在使用它们的地方附近，因为如果我们不使用它们，我们可能会忘记使用它们。

这也给人一种在整个代码中使用该变量的印象，这是不可取的。

# 理想情况下，在靠近第一次使用的地方声明和定义每个变量

这使得我们只在需要时才声明变量。

它还让人们知道变量并没有在整个代码中使用。

![](img/f81e07298ed6445f3644405b749220f0.png)

Photo by [Tim Cooper](https://unsplash.com/@tcooper86?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 尽可能使用 const

如果我们使用`const`，那么我们不能给它赋值。

但是，我们仍然可以在内部对对象进行变异。

# 请特别注意计数器和累加器

如果我们有计数器和累加器，那么我们应该记得在必要时再次使用它们之前重置它们。

# 在构造函数中初始化类的成员数据

我们应该在构造函数中初始化所有类的实例变量，这样它们就可以在任何地方使用了。

例如，我们可以写:

```
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

这使得`this.name`和`this.age`在类的所有方法中都有定义。

# 检查是否需要重新初始化

如果一个变量的值需要初始化，那么我们应该使用`let`。否则我们用`const`。

# 初始化一次命名常数；用可执行代码初始化变量

我们应该用`const`声明命名的常量，这样它们就不能被重新赋值。

如果我们有变量，它们应该在函数中使用它的代码之前用`let`或`const`声明，这样我们可以在调用函数时重新初始化它。

# 使用自动初始化所有变量的 Linter 设置

我们可以使用 linter 来检查所有的变量都被初始化了。

这使得过程自动化，以便我们可以避免错误。

# 利用我们的短绒警告信息

像 ESLint 这样的 Linters 有很多规则，我们可以检查以确保我们正确地声明了变量。

因此，我们应该利用它们，以便我们可以在检测到不正确的变量声明时自动修复它们。

# 结论

我们应该正确地声明变量。用棉绒很容易。

隐式声明是不好的，因为它们是全局的。我们应该使用`let`和`const`来声明它们。

严格模式防止隐式变量声明。

## **简明英语笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****