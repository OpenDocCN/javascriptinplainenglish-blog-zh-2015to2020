# JavaScript 面向对象编程简介

> 原文：<https://javascript.plainenglish.io/object-oriented-programming-in-javascript-688c13b988a1?source=collection_archive---------4----------------------->

![](img/a538a86fa262c56fd6aa9278f3e61149.png)

Photo by [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

OOP 是编程中广泛使用的概念。除了 C 语言，几乎所有的现代语言都遵循 OOP 原则。这个概念与类和对象有关。 ***类*** 是一个对象的设计或蓝图，它定义了核心属性和功能。 ***对象*** 是类的实例，其中包含属性和方法。JavaScript 不是基于类的面向对象语言。但是它仍然有使用面向对象编程(OOP)的方法。

我们如何在 JavaScript 中实现一个类和一个对象？

为了更好理解，我们来看一个例子。为了创建一个电影类，在初始化实例时，我们需要电影的名字和导演。因此，该类将如下所示。

[https://gist.github.com/GAierken/a2ba3678e9745bb97c440ee26bd45e36](https://gist.github.com/GAierken/a2ba3678e9745bb97c440ee26bd45e36)

在我们定义了类之后，当我们创建电影实例时，就像上面例子中的“titanic”,“Titanic”对象将具有“name”和“director”属性，我们可以调用它们来获得我们想要的结果:titanic.name 将为我们提供“Titanic ”,而 titanic.director 将为我们显示“James Cameron”。

除了类和对象，面向对象编程有四个原则:

*   包装
*   抽象
*   遗产
*   多态性

## ***封装***

封装描述了在一个单元中捆绑数据和处理该数据的方法的思想，例如 JavaScript 中的类。为了让它听起来简单，它被用来隐藏一个对象的内部表示，或者从外部隐藏它的状态。

假设我们创建了另一个电影对象，并希望控制台记录一个描述。在作用域之外无法访问名称和控制器。数据是隐藏的，封装在电影里。

[https://gist.github.com/GAierken/57445f0072b33ae9891dd9a39d4d3b7e](https://gist.github.com/GAierken/57445f0072b33ae9891dd9a39d4d3b7e)

## ***抽象***

抽象是一种隐藏实现细节，只向用户显示功能的方式。换句话说，它忽略了不相关的细节，只显示所需的细节。在下面的示例中，我们将 name 和 director 设置为私有变量，将 description 设置为私有方法。类方法可以调用类范围内的私有方法，但是在我们调用“titanic”实例的时候并没有显示细节的实现。

[https://gist.github.com/GAierken/78d266500f9a4f860584c4d574d8cbd3.js](https://gist.github.com/GAierken/78d266500f9a4f860584c4d574d8cbd3.js)

## ***继承***

在经典继承中，基类(父类)的方法可以被复制到派生类(子类)中。在 JS 中，通过使用原型对象来支持继承。这允许开发人员重用公共逻辑，同时仍然保持独特的层次结构。通过“扩展”，Employee 类从 Person 类继承了构造函数和 toString 方法。

[https://gist.github.com/GAierken/240714c249517994e92f93c7d01e3b1a](https://gist.github.com/GAierken/240714c249517994e92f93c7d01e3b1a)

## ***多态性***

如名称所示:“poly”=“multi”和“morphism”=“form”，多态性提供了一种以多种形式执行单个动作的方法。该程序将确定每次执行该对象时需要哪些含义或用法，从而减少重复代码的需要。在下面的例子中，所有的电影都有相同的功能和不同的形式。

[https://gist.github.com/GAierken/5c69346d0dd6fc00784ce38d594d18c4](https://gist.github.com/GAierken/5c69346d0dd6fc00784ce38d594d18c4)

这些是 JavaScript 中面向对象的基本概念和例子。我希望这能帮助你理解面向对象的基础知识。；)

## *资源*

[](https://searchapparchitecture.techtarget.com/definition/object-oriented-programming-OOP#:~:text=Object%2Doriented%20programming%20%28OOP%29%20is%20a%20computer%20programming%20model,has%20unique%20attributes%20and%20behavior.) [## 什么是面向对象编程？

### 面向对象编程(OOP)是一种围绕数据组织软件设计的计算机编程模型。

searchapparchitecture.techtarget.com](https://searchapparchitecture.techtarget.com/definition/object-oriented-programming-OOP#:~:text=Object%2Doriented%20programming%20%28OOP%29%20is%20a%20computer%20programming%20model,has%20unique%20attributes%20and%20behavior.) [](https://www.geeksforgeeks.org/introduction-object-oriented-programming-javascript/) [## JavaScript-geeks 中的面向对象编程简介

### 极客的计算机科学门户。它包含写得好，想得好，解释得好的计算机科学和…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/introduction-object-oriented-programming-javascript/) 

## **简单明了的 JavaScript**

你知道我们有三个出版物和一个 YouTube 频道吗？在 [**找到所有链接！**](https://plainenglish.io/)