# JavaScript 中“this”的 5 个实际应用

> 原文：<https://javascript.plainenglish.io/5-practical-applications-of-this-49b0c4880e61?source=collection_archive---------15----------------------->

## 掌握绑定规则，编写高级代码

![](img/230cd2ac410904eaa6376bb6a8ec9e72.png)

Photo by [Tamarcus Brown](https://unsplash.com/@tamarcusbrown?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在编程中，一切都有一个结构和一套规则。声明变量有一种特定的方式，运行循环有一种完全不同的方式。

你对这些语法规则了解得越多，你就是越好的程序员。

JavaScript 中臭名昭著的“this”关键字和绑定也有这样的规则。

如果你使用 JS 已经有一段时间了，你一定注意到了‘this’关键字的使用。

这是这门语言最难的方面之一，主要是因为要完全理解它，你需要掌握许多底层细节。

要开始学习“this ”,你需要先了解什么是词法环境和执行上下文。

我不会脱口而出那些虽然技术上正确，但最终会让读者困惑的行话。

## 词汇环境

每当 JavaScript 引擎执行(运行)一段代码时，它会将这段代码中声明和使用的变量存储在一个名为词法环境的特殊存储中。

无论是运行函数还是运行整个。但是如果你正在运行一个使用如下所示的全局变量的函数呢？

```
var age=10;
function printAge(){
  console.log(age);
 }
printAge()
```

这里“年龄”是一个全局变量，我们在函数`printAge()`中打印它。那么，如果我们只想执行函数，而不想执行其余的代码，该怎么办呢？词汇环境将如何知道变量‘年龄’的值？

词汇环境有两个部分。

1.  **环境记录-** 存储变量和函数声明的地方。
2.  **对外部(父)环境的引用-** 这是存储全局变量引用的地方。

**执行上下文**

顾名思义，执行上下文是代码执行时创建的环境。它引用了所有有助于代码运行的东西，包括分配内存。

它甚至引用了包含所有变量值的词法环境。

换句话说，它包含代码的当前评估状态。

执行上下文是在堆栈中管理的，也就是说，可以有许多这样的环境独立运行。

*你可以在这里* [*找到更详细的解释*](https://stackoverflow.com/questions/35759544/what-is-the-difference-and-relationship-between-execution-context-and-lexical-en) *但是上面说的知识已经足够我们理解‘这个’了。*

# 那么，“这”是什么？

关键字“this”指的是函数所属的对象。我知道这可能会令人困惑，因此我们将再次使用例子来理解。

```
var age = 12;
var bodyProps = {
  age: 10,
  logAge: function () {
    console.log('Inside nested function:', age);
    console.log('Inside nested function using this:', this.age);
  },
  statement: console.log('Inside statement function:', this.age),
};
bodyProps.logAge();
console.log('Outside function:', age);
```

输出:

```
Inside statement function:12Inside nested function:12Inside nested function using this:10Outside function:12
```

如你所见，通过上面的代码片段,“this”的定义将变得更加清晰。

每当我们使用“this”时，我们就获得了对该函数所属对象的引用。

在我们的例子中，对象是全局范围，它有一个值为 12 的“年龄”变量。

变量`bodyProps`的词法环境包含‘age’变量值‘10 ’,然而当我们写`this.age`时，它引用函数所属的对象(在我们的例子中),并使用‘age’变量值(即值 10)。

简单地说，关键字“this”指的是函数所属的对象。

这是一个使用“This”的非常简单的例子。但是，掌握这个概念可以让您:

1.  重用代码(变量、对象、函数等)
2.  关注正确的方法

我们可以应用相同的“this”原则来重用复杂的函数。这允许我们遵循 D.R.Y 规则(不要重复自己)来编写更干净、有组织的代码。

为了实现这一点，我们使用了一种叫做绑定的东西。绑定帮助我们将一个对象(包含“this”关键字)关联到一个函数，以获得不同的结果。当我解释 5 个绑定规则时，这将变得更加清楚。

绑定是一个高级主题，正确使用“this”可以实现。

# 1.隐式结合

隐式绑定涵盖了“this”关键字最常见的用例，也相当简单。

事实上，我之前使用的代码片段完美地封装了隐式绑定。

然而，为了简单起见，我将使用不同的代码片段来解释隐式绑定。

```
function someFunc(obj) {
  obj.printName = function () {
    console.log(this.name)
  }
}
var user = {
  name: 'foo',
  getName: function () {
    console.log(this.name);
  }
};
var user1 = {
  name: 'bar',
  getName: function () {
    console.log(this.name);
  }
};user.getName(); //=> foo
user1.getName();//=> bar
someFunc(user);
someFunc(user1);
user.printName();//=> foo
user1.printName();//=> bar
```

这里的“this”绑定到用户对象。因此，它提供了对“用户”变量中声明的变量的引用。

但有趣的是，我们给我们的“用户”附加了一个功能`printName`。

这同样适用于我们的“user1”变量。

因此，我们可以有把握地说，当我们执行`user.printName()`时，函数`printName()`会绑定到‘用户’变量。这同样适用于“用户 1”。

**换句话说，“this”绑定到点的左边。)运算符，在执行时与函数相邻。**

# 2.显式绑定

显式绑定对于调用对象范围之外的函数很有用。

为了理解这个概念，我们需要知道我上面解释过的执行上下文。每个执行上下文独立运行，每个程序可以有几个类似堆栈的上下文。

但是如果我们想从不同的执行上下文中访问一些东西呢？“这个”怎么会知道呢？

这就是显式绑定发挥作用的地方。

**在隐式绑定中，我们可以访问执行上下文内部的函数，但是在显式绑定中，我们可以访问执行上下文外部的函数。**

对于显式绑定，JavaScript 为我们提供了三个函数:`call()`、`apply()`和`bind()`。

## “call()”方法:

在`call()`方法中，函数必须作为参数传递的上下文。

```
let getBrand = function() {
     console.log(this.brand); //=> Samsung
 }

let phone = {
   name: 'Galaxy S20',
   brand: 'Samsung'  
 };
getBrand.call(phone);
```

在这里，`this`捕捉对在`call()`方法中作为参数传递的任何内容的引用。

因此，我们引用了“phone”变量的“name”和“brand”属性，并通过使用`call()`方法成功关联了`getBrand()`函数。

## “apply()”方法:

`apply()`方法弥补了`call()`方法的不足。

如上所示，call()方法接受参数，但是如果我们尝试将数组作为参数传递，我们需要手动将数组的每个索引作为参数传递，如下所示:

```
let getBrand = function(name1,name2) {
     console.log(this.brand);  //=> Samsung
     console.log(name1); //=> S10
     console.log(name2); //=> Note 9
 }

let phone = {
   name: 'Galaxy S20',
   brand: 'Samsung'  
 };
var nameList=['S10','Note 9'];
getBrand.call(phone,nameList[0],nameList[1]);
```

`apply()`方法的行为与`call()`方法完全相同，除了它可以接受一个数组作为参数。

```
let getBrand = function(name1,name2) {
     console.log(this.brand);  //=> Samsung
     console.log(name1); //=> S10
     console.log(name2); //=> Note 9
 }

let phone = {
   name: 'Galaxy S20',
   brand: 'Samsung'  
 };
var nameList=['S10','Note 9'];
getBrand.apply(phone,nameList);
```

## “bind()”方法:

和上面的函数一样，`bind()`的行为也是一样的，但是有一个关键的不同——它返回一个我们可以调用的新函数。

```
let getBrand = function(name1,name2) {
     console.log(this.brand);  //=> Samsung
     console.log(name1); //=> S10
     console.log(name2); //=> Note 9
 }

let phone = {
   name: 'Galaxy S20',
   brand: 'Samsung'  
 };
var nameList=['S10','Note 9'];
var myFun = getBrand.bind(phone,nameList[0],nameList[1]);
myFun();
```

如您所见，它返回了一个函数，我们将该函数存储在“myFun”变量中，并在以后调用。

# 3.默认全局窗口绑定

默认情况下，如果“this”没有绑定到任何绑定技术，它默认为全局对象绑定。

通过下面的例子，这一点将变得更加清楚:

```
let getArticle = function(title) {
    console.log(this.title);
};

var title = 'JavaScript tutorial';
getArticle();
```

在这里，我们在调用函数`getArticle()`之前声明了全局变量‘title ’,这是非常关键的，否则你将得到‘undefined’作为输出。

**由于没有应用绑定,“this”关键字引用全局范围，因此它将在函数中记录全局变量“title”的值。**

# 4.“新”关键字

我们已经讨论了显式和隐式绑定方式，但是还有另一种绑定方式。

“new”关键字也可以用于绑定。这是一个相当常见的问题，在大多数语言中都有使用。

关键字可用于创建如下所示的对象:

```
let phone= function(price, brand) {
     this.price = price;
     this.brand = brand;
     this.log = function() {
         console.log(this.price +  ' is price of ' + this.brand);
     }
 };
let iphone = new phone('Apple', '$1000');
let galaxy = new phone('Samsung', '$800');iphone.log(); // Apple is price of $1000
galaxy.log(); //Samsung is price of $800
```

**可以看到，使用** `**new**` **关键字提供了同一个对象的新实例。**

这形成了面向对象编程的结构。

这里，函数`phone()`使用“new”关键字创建了两次，并绑定到两个不同的变量“iphone”和“galaxy ”,这两个变量具有不同的属性。

通俗地说，函数`phone()`是用 new 关键字调用的，将绑定到新创建的对象“iphone”，然后同样的情况也会发生在“galaxy”上。

# 5.HTML 事件绑定

如果您有使用普通 JavaScript 构建网站的经验，那么您可能已经看到了这一点。

使用“this ”,我们可以绑定具有某种类型的事件侦听器的 HTML 事件，例如单击事件。

```
<div onmouseover="this.style.color='teal'">Change color!</div>
```

在上面的代码中，我们有一个带有事件监听器的 HTML 元素。

当事件监听器捕捉或监听预期的事件(在我们的例子中，鼠标指针在元素上移动)时，它将执行需要“this”关键字的函数来获取对 HTML 元素的引用。

**通过“This”关键字将 HTML 元素绑定到代码，整个点击事件功能成为可能。**

# 结论

JavaScript 应用比以往任何时候都更加普及，因此了解这种语言的高级概念是明智的。

你可以在这里找到详细的文档[，在这里你可以找到关于`super()`和箭头功能的‘this’用法。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

掌握“这”的概念对任何涉及 JavaScript 的项目都是有益的，无论是服务器端还是客户端。此外，它在编写面试代码时也是至关重要的。

重用复杂的函数和关注被调用的特定方法可以为你的代码提供一个全新的维度。