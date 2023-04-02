# JavaScript 模块系统

> 原文：<https://javascript.plainenglish.io/javascript-module-system-in-plain-english-2842ef5c76?source=collection_archive---------6----------------------->

## 简短有用的 JavaScript 课程。让它变得简单

![](img/a92258aa41470d9bebf40f21338058a7.png)

Shutterstock

**模块**只不过是写在文件中的一大块 JavaScript 代码。除非模块文件导出，否则模块中的变量和函数不可用。

两个主要模块系统是

*   [ES6 模块系统](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
*   [常见的](https://requirejs.org/docs/commonjs.html)

# ES6 模块系统

几年前，JavaScript 中还没有模块的概念。不可能在另一个 JavaScript 文件中直接引用或包含一个 JavaScript 文件。ECMAScript 2015 (ES6)引入了这个模块系统。

导出的模块总是处于严格模式。

有三种主要的出口类型:

*   默认导出(每个模块一个)
*   命名导出(每个模块几个)
*   混合出口

## 默认导出

每个文件可以有一个默认导出。当我们导入时，我们必须为它指定一个名称。

```
//--someFunction.js --
export default () => console.log("Hello");//------ main.js ------
//You can use any name you like.
import oneFunc from './someFunction';
oneFunc(); //Hello.....//--someFunction.js --
export default someValue = 10;//------ main2.js ------
//You can use any name you like.
import value from './someFunction'; 
console.log(value) //10
```

## 指定出口

每个文件可以有多个命名导出。导入模块的名称必须与导出模块的名称相同。你必须用大括号把它们括起来。

```
//--someFunction.js --
export const sum = (a,b) => a + b;
export const subtract = (a,b) => a-b;//-- main.js --
import { sum, subtract } from './someFunction';
console.log(sum(1,2)); // 3
console.log(subtract(2, 1)); // 1.....//Import all the named exports onto an object
//-- main.js --
import * as myObject from './someFunction';
console.log(myObject.sum(1,2)); // 3
console.log(myObject.subtract(2, 1)); // 1
```

## 混合默认和命名导出

```
//--someFunction.js --export default () => console.log("Hello"); **//*1**
export const sum = (a,b) => a + b; **//*2**
export const subtract = (a,b) => a-b; **//*2**//-- main.js --
import sayHello, {sum, subtract } from ./someFunction***1\. You to specify a name for the default export.You can use any 
    name you like.
*2\. You have to import them surrounding by braces.**
```

## 摘要

默认导出

*   导入时可以使用任何名称
*   导出单个值/函数/类

指定出口

*   您可以导出多个值/函数/类
*   导入时必须使用导出的名称

# CommonJS

[**CommonJS**](https://requirejs.org/docs/commonjs.html)**:**是 web 浏览器外部的 JavaScript 的模块系统。在 **Node.js 中使用。**它们让你封装功能，并将其暴露给其他 JavaScript 文件，比如库。

```
//--someFunction.js --
exports.sum = (a,b) => a+b
exports.a = 1
exports.b = 2//-- main.js --
const someModule = require('./someFunction.js')
const {a, b} = require('./someFunction.js')console.log(someModule.sum(1,2) //3
console.log(a, b) //1,2
```

在下一篇文章中，我将解释如何在 Node.JS 中启用 ECMAScript 6 导入。

## 如果这对你有帮助，请点击下面的拍手按钮。非常感谢！