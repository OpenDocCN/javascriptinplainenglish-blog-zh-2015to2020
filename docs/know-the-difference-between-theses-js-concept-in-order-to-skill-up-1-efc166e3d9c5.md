# 了解这些关键 JavaScript 概念之间的区别

> 原文：<https://javascript.plainenglish.io/know-the-difference-between-theses-js-concept-in-order-to-skill-up-1-efc166e3d9c5?source=collection_archive---------14----------------------->

![](img/a3bdfc12299399523a998d84de15105f.png)

嘿！在本文结束时，您将会提高您的 JavaScript 技能，因为您知道了一些开发人员经常询问的许多关键 JS 概念之间的区别！

# Spread vs Rest 运算符

运算符相同(`...`)，但用法不同。

`spread operator`用于自变量函数，它允许我们为该函数设置无限个参数。

```
// rest parameter is handle as array in the function
const add = (...rest) => {
   return rest.reduce((total, current) => total + current)
}

// Nice your function can handle different number of parameters !
add(1, 2, 3)
add(1, 2, 3, 4, 5)
```

`rest operator`有另一个用途，它允许从`array`或`object`提取值，例如:

```
// We extract the first Item of the array into the variable and the others variable in an array named others
const [ firstItem, ...others ] = [ 1, 2, 3, 4 ]
firstItem // 1
others // [ 2, 3, 4 ]
```

我们可以用物体来制造它

```
const toto = { a: 1, b: 2, c: 3 }
const { a, ...others } = toto
a // 1, we extract the a key from toto object
others // { b: 2, c: 3 }, we extract other key in the object thanks to rest operator
```

# 无效合并(`??` ) vs OR 运算符(`||`)

作为第一印象，我们可以理解 Nullish 合并(`??`)操作符类似于 OR ( `||`)操作符。但事实并非如此！

`Nullish`将检查该值是`null`还是`undefined`，

还有…

`OR`操作员将检查该值是否为`falsy`。

没完全明白吗？让我们在下面的代码中检查一下！

```
const foo = 0 ?? 42
const foo2 = 0 || 42

foo // 0 since nullish check if value is null or undefined
foo2 // 42 since OR operator will check if the value is falsy, in this example it's the case

const toto = null ?? 42
const toto2 = null || 42

toto // 42 since nullish check if value is null or undefined
toto2 // 42 since OR operator will check if the value is falsy, in this example it's the case (null is a falsy value)
```

# 双倍等于(`==` ) vs 三倍等于(`===`)

`double equals`将**校验值，而不是类型**，但如果类型不同，它将制作一个`implicit coercion`，以便将值转换为相同的类型并校验值。

`implicit coercion`的机制不太好理解，但你可以在我之前的文章[*https://dev.to/codeozz/implicit-coercion-in-javascript-neh*](https://dev.to/codeozz/implicit-coercion-in-javascript-neh)中详细查看

`triple equals`将检查数值和类型！而且也不会做出双等的隐性强制。

总的来说，我建议你总是使用三重等号！

下面的代码将说明不同之处:

双等于示例:

```
// Not the same type so implicit coercion will be made
'24' == 24

// Convert string into number so 
Number('24') == 24

// We got an number so we can check value
24 == 24 // true !
```

三倍等于示例:

```
// Not the same type and triple equal will not make an implicit coercion !
'24' === 24 // false
```

# var vs let vs const

面试中常见的问题。

我将把这个解释分成三个部分:

# 一)范围

声明是全局作用域或函数作用域的。

```
if (true) {
  var timeVar = 56
}
console.log(timeVar) // 56
```

这可能是危险的，因为你可以删除现有的变量

```
var timeVar = 24
if (true) {
  // the first variable is erased
  var timeVar = 56
  console.log(timeVar) // 56
}
console.log(timeVar) // 56
```

`let` & `const`声明都是块作用域的。

```
if (true) {
  let timeLet = 56
  const timeConst = 98
  console.log(timeLet) // 56
  console.log(timeConst) // 98
}
console.log(timeLet) // Error: timeLet is not defined in this scope since it's only block scope

console.log(timeConst) // Error: timeConst is not defined in this scope since it's only block scope
```

# II)重新声明和更新变量

`var` **能否重新声明**并更新

```
// can be re-declared
var timeVar = 56
var timeVar = 'toto'

// can be updated
timeVar = 'tutu'
```

**`let` **不能重新声明**和更新**

```
**// can't be re-declared
let timeLet = 56
let timeLet = 'toto' // Error: Identifier 'timeLet' has already been declared

// can be updated
timeLet = 'tutu'**
```

****`const` **不能重新声明**并且**不能更新******

```
**// can't be re-declared
const timeConst = 56
const timeConst = 'toto' // Error: Identifier 'timeConst' has already been declared

// can't be updated
timeConst = 'tutu' // TypeError: Assignment to constant variable.**
```

# ****III)吊装****

****有些人不知道 JavaScript 中的`Hoisting`。这篇文章我就不跟你解释这是什么了，只是吊装手柄`var`、`let`和`const`的方式不同。****

```
**// They are all hoisted to the to of their scope.
// But while var variable are initialized with undefined
// let and const variables are not initialized.

myVarVariable // undefined
myLetVariable // Error since not initialized by Hoisting, you cannot use it's declaration
myConstVariable // Error since not initialized by Hoisting, you cannot use it's declaration

myVarVariable = 5
myLetVariable = 'letValue'
myConstVariable = 'constValue'

var myVarVariable
let myLetVariable
const myConstVariable**
```

******总的来说，我建议你一直使用** `**const**` **(用于常量值)** `**let**` **用于其他用途**。****

****希望你喜欢这篇读物！****

****🎁你可以免费获得我的新书《javascript 中被低估的技能》,《改变现状》,如果你在推特上关注我并给我打电话的话😁****

****或者在这里得到它****

****🎁[我的简讯](https://www.getrevue.co/profile/code__oz)****

****☕️你可以[支持我的作品](https://www.buymeacoffee.com/CodeoZ)🙏****

****🏃‍♂️，你可以跟着我👇****

****🕊 [推特](https://twitter.com/code__oz)****

****👨‍💻 [Github](https://github.com/Code-Oz)****

****你可以标记🔖这篇文章！****

*****更多内容请看*[***plain English . io***](http://plainenglish.io/)****