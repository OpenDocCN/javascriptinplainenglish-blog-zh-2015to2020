# JavaScript 中的函数和回调

> 原文：<https://javascript.plainenglish.io/functions-and-callbacks-in-javascript-5be5f9ce785c?source=collection_archive---------6----------------------->

## 函数简介，声明和调用函数，JavaScript 中的回调函数(有代码示例)

![](img/77e74c551f748c8220c2d84aab23d5d7.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是函数？

> **函数**只是为执行特定任务而编写的一段代码。函数保持代码的组织性，在需要的时候可以重复使用。因此，它们使我们的应用程序更加灵活。

# 让我们用一个例子来理解

想象一下，您正在构建一个*计算器*，它可以执行所有基本的算术运算，比如-*加法*、*减法*、*乘法*和*除法*。我们将把所有这些不同的操作分解成单独的**函数**，这样每个任务(或操作)都将有自己的代码块，可以重用。

# 在 JavaScript 中定义和调用函数

有很多方法可以在 JavaScript 中定义和声明函数语句。然而，声明函数的常用语法是使用`function`关键字。

```
**function** function_name(*param1, param2, param3,...*){
  *// body goes here*
}
```

上面的代码片段是在 JavaScript 中定义函数的例子。它由`function`关键字后跟*函数名*组成(可以是任何您选择的名称)。函数可以由一些参数组成，如 *param1，param2，param3，…* 所示，最后是位于大括号`{ }`内的函数体，其中包含函数必须执行的语句。

一个函数一旦被声明，就可以在我们的应用程序中的任何地方被调用，只需要提供*函数名*以及参数的值，也被称为函数*参数。*

```
function_name(*arg1, arg2, arg3,...*);
```

函数通常以`return`语句结束。return 语句停止函数 a 的执行，并从该函数返回一个**值**。

***给定半径值，返回圆的面积—***

```
**function** calcAreaOfCircle(radius){     

  **var** area = 3.14 * radius * radius;
  **return** area;             
}calcAreaOfCircle(5);
calcAreaOfCircle(10); 
```

***以下为更多关于函数运算的例子—***

introduction to functions

# 回调函数

回调函数只是作为参数传入另一个函数的常规函数。在 JavaScript 中，回调在用户必须与网站交互时被广泛使用(通过点击事件、表单提交事件、鼠标点击和鼠标悬停事件)。

```
**function** greet(name){
  **alert**("Hey, " + name);    
}**function** userInput(callback){ 
  **var** name = prompt("Enter your name");
  callback(name);
}userInput(greet);
```

尝试在浏览器控制台中执行上述代码片段，并让浏览器用漂亮的问候文本向您问候。😄

上面的脚本是一个*同步*回调的例子。当调用`userInput`函数时，它需要另一个函数/回调函数作为参数(如函数声明所示)，浏览器首先提示您“输入您的名字”。该名称存储在变量`name`中，后面是回调函数，该函数将`name`作为参数。因为回调函数只不过是上面声明的函数`greet`。将显示带有问候消息的警报。

***让我们用函数和回调的概念来构建一个基本的计算器—***

```
**function** add(a, b){
  **return** a + b;
}**function** sub(a, b){
  **return** a - b;
}**function** mult(a, b){
  **return** a * b;
}**function** div(a, b){
  **return** a / b;
}**function** calculate(a, b, operation){
  **return** operation(a, b);
} calculate(5, 10, add);         
calculate(5, 10, sub);
calculate(5, 10, mult);
calculate(5, 10, div);
```

让我们来理解上面的代码片段。首先，我们为两个数的 **a** 和 **b** 的*加法、*减法、*乘法*和 *d* 除法创建了`add`、`sub`、`mult`和`div`四个函数。后来我们声明了一个函数`calculate` ，它以`a`、`b`和一个回调函数`operation`为参数。现在，在调用函数`calculate`时，我们将 5 和 10 作为两个数字传递，并为我们必须执行的实际操作传递回调函数(`add`或`sub`)。

回调对于**异步编程也是必要的，这包括等待用户请求或向另一个服务器发出请求，然后用该响应做一些事情。**

**`setTimeout`和`setInterval`函数用于**调度任务**，它们接受一个*函数*和*时间间隔(* [*毫秒*](https://en.wikipedia.org/wiki/Millisecond) *)* 作为参数。**

```
****function** printHi(){
  **console**.log("Hi");
}**function** printHello(){
  **console**.log("Hello");
}**setTimeout**(printHi, 5000);   
**setInterval**(printHello, 5000);**
```

**在上面的代码片段中，我们声明了两个函数`printHi`和`printHello`，它们只在控制台上打印“Hi”和“Hello”。 ***尝试在浏览器的控制台中执行上面的代码片段(ctrl +shift + i)。*****

**`setTimeout`函数将`printHi`作为第一个参数，并在 *5000ms(即 5 秒)后在控制台上打印“Hi”。***

**`setInterval`功能将`printHello`作为第一个参数，每隔*5000 毫秒(即 5 秒)的*之后，将在控制台上打印“Hello”。****

# *结论*

*[*函数式编程*](https://en.wikipedia.org/wiki/Functional_programming) 是一种使用函数和回调构建应用程序的编程范式。在这里，功能是王道。函数使程序易于调试。因此，每当应用程序的任何部分出现错误时，我们可以直接进入该功能块，检查出了什么问题。*

*这概括了 JavaScript 中的函数和回调以及一些操作。不要忘记自己尝试 gists 中提供的示例和代码片段。*

> ***下期见，祝好运，编码愉快！***

# ***用简单英语写的便条***

*你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些信息——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！***