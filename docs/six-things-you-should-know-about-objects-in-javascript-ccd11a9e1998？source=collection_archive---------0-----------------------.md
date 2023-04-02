# 关于 JavaScript 中的对象，你应该知道的六件事

> 原文：<https://javascript.plainenglish.io/six-things-you-should-know-about-objects-in-javascript-ccd11a9e1998?source=collection_archive---------0----------------------->

![](img/141f27a3c2375b749b08e6bfb63d3ed5.png)

# 1.对象方法& *本*

*   方法只是保存函数值的属性

## **—简单对象方法**

产量为

```
The rabbit says 'I'm alive.'
```

## —对象方法& this

*   当一个函数作为方法被调用时——作为属性被查找并立即被调用，如在`object.method()`中——函数体中的特殊变量`this`将指向被调用的对象

产量为

```
The white rabbit says 'Oh my ears and whiskers, how late it's getting!'
```

## —应用和呼叫

*   `apply` & `call`可用于`object.method()`
*   `apply` & `call`方法都采用第一个参数，该参数可用于模拟方法调用
*   第一个参数实际上用于给`this`赋值

产量为

```
The white rabbit says 'Burp!'
The old rabbit says 'Oh my.
```

# 2.原型

*   几乎所有的物体都有一个`prototype`
*   原型是另一个用作属性后备来源的对象
*   当一个对象请求一个它没有的属性时，将搜索它的原型来查找该属性，然后是原型的原型，依此类推

## —空对象的原型

*   空物体的原型是伟大的祖先原型，几乎所有物体背后的实体，`Object.prototype`

产量为

```
[Function: toString]
[object Object]
```

## —其他对象的默认属性(数组、函数……)

*   许多对象没有直接将`Object.prototype`作为它们的原型，但是有自己的默认属性
*   函数来源于`Function.prototype`数组来源于`Array.prototype`

产量为

```
true
true
```

## — Object.create 创建具有特定原型的对象

*   *原型兔子*充当所有兔子共享的属性的容器
*   单个 rabbit 对象，如 killer rabbit，包含仅适用于自身的属性(在本例中为其类型),并从其原型派生共享属性

产量为

```
The killer rabbit says 'SKREEE!'
```

# 3.构造器

## —构造器原型

*   创建从共享原型派生对象的更方便的方法是使用**构造函数**
*   在 JavaScript 中，调用前面带有`new`关键字的函数会导致它被视为构造函数
*   构造函数将把它的`this`变量绑定到一个新对象，除非它显式返回另一个对象值，否则这个新对象将从调用中返回
*   用`new`创建的对象被称为其构造函数的`instance`
*   将构造函数的名字大写是一种惯例，这样可以很容易地将它们与其他函数区分开来

产量为

```
black
```

## —默认情况下，构造函数具有 Object.prototype

*   构造函数(实际上是所有的函数)自动获得一个名为`prototype`的属性，默认情况下，它保存一个从`Object.prototype`派生的普通的空对象
*   使用此构造函数创建的每个实例都将此对象作为其原型

产量为

```
The black rabbit says 'Doom...'
```

# 4.覆盖派生属性

## —相同的原型名称

*   如果原型中有一个同名的属性，这个属性不会改变
*   属性被添加到对象本身
*   `console.log(blackRabbit.teeth)`打印`small`是因为`blackRabbit`对象没有`teeth`属性，它继承了`Rabbit`对象自身的`teeth`属性，即`small`

# 5.原型干扰

## —可计数与不可计数

*   `toString`没有出现在`for/in`循环中，但是`in`操作符为其返回了`true`
*   这是因为 JavaScript 区分了**可枚举的**和**不可枚举的**属性
*   我们通过简单地给它们赋值而创建的所有属性都是可枚举的
*   `Object.prototype`中的标准属性都是不可数的，这就是为什么它们没有出现在这样的`for/in`循环中

产量为

```
pizza
touched tree
nonsense
true
true
```

*   使用`Object.defineproperty`功能可以定义我们自己的不可数属性，这允许我们控制我们正在创建的属性的类型
*   在这个例子中，属性在那里`map.hiddenNonsense`，但是它不会在一个循环中出现

产量为

```
pizza
touched tree
hi
```

## — hasOwnProperty 与中的对象

*   `hasOwnProperty`方法告诉我们对象**本身**是否具有该属性，而无需查看其原型
*   这通常比`in`操作符给我们的信息更有用

产量为

```
true
false
```

*   因此，当您担心有人可能已经破坏了基于对象的原型时，我建议您像这样编写您的`for/in`循环

# 6.无原型对象

*   `Object.create`函数允许我们创建一个具有特定原型的对象
*   你可以传递`null`作为原型来创建一个没有原型的新对象
*   因此，我们不再需要`hasOwnProperty`,因为对象拥有的所有属性都是它自己的属性
*   现在我们可以安全地使用`for/in`循环，不管人们对`Object.prototype`做了什么