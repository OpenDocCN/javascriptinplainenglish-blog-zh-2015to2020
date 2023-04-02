# 保持代码整洁的 19 个简单 JavaScript 编码标准

> 原文：<https://javascript.plainenglish.io/19-simple-javascript-coding-standards-to-keep-your-code-clean-7422d6f9bc0?source=collection_archive---------1----------------------->

## 提高代码质量的 JavaScript 编码标准

![](img/a5ced09a3f4e524cf02446aa3aabb84d.png)

Photo by [Autri Taheri](https://unsplash.com/@ataheri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编码标准可以在以下方面提供帮助:

*   保持代码的一致性
*   更容易阅读和理解
*   更易于维护

下面的编码标准是我自己对以上几点的看法。

## **比较时用===代替==**

这很重要，因为 JavaScript 是一种动态语言，所以使用`==`可能会给你意想不到的结果，因为它允许类型不同。

*失败:*

```
if (val == 2)
```

*通过:*

```
if (val === 2)
```

## **千万不要用** `**var**` **用** `**let**` **代替**

一个简单的方法，使用`let`将有助于解决 JavaScript 中`var`导致的任何范围问题。

*失败:*

```
var myVar = 10;
```

*通过:*

```
let myVar = 10;
```

## **用** `**const**` **代替** `**let**`

这阻止了开发人员试图改变他们不应该做的事情，并且确实有助于可读性。

*失败:*

```
let VAT_PERCENT = 20;
```

*过关:*

```
const VAT_PERCENT = 20;
```

## **始终使用分号(；)**

虽然这在 JavaScript 中是可选的，因为分号不像其他语言那样需要作为语句终止符。使用`;`确实有助于保持代码的一致性，对于语句分隔符来说也是一个很好的选择。

*失败:*

```
const VAT_PERCENT = 20;let amount = 10
return addVat(amount, vatPercent)
```

*通过:*

```
const vatPercent = 20;
let amount = 10;
return addVat(amount, vatPercent);
```

## **JavaScript 中的命名约定**

`let` 应该是**茶包**

`const`如果在一个文件的顶部使用大写蛇形。`MY_CONST`。如果不在文件顶部，使用`camelCase`

`class`应该是**帕斯卡套管。** `MyClass`

`functions`应该是`camelCase`。`myFunction`

## **接触字符串时使用模板文字**

模板文字是允许嵌入表达式的字符串文字。

*如果你需要支持 IE11 或更低版本，他们不支持字符串文字*

*失败:*

```
let fullName = firstName + " " + lastName;
```

*通过:*

```
let fullName = `${firstName} ${lastName}`;
```

## **尽可能使用 ES6 箭头功能**

箭头函数是书写函数表达式的一种更简洁的语法。它们是匿名的，并且改变了函数中的绑定方式。

*失败:*

```
var multiply = function(a, b) {
  return a* b;
};
```

*通过:*

```
const multiply = (a, b) => { return a * b};
```

## **总是用花括号把控制结构括起来**

所有控制结构(即`if`、`else`、`for`、`do`、`while`以及任何其他结构)都需要支撑

这可以用一行`if`语句来划分意见，但是当不使用花括号时，这已经导致了多次错误。这可能导致开发人员在下面添加其他语句，以为条件语句围绕着它。

*例如*

```
if (myNumber === 0)
   doSomething();
   doSomethingElse();  // This will always run!!// This is actually what is happeningif (myNumber === 0) {
    doSomething();
}
doSomethingElse();
```

事情应该是这样的:

*失败:*

```
if (valid)
   doSomething();if (amount > 100) 
    doSomething();
else if(amount > 200)
    doSomethingElse();
```

*通过:*

```
if (valid) {
   doSomething();
}if (amount > 100) {
   doSomething();
} 
else if(amount > 200) {
    doSomethingElse();
}
```

## 另外，确保 JavaScript 中的花括号在同一行开始，中间有空格。

*失败:*

```
if (myNumber === 0)
{
    doSomething();
}
```

通过:

```
if (myNumber === 0) {
    doSomething();
}
```

## **尝试减少嵌套**

`if`中的`if`会变得混乱，很难读懂，非常快。有时你可能不能到处走走，但总是要看看你的 JavaScript 的结构，看看你是否能改变它。

*失败:*

```
if (myNumber > 0) {
  if (myNumber > 100) {
       if (!hasDiscountAlready) {
           return addDiscountPercent(0);
       } else {
           return addDiscountPercent(10);
       }
  } else if (myNumber > 50) {
    if (!hasDiscountAlready) {
       return addDiscountPercent(5);
    }
  } else {
    if (!hasDiscountAlready) {
      return addDiscountPercent(0);
    } else {
      return addDiscountPercent(1);
    }
  }
} else {
     error();
}
```

*通过:*

```
if (myNumber <= 0) {
   return error;
}if (!hasDiscountAlready) {
    return addDiscountPercent(0);
}if (myNumber > 100) { 
    return addDiscountPercent(10);
}if (myNumber > 50) { 
    return addDiscountPercent(5);
}return addDiscountPercent(1);
```

从上面的例子中你可以看出，这更容易阅读。大多数时候，如果可能的话，我通常会尽量避免别人的。另一个建议是先做好检查。像下面这样:

*失败:*

```
if (valid) { 
   return buy();
} else { 
   return error();
}
```

*通过:*

```
return valid ? buy() : error();
```

## **试着想出一个文件和函数的“最大”行长度**

这可能很难，因为每个应用程序都是不同的，但我认为当您认为代码的某些部分变得太大时，有一个指南是很重要的。这可能需要一点实验来找到适合你的长度。

*示例:*

`files`最多 80 行代码

`functions`最多 15 行代码

## **小写文件名**

`MyFile.js`应该是`myFile.js`

## **尽可能使用默认参数**

在 JavaScript 中，如果你在调用一个函数时没有传入一个值到一个参数中，它将会是`undefined`

*失败:*

```
myFunction(a, b) {
  return a + b;
}
```

*通过:*

```
myFunction(a = 0, b = 0) { 
   return a + b;
}
```

## **切换报表应使用** `**break**` **并有** `**default**`

我通常不鼓励使用 switch 语句，但是如果你真的想使用它们，请确保对每个条件都使用了`break`并使用了`default`。

*失败:*

```
switch (myNumber)
{
  case 10: 
   addDiscountPercent(0);
  case 20: 
   addDiscountPercent(2);
  case 30:
   addDiscountPercent(3);
}
```

*通过:*

```
switch (myNumber)
{
  case 10: 
    addDiscountPercent(0);
    break;
  case 20: 
    addDiscountPercent(2);
    break;
  case 30:
    addDiscountPercent(3);
    break;
  default: 
    addDiscountPercent(0);
    break;
}
```

## **使用** `**named**` **出口**

不要使用`default`出口。导入模块必须给这些值命名，这可能会导致模块间命名的不一致。

*失败:*

```
export default class MyClass {
```

*通过:*

```
export class MyClass {
```

## **不使用通配符导入**

这确保您有一个默认的导出。

*失败:*

```
import * as Foo from './Foo';
```

*过关:*

```
import Foo from './Foo';
```

## **使用布尔值的快捷方式**

*失败:*

```
if (isValid === true)if (isValid === false)
```

*通过:*

```
if (isValid)if (!isValid)
```

## 试着避免不必要的三元语句

*失败:*

```
const boo = a ? a : b;
```

*通过:*

```
const boo = a || b;
```

## **每个声明只使用一个变量**

这一次可能会再次出现意见分歧，但我觉得一次宣布多个意见，有时会被遗漏。

*失败:*

```
let a = 1, b = 2;
```

*通过:*

```
let a = 1;
let b = 2;
```

# **结论**

任何语言的编码标准都有助于提高应用程序的可读性和可维护性。如果你在团队中工作，有一件事可能很难，那就是执行编码标准。以下是一些有帮助的想法:

*   案头审查，逐行检查代码。
*   林挺或者某种代码分析器
*   创建一致的应用程序，每个开发人员都知道他们需要创建/更新什么。
*   当创建新的东西时，让你的一个高级开发人员开始，因为其他开发人员可以使用该代码作为指导