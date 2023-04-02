# 在 Javascript 中提升如何与“let”和“const”一起工作

> 原文：<https://javascript.plainenglish.io/how-hoisting-works-with-let-and-const-in-javascript-725616df7085?source=collection_archive---------2----------------------->

## 频繁采访的概念

![](img/cce7f82dd9aac9cf981ab8cda5a6b8d5.png)

Photo by [Lance Anderson](https://unsplash.com/@lanceanderson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/crane?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 术语“提升”令人困惑

我相信人们难以理解*提升*的首要原因之一是因为这个术语本身有些误导。在韦氏词典，单词“提升”的定义是“提升或举起的动作”。

这可能会让人认为提升涉及到以某种方式对书面代码进行物理重组。这不是真的。

相反，术语*提升*被用作一种比喻来描述 JavaScript 引擎解释编写的 JavaScript 代码时发生的过程。

# JavaScript 代码是如何解释的？

所有编写的 JavaScript 都在编写它的**执行上下文**中解释。当您打开文本编辑器并创建一个新的 JavaScript 文件时，您会创建一个所谓的**全局执行上下文**。

JavaScript 引擎分两个阶段解释在这个全局执行上下文中编写的 JavaScript；**编译**和**执行**。

## 汇编

在编译阶段，JavaScript 解析编写的代码，寻找所有的函数或变量声明。这包括:

```
-let
-const
-class
-var
-function
```

当编译这些关键字时，JavaScript 在内存中为它遇到的每个声明的变量创建一个唯一的空间。这个“提升”变量并在内存中给它一个空间的过程叫做提升。

通常，提升被描述为将*变量*和*函数*声明移动到它们(全局或函数)作用域的顶部。

然而，变量**根本不移动**。****

**实际发生的情况是，在编译阶段，声明的变量和函数在其他代码被读取之前被存储在内存中，因此产生了“移动”到它们的作用域顶部的错觉。**

## **执行**

**在第一阶段完成并且所有声明的变量都被提升之后，第二阶段开始；执行。解释器返回到代码的第一行，再次向下工作，这一次分配变量值和处理函数。**

# **变量是用 let 和 const 来声明的吗？**

**是的，用 let 和 const 声明的变量被提升。它们与提升过程中其他声明的不同之处在于它们的初始化。**

**在编译阶段，用`var`和`function`声明的 JavaScript 变量被提升并自动初始化为`undefined`。**

```
console.log(name) // undefined
var name = "Andrew";
```

**在上面的例子中，JavaScript 首先运行它的编译阶段并寻找变量声明。它遇到`var name`，提升那个变量并自动给它赋值`undefined`。**

**相反，用`let`、`const`和`class`声明的变量被提升，但仍未初始化:**

```
console.log(name); // Uncaught ReferenceError: name is not defined
let name = "Andrew";
```

**这些变量声明只有在运行时被求值时才会被初始化。这些变量被声明和被评估之间的时间被称为**时间死区**。如果您试图在这个死区内访问这些变量，您将得到上面的引用错误。**

**浏览第二个例子，JavaScript 运行其编译阶段并看到`let name`，提升该变量，但不初始化它。接下来，在执行阶段，调用`console.log()`并传递参数`name`。**

**因为变量还没有初始化，所以还没有赋值，因此返回参考错误，说明`name`没有定义。**

# **哪里可以引用`let`和`const`？**

**同样，用`let`和`const`声明的变量只有在 JavaScript 引擎在运行时评估它们的赋值(也称为词法绑定)时才会被初始化。**

**在代码中引用声明上方的`let`和`const`变量不是错误，只要代码没有在声明之前执行。**

**例如，以下代码运行良好:**

**但是，这将导致引用错误:**

**正如我希望你在这一点上理解的那样，产生这个错误是因为在变量`name`被声明之前`greetings()`被执行了。**

## ****用简单英语写的 JavaScript 笔记****

**我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)**