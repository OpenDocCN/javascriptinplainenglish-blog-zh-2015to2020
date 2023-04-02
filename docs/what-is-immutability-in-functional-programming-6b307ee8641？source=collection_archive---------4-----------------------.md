# 函数式编程:TypeScript 中的不变性

> 原文：<https://javascript.plainenglish.io/what-is-immutability-in-functional-programming-6b307ee8641?source=collection_archive---------4----------------------->

![](img/9152e27343389b5e59b3142a90841578.png)

Do not mutate the state!

## 什么是不变性和不可变数据，如何在 Java、JavaScript 库、React.js、React Native 中使用它，为什么要使用它

在面向对象和函数式编程中，如果一个对象在创建后不能被修改，就称为不可变。

**那么，这为什么如此重要呢？**

# 我在掩盖什么

在动机一节中，我提到了回答这个问题的一些原因，接下来我用一个例子解释了如何不改变 JavaScript 中的数据(这很简单)，最后，我用一些众所周知的事实证明了 Java、C#和 React.js 等 JavaScript 库的不变性。

# TL；速度三角形定位法(dead reckoning)

**什么是不变性？**

你不能改变一个不可变的数据，你只需要复制它，然后更新新的数据。

不变性就是一切。

函数式编程通常避免使用`mutable`状态。

# 动机

**为什么在函数式编程中使用不变性很重要？**

**这里是不变性给你的主要优势:**

## **时间旅行调试**

TTD 是通过代码回溯时间的过程，不变性增强了这个过程中的状态推理，

## **性能提升**

如果你有一个对象，改变了它的一个属性，javascript 需要很长时间才能意识到属性已经被改变，它需要检查所有的对象属性，通过不变性，它已经知道应用了什么更新，通过状态版本之间更便宜的比较，

> **隔离失败**
> 
> 你有一个线程运行在一个临界区，访问共享状态，如果出错了会发生什么？您需要终止所有访问共享内存的线程，因为您不知道终止的进程在共享内存中留下了什么状态，所以基本上您会丢失那里的所有内容。在不可变状态下，如果你丢失了某个进程，你只是丢失了那个特定进程的状态，其他所有进程都不会受到影响。
> 
> **可变性的局部性问题**
> 
> 你在哪里定位你的可变状态，你有一个在诺丁汉运行的进程和一个在伦敦运行的进程，它们需要共享内存，你在哪里共享内存？假设你在莱斯特有你的共享内存，如果网络瘫痪会发生什么，但是不变性，每个都有自己的副本，当网络备份时，你只需要再次同步数据。
> 
> [*Francesco Cesarini，Erlang Solutions 的技术总监*](https://www.youtube.com/watch?v=8Sf6ToPNiA4)

# 走查

## **什么是不变性，如何使用它？**

**在 JavaScript** 中，有些内置类型(数字、字符串)是不可变的，但是自定义对象一般是可变的。

考虑一下这个:

```
let ***immutableString*** = "I'm an Immutable value";
```

**这是否意味着一个新值不能赋给一个不可变的对象？**

这要视情况而定，但在这个例子中，它绝对是一个字符串，它可以被赋给一个新值，但一个新的内存引用将被分配给它。

```
let ***immutableString*** = "I'm an Immutable value";
console.log(***immutableString***); // I'm an Immutable value
let ***newImmutableString*** = ***immutableString***.replace("an Immutable", "a Mutable");
console.log(***immutableString***); // I'm an Immutable value
```

*可以看出* `immutableString` *之所以不变是因为顾名思义是不可变的。(这里我用了* `let` *来说明，如果我用* `const` *也不妨碍它是可变的，所以用* `const` *来代替)*

如果 JavaScript 中的字符串是可变的，它会有不同的行为，我使用了[可变字符串](https://github.com/christophehurpeau/mutable-string)库，它会给你可变字符串，下面是演示:

```
const MutableString = require('mutable-string');
let ***mutableString*** = new MutableString("I'm an Immutable value");
console.log(***mutableString***.toString());  // I'm an Immutable value
let newMutableString = ***mutableString***.replace('an Immutable', new MutableString("a Mutable"));
console.log(***mutableString***.toString());  // I'm a Mutable value
```

与 JavaScript 中的字符串相反,`mutableString`被就地改变。

但是对于 JavaScript 中的对象和数组来说，对对象/数组进行更改将通过引用传递它们，而不是复制它们:

```
let ***mutableArray*** = [1, 2, 3, 4, 5];
console.log(***mutableArray***); // [ 1, 2, 3, 4, 5 ]
let ***updateMutableArray*** = ***mutableArray***.push(6);
console.log(***mutableArray***); // [ 1, 2, 3, 4, 5, 6 ]
```

*当我们把一个新值推送到* `mutableArray` *时，它会被就地更新，而不是被新的复制。*

如果你想让它不变呢？

你可以通过使用一个不可变数组来使它不可变，例如来自 [immutable.js](https://github.com/immutable-js/immutable-js) 的 List。

```
const {List} = require('immutable');let ***immutableList*** = List([1, 2, 3, 4, 5]);
console.log(***immutableList***.toArray()); // [ 1, 2, 3, 4, 5 ]
let ***newImmutableList*** = ***immutableList***.push(6);
console.log(***immutableList***.toArray()); // [ 1, 2, 3, 4, 5 ]
```

*当我们将一个新值推送到* `immutableList` *时，它不会原地改变，而会为此创建一个新的更新引用。*

如果你不想使用不可变的列表或数组呢？

如果你不想使用一个不可变的列表，你就不应该在 JavaScript 中使用破坏性的函数，而是使用非破坏性的函数来完成你的工作。我们可以这样更新我们的`mutableArray`示例:

```
let ***mutableArray*** = [1, 2, 3, 4, 5];
console.log(***mutableArray***); // [ 1, 2, 3, 4, 5 ]
let ***updatedMutableArray*** = [...***mutableArray***, 6];
console.log(***mutableArray***); // [ 1, 2, 3, 4, 5 ]
```

*这种方式下* `mutableArray` *不在原地变化。*

但是不变性并不总是一个好的选择，考虑这个例子:

```
const ***a*** = [1, 2, 3, 4, 5];
let ***immutableSum*** = 0;
for (let ***i*** = 0; ***i*** < ***a***.length; ***i*** += 1) {
  ***immutableSum*** += ***a***[***i***];
  console.log(***immutableSum***);
}
```

这是输出:

```
1
3
6
10
15
```

*随着循环的进行，每个循环创建一个 immutableSum 的新副本，如果这个数组包含 1000 个项，则创建了 1000 个副本。所以要确定你想从他们那里得到什么样的行为。*

# 其他演示

如果您对其他语言(如 Java)的实现以及 React.js 和 React Native 等框架或库不感兴趣，您可以直接跳到众所周知的事实。

# Java 语言(一种计算机语言，尤用于创建网站)

在 Java 中`String`类是不可变的:

```
String ***immutableString*** = "I'm an Immutable Java String";
***immutableString***.toLowerCase();
```

在这之后，方法`toLowerCase()`调用`immutableString`仍然和以前一样。

Java 的可变版本`String`是`StringBuffer`和`StringBuilder`。

原始类型变量(`int`，`long`，`short`，...)可以在定义后重新分配。用关键字`final`你可以防止它们被重新分配(以某种方式使它们对变异安全)。

```
int ***primitiveInt*** = 0;
***primitiveInt*** = 1;**final** int ***finalPrimitiveInt*** = 0;
***finalPrimitiveInt*** = 1; *// can't be reassigned*
```

但是它不能阻止对象的属性被重新分配。

```
**final** MyObject ***myObject*** = **new** MyObject();
***myObject.property*** = 0;  *// myObject is mutable*
***myObject*** = **new** MyObject();  *// can't be reassigned*
```

原始包装器(`Integer`、`Long`、`Short`、`Double`、`Float`、`Character`、`Byte`、`Boolean`)也都是不可变的。

# React.js 和 React Native

React 允许您使用任何想要的数据管理风格，包括突变。react 和 react native 中的不变性是通过前面提到的`immutable.js`、`mori`和`immutability-helper`等库实现的。下面是最后一个例子的演示:

```
import update from 'immutability-helper';const immutableArray = [1, 2, 3, 4, 5];
console.log(immutableArray);  // [1, 2, 3, 4, 5]
const immutableArray2 = update(immutableArray, {$push: [6]});
console.log(immutableArray);  // [1, 2, 3, 4, 5]
```

`immutableArray`保持不变。

这就是它的工作原理。以`$`为前缀的按键被称为*命令*。他们正在“变异”的数据结构被称为*目标*。

## 可用命令

*   `{$push: array}` `push()`目标上`array`中的所有项目。
*   `{$unshift: array}` `unshift()`目标上`array`中的所有项目。
*   `{$splice: array of arrays}`对于`arrays`中的每一项，用该项提供的参数调用目标上的`splice()`。*注意:数组中的项目是按顺序应用的，所以顺序很重要。在操作过程中，目标的指数可能会发生变化。*
*   `{$set: any}`完全更换目标。
*   `{$toggle: array of strings}`切换目标对象的布尔字段列表。
*   `{$unset: array of strings}`从目标对象中删除`array`中的键列表。
*   `{$merge: object}`将`object`的按键与目标合并。
*   `{$apply: function}`将当前值传递给函数，并用新的返回值更新它。
*   `{$add: array of objects}`给`Map`或`Set`加一个值。当添加到一个`Set`时，你传入一个要添加的对象数组，当添加到一个地图时，你传入`[key, value]`数组，像这样:`update(myMap, {$add: [['foo', 'bar'], ['baz', 'boo']]})`
*   `{$remove: array of strings}`从`Map`或`Set`中删除数组中的键列表。

如果您在应用程序的性能关键部分使用不可变数据，您可以从`ShouldComponentUpdate()`或其他性能优化中受益，以加速您的应用程序。

# C#

在 C#中，你可以用`readonly`语句来强制一个类的字段的不变性。通过将所有字段强制为不可变的，您获得了不可变的类型。

```
**class** **AnImmutableType**
{
    **public** **readonly** double _value;
    **public** AnImmutableType(double x) 
    { 
        _value = x; 
    }
    **public** AnImmutableType Square() 
    { 
        **return** **new** AnImmutableType(_value * _value); 
    }
}
```

# 众所周知的事实

## JavaScript 中的只读(类型脚本)

为了模拟对象的不变性，可以将属性定义为只读(writable: false)。

```
**var** obj = {foo: ''};
Object.defineProperty(obj, ‘foo’, { value: ‘bar’, writable: **false** });
obj.foo = ‘bar2’;  *// silently ignored*
```

然而，上面的方法仍然允许添加新的属性。或者，可以使用 Object.freeze 使现有对象不可变。

```
**var** obj = { foo: ‘bar’ };
Object.freeze(obj);
obj.foo = ‘bars’; *// cannot edit property, silently ignored*
```

在来自`defineProperty`方法或`freeze`的 React 中，它引发了一个 TypeError，并且不能赋给只读属性。

## **不可变变量**

常量和只读字段是不可变的变量。

## **弱对强的不变性**

如果一个对象状态的一些字段不可改变并且是不可变的，那么其他可以改变的字段称为*弱不可变的。*如果所有字段都是不可变的，那么对象也是不可变的。如果整个对象不能被另一个类扩展，这个对象被称为*强不可变。*

## **线程安全**

在多线程应用程序中，每个线程都可以处理由不可变对象表示的数据，而不用担心数据被其他线程更改。因此，不可变对象被称为线程安全的。

# 摘要

为了让代码更可预测，推理起来更安全，你必须使用不变性而不是可变性，例如，你有一个数组`[1, 2, 3, 4, 5]`，有人用破坏性的`push(6)`推给它，突然你的数组发生了变异，所以你不能确定你的状态是否安全。通过使用不可变的数组，比如来自 immutable.js 的`List([1, 2, 3, 4, 5])`,如果有人想用库的`push(6)`方法推它，他/她会制作数组的副本，你的数组仍然是安全的。

# 进一步阅读

如果你对函数式编程感兴趣，并希望**让你的代码更简洁、更可组合、更适合测试**，我有另一篇关于函数式编程的文章[。](https://medium.com/@zareanmasoud/devmade-curry-ing-powder-recipe-in-functional-programming-c6e0e45cfbae)