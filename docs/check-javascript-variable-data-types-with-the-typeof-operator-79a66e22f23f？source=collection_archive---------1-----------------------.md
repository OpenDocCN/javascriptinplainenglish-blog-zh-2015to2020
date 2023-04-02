# 使用 Typeof 运算符检查 JavaScript 变量数据类型

> 原文：<https://javascript.plainenglish.io/check-javascript-variable-data-types-with-the-typeof-operator-79a66e22f23f?source=collection_archive---------1----------------------->

![](img/10a312e15883466d4da7c037cc4ed315.png)

Photo by [Stanley Dai](https://unsplash.com/@stanleydai?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种松散类型或动态类型的语言。这意味着用一种类型声明的变量可以转换成另一种类型，而不用显式地将数据转换成另一种类型。变量也可以在任何时候包含任何类型，这取决于赋值的内容。JavaScript 有多种数据类型。有 7 种原始数据类型和一种对象类型。这 7 种基本类型是布尔、空、未定义、数字、BigInt、字符串和符号。因为有不同的数据类型，并且它们一个变量或一个对象的属性类型可以改变任何东西，我们需要一种方法来检查一个变量或一个对象的属性的数据类型。

为了检查对象属性或变量的类型，我们使用对象的`typeof`操作符。它让我们以字符串的形式获取对象的类型名或其属性。我们可以在允许的情况下使用它:

```
typeof x
```

其中`x`是变量的名称。这是`typeof`运算符的唯一操作数。在检查一个对象的属性的类型时，我们可以写:

```
typeof x.prop
```

通过上面的代码，我们可以方便地得到`x`的`prop`属性的类型。`typeof`操作符返回一个包含操作数类型名称的字符串。它可以将以下类型名称作为字符串返回:

*   未定义类型的对象将返回`'undefined'`
*   null 类型的对象将返回`'object'`
*   布尔类型的对象将返回`'boolean'`
*   数字类型的对象将返回`'number'`
*   BigInt 类型的对象将返回`'bigint'`
*   字符串类型的对象将返回`'string'`
*   符号类型的对象将返回`'symbol'`
*   函数类型的对象将返回`'function'`
*   任何其他类型的对象都将返回`'object'`

我们可以像下面的例子一样使用`typeof`运算符:

## 数字

如果对以下操作数应用了`typeof`运算符，则该操作数将返回`'number'`。因此，所有被记录的表达式都将输出`true`。

```
typeof 1 === 'number';
typeof 3.1 === 'number';
typeof(2) === 'number';
typeof Math.LN10 === 'number';
typeof Infinity === 'number';
typeof NaN === 'number'; 
typeof Number('2') === 'number'; 
typeof Number('shoe') === 'number';
```

数字类型是双精度 64 位数字，其值可以在-2 的 53 次方减 1 到 2 的 53 次方减 1 之间。整数没有特定的类型。所有数字都是浮点数。还有 3 个符号值:`Infinity`、`-Infinity`和`NaN`。这些都在范围内或者是`Infinity`、`-Infinity`或`NaN`所以都有`'number'`的类型。`Number(‘shoe’)`返回`NaN`，因为`NaN`是数字类型，`typeof Number(‘shoe’)`是数字类型。

## 用线串

如果对以下操作数应用了`typeof`运算符，它们将返回`'string'`。因此，所有被记录的表达式都将输出`true`。

```
typeof '' === 'string';
typeof 'abc' === 'string';
typeof `template string` === 'string';
typeof '2' === 'string';
typeof (typeof 2) === 'string';
typeof String(1) === 'string';
```

字符串用于表示文本数据。字符串中的每个元素在字符串中都有自己的位置。它的索引是零，所以字符串第一个字符的位置是 0。字符串的`length` 属性有字符串的总字符数。

JavaScript 字符串是不可变的。我们不能修改已经创建的字符串，但是我们仍然可以创建一个包含最初定义的字符串的新字符串。我们可以用`substr()`函数从一个字符串中提取子字符串，并用`concat()`函数连接两个字符串。

在 JavaScript 中，任何被单引号、双引号或反勾号包围的东西都是字符串。普通字符串用单引号和双引号括起来，模板字符串用反引号括起来。`String`函数将任何东西转换成字符串，所以`String(1)`返回`'1'`，所以它的类型是 string。`typeof`操作符返回一个字符串，所以如果我们对`typeof`返回的结果应用`typeof`操作符，那么它应该是一个字符串。

## 布尔运算

如果对以下操作数应用了`typeof`运算符，它们将返回`'boolean'`。因此，所有被记录的表达式都将输出`true`。

```
typeof true === 'boolean';
typeof false === 'boolean';
typeof Boolean(2) === 'boolean';
typeof !!(2) === 'boolean';
```

`Boolean`函数和双非运算符(`!!`)都会将真值转换为`true`，将假值转换为`false`。JavaScript 中的 Falsy 值包括 0、空字符串、`null`、`undefined`和`false`。因此，如果它们作为参数传递给`Boolean`函数，那么它们将被转换为`false`。其他的都是真的，所以用`Boolean` 函数或双 not 运算符将它们转换成`true`。

## 标志

如果对以下操作数应用了`typeof`运算符，它们将返回`'symbol'`。因此，所有被记录的表达式都将输出`true`。

```
typeof Symbol() === 'symbol'
typeof Symbol('foo') === 'symbol'
typeof Symbol.iterator === 'symbol'
```

符号是 ES2015 的新功能。它是唯一且不可变的标识符。一旦创建，就无法复制。每次你创造一个新的符号，它都是独一无二的。它主要用于对象中的唯一标识符。这是一个符号的唯一目的。

`Symbol`函数创建一个新的符号，所以它返回的任何东西都是符号类型的。`Symbol.iterator`是特殊符号，所以也是类型符号。

## 不明确的

如果对以下操作数应用了`typeof`运算符，则该操作数将返回`'undefined'`。因此，所有被记录的表达式都将输出`true`。

```
typeof undefined === 'undefined';
typeof declaredVariableWithoutValueAssigned === 'undefined';
typeof undeclaredVariable === 'undefined';
```

没有赋值的变量具有未定义的类型。如果你没有给一个已声明的变量赋值或者试图引用一个未声明的变量，那么如果你对它们应用`typeof`操作符，你会得到未定义的类型。同样，如果你从未声明过一个对象的属性，或者声明了属性但没有赋值，那么这些属性也将具有未定义的类型。

## 目标

如果对以下操作数应用了`typeof`运算符，它们将返回`'object'`。因此，所有被记录的表达式都将输出`true`。

```
typeof {a: 1} === 'object';
typeof [1, 2, 4] === 'object';
typeof null === 'object';
typeof new Date() === 'object';
typeof /regex/ === 'object';
```

对象是一种引用数据类型，这意味着它可以被指向内存中对象位置的标识符引用。在内存中，对象的值被存储，通过标识符，我们可以访问该值。对象有属性，这些属性是键值对，其值可以包含原始类型的数据或其他对象，这意味着我们可以使用对象来构建复杂的数据结构。

除了具有 object 类型的键值对的数据之外，当对`typeof`操作符进行操作时，`null`类型也返回 object。当我们对数组和其他可迭代对象应用操作符来获取类型时，它们也将 object 作为类型返回，因为这些类型的数据在 JavaScript 中也是对象。为了检查一个对象是否是一个数组，我们可以使用`Array.isArray`函数来确定一个对象是否是一个数组。

## 功能

如果对以下操作数应用了`typeof`运算符，它们将返回`'function'`。因此，所有被记录的表达式都将输出`true`。

```
typeof function() {} === 'function';
typeof class C {} === 'function';
typeof Math.sin === 'function';
```

函数是 JavaScript 中的对象。任何种类的函数，像箭头函数，函数都用关键字`function`声明，类是 function 类型的。注意，JavaScript 中的类只是函数的语法糖，我们可以用它来创建新的对象。因此，类仍然是函数类型。

## 比金茨

如果对以下操作数应用了`typeof`运算符，它们将返回`'bigint'`。因此，所有被记录的表达式都将输出`true`。

```
typeof 42n === 'bigint';
```

在 JavaScript 中，BigInt 类型存储超出安全整数范围的数字。可以通过在数字末尾添加一个`n`字符来创建一个 BigInt 数字。使用 BigInt，我们可以计算出超出正常数字安全范围的结果。所以任何以`n`结尾的数字都是`bigint`类型。

![](img/6ce51dab860a0dabf592c2f841d29ff8.png)

Photo by [Alistair Smith](https://unsplash.com/@eagleeyevisuals?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 新操作员

`new`操作符总是将一个对象返回给它所应用的对象，所以下面的操作符总是有 object 类型。如果对以下操作数应用了`typeof`运算符，它们将返回`'object'`。因此，所有被记录的表达式都将输出`true`。

```
typeof new Boolean(true) === 'object'; 
typeof new Number(1) === 'object'; 
typeof new String('abc') === 'object';
typeof new String(null) === 'object';
typeof new Number(undefined) === 'object';
```

请注意，无论我们拥有什么，如果使用关键字`new`创建对象，我们都会得到类型`object`。不管它是布尔型、数字型、字符串型还是你传入的任何类型，因此，当我们不需要使用`new`关键字来创建对象时，就不要这样做。要将事物转换成布尔型，我们使用`Boolean`函数，要将事物转换成数字，我们使用`Number`函数，要将事物转换成字符串，我们使用`String`函数。

# 空类型

`null`对象属于 object 类型，因为它们由类型标签 0 表示。与大多数平台一样，JavaScript 的空指针是 0x00。因此，`null`有类型标签 0，所以 object 有 object 作为`typeof`返回的类型。

# 确定表达式的类型

我们可以使用`typeof`操作符来确定表达式返回的实体的类型。我们用括号来确定表达式中组合在一起的结果的类型。例如，如果我们想得到`1 + ‘1’`的类型，那么我们需要写:

```
typeof (1 + '1');
```

如果我们写上面的代码，我们得到 string 作为类型，这是表达式结果的正确类型。如果我们不在表达式两边加上括号，那么我们只能得到表达式第一部分的类型，即与`'1'`连接的数字。

# 错误

在用`let`或`const`声明的变量之前使用`typeof`操作符将导致抛出 ReferenceError，因为它引用了一个尚未声明的变量。之前在未声明的变量上使用`typeof`操作符将返回类型`'undefined’`。在 ES2015 之前，`typeof`操作符从未产生错误。

例如，如果我们有:

```
typeof letVariable; 
typeof constant; 
typeof c;

let letVariable;
const constant = 'constant';
class c{};
```

前 3 行将在运行时抛出一个 ReferenceError。

`typeof`操作符对于检查基本类型和对象的类型很有用。对于基本类型，它将返回对象的类型，除了`null` type，它将 object 作为类型返回。对象，包括数组和其他可迭代对象，将总是返回 object 作为类型。对于用`let`或`const`关键字和类声明的变量，我们必须在对它们使用`typeof`操作符之前声明它们。