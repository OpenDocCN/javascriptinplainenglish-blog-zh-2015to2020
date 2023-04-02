# 理解 JavaScript 中的时间死区

> 原文：<https://javascript.plainenglish.io/understanding-temporal-dead-zone-in-javascript-53a735a682?source=collection_archive---------4----------------------->

![](img/943d1dfdf27af0448ac30c8da0aaf559.png)

Photo by Blake Connally on Unsplash

时态死区是 JavaScript 中一个高级而有趣的话题。为了理解它是什么以及它是如何工作的，我们需要理解 JavaScript 中变量声明的不同方式。

# Var、Let 和 Const

ES6 为该语言带来了许多新特性，其中之一是能够使用不同的关键字定义变量，这些关键字之间有细微的差异([https://www . ECMA-international . org/ECMA-262/6.0/# sec-let-and-const-declarations](https://www.ecma-international.org/ecma-262/6.0/#sec-let-and-const-declarations))。

我们可以用三种方式声明变量

```
var name = "Yomesh";
let surname = "Gupta";
const profession = "Software Engineer";
```

让我们来了解一下这三者的区别。

## 辖域

用最简单的术语来说，作用域就是变量可访问的区域。变量的作用域由其声明的位置控制。让我们考虑一个例子。

```
function init() {
    var firstname = "Yomesh";
    console.log(firstname); // Yomesh
}
console.log(firstname); // Error
```

在上面的代码片段中，变量`firstname`是在函数`init`中声明的，也就是说，我们可以在这个函数中的任何地方获取或设置变量的值。然而，在函数外部访问同一个变量会导致`ReferenceError: firstname is not defined`。

## 范围界定的类型

我们有三种类型的范围界定，即`Global`、`Function`和`Block`范围界定。现在你一定在想这到底意味着什么？别再想了，现在是示范的时候了！

```
// Global Scope
var firstname = "Yomesh";function sayName() {
    console.log(firstname); // firstname is accessible here
}
function nestedSayName() {
    function sayNameNow() {
        console.log(firstname); // firstname is accessible here
    }
    sayNameNow();
}sayName(); // console logs "Yomesh"
nestedSayName(); // console logs "Yomesh"
```

在上面的代码片段中，变量`firstname`是在函数外部声明的。任何在函数之外声明的变量都属于全局范围，并且可以被代码的所有部分访问。

```
// Function Scope
function sayName() {
    var firstname = "Yomesh";
    console.log(firstname); // firstname is accessible here
    function sayNameNow() {
        // firstname is accessible here and we can update it.
        firstname = "Ronaldo";
        console.log(firstname); // Ronaldo
    }
    sayNameNow();
}
sayName();
```

在上面的代码片段中，变量`firstname`在函数`sayName`中声明。它的作用范围是该函数，即它可以在`sayName`函数或任何嵌套函数中使用。所有通过`var`声明的变量都是函数范围的。

```
// Block Scope
function sayName() {
    var condition = true;
    if (condition) {
        let firstname = "Cristiano";
        const surname = "Ronaldo";
        console.log(firstname, surname); // Cristiano Ronaldo
    }
    console.log(firstname, surname); // ReferenceError
}
```

在上面的代码片段中，变量`firstname`和`surname`是在`if`代码块创建的范围内声明的。第一个`console.log`语句正确记录了值，因为变量在该范围内是可访问的。然而，第二个`console.log`抛出一个错误，因为那里根本不存在变量。

## 声明和初始化

变量声明引入了一个新的标识符。JavaScript 解释器在创建阶段创建一个新变量。

```
var name = "Yomesh"; // allowed
var name = "Cristiano"; // allowed
let surname = "Gupta"; // allowed
let surname = "Ronaldo"; // Error
const profession = "Software Engineer"; // allowed
const profession = "Footballer"; // Error
```

通过`var`声明的任何变量都可以被重复声明任意多次。但是，`let`和`const`在各自的作用域内只能声明一次。通过`var`声明的变量有默认值`undefined`。通过`let`和`const`声明的变量是存在的，但是没有值，并且在赋值之前不能被访问(稍后将详细介绍)。

## 绑定到全局对象

```
var firstname = "Yomesh";
let surname = "Gupta";
const profession = "Software Engineer";// Yomesh Gupta Software Engineer
console.log(firstname, surname, profession);// Yomesh undefined undefined
console.log(window.firstname, window.surname, window.profession);
```

在全局范围内通过`var`关键字声明的变量会自动附加到全局对象上，即浏览器中的`window`。而`let`和`const`就不一样了。

## 理解时间死区

JavaScript 引擎分两个阶段运行我们的代码——创建阶段和执行阶段。在第一阶段，引擎检查代码并为变量分配内存。

```
console.log(firstname); // undefined
var firstname = "Yomesh";
```

在同一个阶段，通过`var`声明的变量被赋予值`undefined`，这就是为什么在上面的代码片段中，你将得到变量`firstname`的值为`undefined`。这也就是我们所说的“**吊装**”。

```
console.log(firstname); // ReferenceError
let firstname = "Yomesh";
```

对于通过`let`和`const`声明的变量，它们也被分配了内存，但最初并未赋值`undefined`，即变量声明确实被提升，但它们在初始化之前会抛出一个错误。让我们考虑一个例子。

```
/*
  Since variables are hoisted
  it would be equivalent to

  let firstname; Beginning of the temporal dead zone*/
console.log(firstname); // ReferenceError as accessed in the TDZfunction init() {
...
}function processing() {
...
}var surname = "Gupta";
var profession = "Software Engineer";
let firstname = "Yomesh"; // Ending of the temporal dead zoneconsole.log(firstname); // Yomesh
```

如你所见，变量`firstname`存在一个区域(时间死区),从变量被提升的地方开始，直到变量被初始化的地方。时间死区是指出代码中错误的好方法。在声明之前访问变量通常是错误的根本原因。

在执行阶段，变量被赋予它们的实际值，以后使用它们是完全可以的。

```
// Accessing `name` here before execution phase assigns the value to the variable would throw a ReferenceError due to TDZ.
// console.log(name);const name = "Yomesh";console.log(name); // Yomesh | Perfectly fine to use here
```

时间死区无处不在。我们考虑了与变量声明相关的例子，但它也存在于像`Default Parameters`这样的新特性中。

我希望这篇文章在某种程度上帮助了你，让你更加清晰。请在评论中或点击[此处](https://www.twitter.com/yomeshgupta)让我知道你的观点。为了测试你的知识，试试下面的问题:[https://code . dev tools . tech/questions/s/what-would-be-the-output-based-on-temporal-dead-zone-qid-adxxcpud 7 lzndjbcjyo 8](https://code.devtools.tech/questions/s/what-would-be-the-output-based-on-temporal-dead-zone---qid---aDXxcPUD7LZNdjBCJYo8/?ref=medium-post-tdz)