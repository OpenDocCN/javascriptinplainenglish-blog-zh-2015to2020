# JavaScript 基础:函数

> 原文：<https://javascript.plainenglish.io/javascript-basics-functions-44bee1c31846?source=collection_archive---------8----------------------->

![](img/d6a500a930b85270dd4f833fb282f033.png)

在编程中，反复做一个任务是很常见的。假设您想要将一系列数字相加:

```
let num1 = 4;
let num2 = 5;
let answer = num1 + num2;let num3 = 7;
let num4 = 12;
let answer = num3 + num4;let num5 = 8;
let num6 = 2;
let answer = num5 + num6;
```

如果你想将数字相加十次，那么重写相同的代码块十次可能并不可行。相反，你可以写一个**函数**。

函数的作用是允许你只写一次代码块，这样你就可以在整个程序中重用它。函数内部是一系列执行特定任务的语句。通过编写函数，您不必重复编写相同的代码块。如果您发现自己编写了相同的代码块，并且它们都在做相同的事情，那么最好为它创建一个函数。

如何编写一个函数？让我们看下面的例子:

```
function sayHello(){
    console.log("Hello");
}
```

要声明一个函数，你需要在函数名前写下关键字`function`。在这个例子中，函数的名字是`sayHello`，后跟括号。花括号内是你写你想让你的函数做什么的地方。在这种情况下，我们希望我们的`sayHello`函数控制台日志“你好”。

为了使函数工作或运行，您必须调用它。要调用这个函数，你需要做的就是键入函数名，后跟括号。为了调用我们的`sayHello`函数，我们这样做:

```
sayHello(); // console logs "Hello"
```

我们的`sayHello`功能只有控制台日志文本。如果你的函数想要基于输入执行动作怎么办？每次声明函数时，都可以添加参数。换句话说，您可以允许函数接受输入，函数将使用该输入执行任务。

在我们的`sayHello`例子中，当我们声明我们的函数时，我们没有在括号内放置任何东西。要声明一个带参数的函数，你需要在括号内写下输入的名字。

```
function addNums(num1, num2){
    console.log(num1 + num2);
}
```

在文章的开头，我们反复添加了两个数字，一遍又一遍地重写同一个代码块。如果我们想写一个将两个数相加的函数，我们可以创建一个名为`addNums`的函数，我们的参数将是我们想要相加的两个数。我们的参数`num1`和`num2`充当我们将加在一起的值的占位符。

为了调用带参数的函数，我们在括号中写下我们的参数。参数是要传递给函数的值。因此，我们调用`addNums`函数并包含我们的参数，而不是重写我们的加法方程:

```
addNums(4,5); // console logs 9
addNums(7,12); // console logs 19
addNums(8,2); // console logs 10
```

对于上面的例子，第一个数字被分配给`num1`参数，第二个数字被分配给`num2`参数。如果你将这个例子与我们在开始时如何将两个数字相加进行比较，你会发现代码更容易处理和维护。如果您的计算中有错误，您可以在函数中修复它，而不是在您复制和粘贴的每个代码块中修复错误。

就如何创建、使用和调用 JavaScript 函数而言，这就是全部的基础知识。要查看更多最新的 JavaScript 文章，您可以查看以下文章:

[](https://levelup.gitconnected.com/javascript-basics-switch-statement-12f8c6082326) [## JavaScript 基础:Switch 语句

### 当编写 if 语句时，您可能希望包含几个其他 if 语句，但是如果您希望包含一个…

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-basics-switch-statement-12f8c6082326) [](https://levelup.gitconnected.com/javascript-basics-ternary-operator-c5e1510c6d2) [## JavaScript 基础:三元运算符

### 在编写 if 语句时，也有一种使用三元运算符来缩短它的方法。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-basics-ternary-operator-c5e1510c6d2)