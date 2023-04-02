# JavaScript 数据类型中你需要知道的 5 件事

> 原文：<https://javascript.plainenglish.io/5-things-you-need-to-know-in-javascript-data-types-c8e50fd87811?source=collection_archive---------4----------------------->

## 深入了解 JavaScript 数据类型

![](img/c9c50c85c762805144fbbf2f732cd0d2.png)

Photo by [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

虽然 JavaScript 数据类型是程序员最熟悉的概念。但仍有 5 件事值得深入探讨。

*   “void 0”、“null”和“undefined”的区别
*   *String.length* 是真实长度吗？
*   比较浮点的正确方法。
*   JavaScript 中的类型转换
*   在 JavaScript 中检查数据类型

# “void 0”、“null”和“undefined”的区别

## 不明确的

[表格 MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined#:~:text=undefined%20is%20a%20property%20of,In%20modern%20browsers%20(JavaScript%201.8.&text=A%20method%20or%20statement%20also,not%20have%20an%20assigned%20value.) :

> `undefined`是*全局对象*的一个属性。也就是说，它是全局范围内的变量。`undefined`的初始值就是原始值`[undefined](https://developer.mozilla.org/en-US/docs/Glossary/Undefined)`。

默认情况下，名为`undefined`的全局属性的值为`undefined`。这个属性可以修改到 ES5，那时它变成了只读的。

## 空

来自 MDN:

> 值`null`用一个文字写:`null`。`null`不是全局对象属性的标识符，就像`[undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)`一样。相反，`null`表示缺乏认同，表示一个变量没有指向对象。

与`undefined`不同，默认情况下，它不会被分配给 JavaScript 中的任何内容。您只能手动设置`null`的值。

## 无效

来自 MDN:

> `void`运算符通常仅用于获取`undefined`原始值，通常使用“`void(0)`”(相当于“`void 0`”)。在这些情况下，可以使用全局变量`[undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)`。

## 结论

因为在 JavaScript 代码中`undefined`是一个变量。尽管`undefined`在 ES5 中被设为只读，我们仍然可以在局部范围内修改它的值。如下面的代码。

所以，为了避免无意的篡改，我们建议使用`**void 0**` **来获得** `**undefined**` **值。**

`null`只有一个值`null`，其语义表示空值，与`undefined`不同的是，`null`是 JavaScript 关键字，因此您可以在任何代码中安全地使用`null`关键字来获取`null`值。

我们还应该注意到`null`和`undefined`具有相同的值，但是类型不同。

![](img/2fb0efe312214462e5e75f4d526ca571.png)

Photo by [Emily Morter](https://unsplash.com/@emilymorter?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# *String.length* 是真实长度吗？

JavaScript 使用的字符串格式 UTF-16 使用单个 16 位代码单元来表示最常用的字符，但对于不常用的字符需要使用两个代码单元，因此由`length`、`charAt`和`charCodeAt`返回的值可能与字符串中的实际字符数不匹配。尤其是对于处理非英语字符串的操作。例如，您可以使用下面的代码来计算中文和英文混合字符串的长度。该函数通常用于输入字符长度受限的情况。

ECMAScript 2016 (ed。7)建立了`2^53 - 1`元素的最大长度。以前，没有规定最大长度。在 Firefox 中，字符串的最大长度为`2**30 - 2` (~1GB)。在 Firefox 65 之前的版本中，最大长度是`2**28 - 1` (~256MB)。

# 比较浮点的正确方法。

你知道吗，在 JavaScript 中，0.1 + 0.2 不等于 0.3。这是一个典型的丧失精度的例子。如果要计算 0.1 + 0.2 的结果，计算机会先把 0.1 和 0.2 转换成二进制，然后相加，最后把相加的结果转换成十进制数。两次转换都会出现精度损失。所以得出的结果并不准确。

所以测试浮点相等的正确方法是使用`Number.EPSILON`

![](img/d54a73b104467d9c724a637f065a2868.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# JavaScript 中的类型转换

在这一节中，我们重点关注数字和字符串之间的转换规则，以及对象和基本类型之间的转换规则。

## 将字符串转换为数字

字符串转换成数字有三种方式，`Number()`，`parseInt()`，`parseFloat()`。

**Number():** 将数字字符串和 null 转换为数字。只要字符串采用正确的十进制、二进制、八进制和十六进制表示法，以及正确的符号科学记数法，就可以转换成数字。如果处理失败，返回`NaN`。

**parseInt(** string [，radix]**):**`parseInt`函数将其第一个参数转换为字符串，解析该字符串，然后返回一个整数或`NaN`。

如果不是`NaN`，返回值将是第一个参数的整数，作为指定`radix`中的一个数字。(例如，`10`的一个`radix`由十进制数转换而来，`8`由八进制转换而来，`16`由十六进制转换而来，以此类推。)

**parse float(string):**`**parseFloat()**`函数解析一个参数(如果需要，首先将其转换为字符串)并返回一个浮点数。

大多数情况下，`Number()`是比`parseInt()`和`parseFloat()`更好的选择。如果使用`parseInt()`，建议随时传入第二个参数。

## 将数字转换为字符串

还有两种方法可以将**数字**转换成**字符串**`**toString()**`**`**String()**`**。****

****toString(**【radix】**)**:`**toString()**`方法返回一个表示指定`Number`对象的字符串。**

****String()** :全局方法`String()`可以将数字转换成字符串。它可以用于任何类型的数字、文字、变量或表达式。**

## **原始包装对象**

**一个对象有一组方法和属性。我们知道字符串有许多实用方法，比如`substr`、`indexOf`和`replace`。何况他们还有一个`length`属性。很明显，字符串也像对象一样。但字符串不是对象，它不允许您定义自定义方法和属性:**

**这是由于 JavaScript 中的**包装对象**造成的。除了`null`和`undefined`之外，所有的原始值，**数字**，**字符串**，**布尔**，**符号**，都有环绕原始值的对象等价物。**

**当我们像对待对象一样对待一个原始值时(例如，通过访问属性和方法)，JavaScript 在幕后创建一个包装器来包装这个值，并将其作为对象公开。JS 引擎从不重用包装器对象，在使用一次后就将它们交给垃圾收集器。**

**避开`String`、`Number`和`Boolean`物体。它们会使你的代码变得复杂，降低执行速度。**

## **对象转换为原始类型**

**在 [ECMAScript 语言规范](http://ecma-international.org/ecma-262/6.0/#sec-toprimitive)中，指定了`ToPrimitive(input[,PreferredType])`函数，实现从对象到原语类型的转换。**

> **抽象操作 ToPrimitive 接受一个输入参数和一个可选参数 *PreferredType* 。抽象操作 ToPrimitive 将其输入参数转换为非对象类型。如果一个对象能够转换成一个以上的原语类型，它可以使用可选提示 *PreferredType* 来支持该类型。**

**从对象转换为字符串和数字有两个步骤。**

1.  **使用`ToPrimitive`将对象转换为原始值。**
2.  **使用`valueOf`和`toString`将原始值转换为字符串或数字**

**如果`valueOf`和`toString`不存在，或者没有返回原始类型，将抛出`TypeError`异常。**

**如果从对象转换成字符串，先调用`toString`方法，再调用`valueOf`；如果从对象转换成数字，它首先调用`valueOf`，然后调用`toString`。**

**在 ES6 之后，对象可以通过定义一个`@@toPrimitive Symobl`方法来覆盖默认的`ToPrimitive`行为。**

**![](img/f7c711687094f739f4e4aa0b1c394f0a.png)**

**Photo by [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

# **在 JavaScript 中检查数据类型**

**JavaScript 中检查数据类型的方法有四种:`typeof`、`instanceof`、`constructor`、`Object.prototype.toString.call()`。**

## **类型 of**

**`**typeof**`运算符返回一个表示未赋值操作数类型的字符串。结果包括:`number`、`boolean`、`string`、`symbol`、`object`、`undefined`、`function`。**

**然而，在`typeof`的返回和运行时数据类型之间有两个例外。一个是运行时数据类型为`Null`的`typeof null`返回`object`，另一个是运行时数据类型为`Object`的`typeof (function(){})`返回`function`。**

**因为`typeof`的结果与运行时类型不一致，所以我们只有在准确知道变量在`number`、`boolean`、`string`、`symbol`、`undefined`、`function`的范围内时，才能使用`typeof`来检查数据的类型。**

## **实例 of**

**`**instanceof**` **操作符**测试一个构造函数的`prototype`属性是否出现在一个对象的原型链中。返回值是一个布尔值。**

**`instanceof`操作符只测试对象的原型链中是否存在`constructor.prototype`。`instanceof`不能测试原始类型。会返回两个结果:当`primitive type instanceof primitive type`返回异常错误时；当`primitive type instanceof reference type`返回`false`时。**

## **构造器**

**`constructor`方法是`[class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/class)`的特殊方法，用于创建和初始化该类的对象。**

**`constructor`方法类似于`instanceof`，也可以测试原语类型。**

**`constructor`有一个缺点:不能测试`null`和`undefined`，因为`null`和`undefined`中没有`constructor`属性。所以我们在实际项目中很少使用`constructor`来检查数据类型。**

## **Object.prototype.toString.call()**

**`Objec.prototype.toString.call()`是最常用的数据类型检测方法。它将返回`[ojbect type]`，第二个值`type`是`object`的类型。**

## **数据类型检测概述**

1.  **原始类型:`number`、`boolean`、`string`、`symbol`、`undefined`、`function`使用`typeof`确定数据类型。**
2.  **`null`使用`Object.prototype.toString.call()`和`===`检查类型。**
3.  **`Object`使用`instanceof`进行测试。**
4.  **我们可以使用`Object.prototype.toString.call()`来检查以上所有情况。**

# **结论**

**在本文中，我们介绍了 JavaScript 运行时的类型系统。也许即使我们不知道这些知识点，我们仍然可以写出可运行的 JavaScript 代码。但是深入了解这些知识之后，我们就能更好地理解 JavaScript 语言的特性，降低潜在风险，写出更好的代码。**

***感谢阅读，美好的未来可期。***

**![](img/5a42c3c840f5d3ba7c931c86a8801679.png)**

**Photo by [salvatore ventura](https://unsplash.com/@salvoventura?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**