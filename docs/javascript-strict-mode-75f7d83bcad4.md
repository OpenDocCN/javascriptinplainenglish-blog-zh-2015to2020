# JavaScript“严格模式”

> 原文：<https://javascript.plainenglish.io/javascript-strict-mode-75f7d83bcad4?source=collection_archive---------5----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/a92258aa41470d9bebf40f21338058a7.png)

ES5 在 ECMAScript 5 中添加了一个新特性，称为语言的“严格模式”，它约束了某些行为的规则。这种严格的上下文避免采取某些动作，并抛出更多的异常和错误。这些限制使代码更安全，并符合更合适的指导原则。

严格模式**在不同方面很有用**:

*   它禁用令人困惑或棘手的功能。
*   它捕捉一些典型的编码错误，抛出异常。
*   当使用相对“不安全”的动作时，它会抛出错误。例如不允许隐式自动全局变量声明省略 var。

**它通过检测更多可能导致执行时间错误的东西，帮助你减少错误**。

你可以对单个函数或整个文件使用严格模式**，这取决于你把严格模式语句放在哪里。**

## 在文件级别

file.js

```
"use strict";
 ... 
 // this code is strict modefunction f1() {
         ... 
        // this code is strict mode
        function f2() { 
           // this code is strict mode
        } 
}
```

## 在个人职能层面

```
function f1() {
         "use strict";
        // this code is strict mode
        function f2() { 
           // this code is strict mode
        } 
}// this code is not strict mode
...
...
```

## 一些典型用途:

在这里，如果您执行下面的代码，您会得到一个引用错误，因为变量“myVar”没有定义。在没有严格模式的情况下，这可以工作，但是变量“myVar”的范围将隐含地是全局范围，这是一个不好的做法。

```
**"use strict";**function myFunction(){
          "use strict";
           // turn on strict mode
           myVar **= 1**;
}myFunction(); // ReferenceError: myVar is not defined at myFunctionsomeVar = "This is some var"; //Unresolved variable or type someVar 
```

严格模式允许提升，但不会改变这一点:

```
**"use strict";**
myVar = 1;
console.log(myVar) //1, is OK.
var myVar;
```

在这里，如果我们不使用严格模式，我们可能会得到**不正确的结果**，即使一切看起来都很好:

```
//Without strict mode
function sum(a, a, c) {
   return a + a + c;   //Wrong if this code ran
}
sum(1,2,3)// **7 ??!!** //With strict mode
**"use strict";**
function sum(a, a, c) {
     'use strict';
      return a + a + c; 
 } // Duplicate parameter name not allowed in this context
```

在这段代码中，当在没有 new 的情况下调用 Person 构造函数**并抛出错误 **:** 时，这是未定义的**

```
"use strict";
function Person(name) {
    this.name = name;
}

var me = Person("Nicholas");//Uncaught TypeError: Cannot set property 'name' of undefined
  at Person. 
```

## 结论

如果严格模式在你的程序中引起问题，这可能是你在你的程序中使用了坏习惯的标志，你应该改正它。

**严格模式对代码来说是一个很大的改进，你应该在你所有的程序中使用它**。无论你是独自工作还是和更多人一起工作。

## 如果这对你有帮助，请点击下面的拍手按钮。非常感谢！