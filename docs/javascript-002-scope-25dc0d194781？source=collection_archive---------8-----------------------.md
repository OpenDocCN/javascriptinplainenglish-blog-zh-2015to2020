# JavaScript 002:范围

> 原文：<https://javascript.plainenglish.io/javascript-002-scope-25dc0d194781?source=collection_archive---------8----------------------->

## 以及您如何也能保持对您的变量的标签！

![](img/c4ef7dc4b64b8205cf5c9a41d2b2a3db.png)

*这是 JavaScript 概念迷你系列的一部分。我们的期望是读者对 JavaScript 的经验很少，所以如果你已经接触过这个领域，那么可以回顾一下。这并不是说更有经验的人会空手而归。我的目标是对一个主题进行足够深入的研究，以便启发那些理解可能很广泛但很肤浅的人。*

**TL；DR** :用`let`和`const`声明的变量是块范围的。用`var`声明的变量是函数范围的。不要隐式声明变量，因为这些变量将是全局范围的，并将覆盖任何其他全局范围的值。最后，通过搜索最内部的作用域来找到变量名，如果没有找到匹配的变量，那么将依次搜索外部的作用域。

> **“JavaScript 的全局范围就像一个公共厕所。你无法避免走进去，但当你走进去的时候，尽量减少与物体表面的接触。”
> ——德米特里·巴拉诺夫斯基**

现在，您已经使用 var、let 和 const 成功地声明和分配了一个变量社区，是时候使用它们了。让我们试一试。

```
let x = 1; function addOne(num){
 console.log(num+1);
}addOne(x); //-> "2"
```

太棒了。当我们在页面底部调用变量`x`和函数`addOne` 时，我们成功地访问了它们。但是我们如何确定我们对`addOne`和`x`的调用会起作用呢？结果是，**两者在被调用时都在作用域内。**

scope 上的 skinny 如下: **scope 是你的代码中变量的可访问性。**

每次我们调用一个变量，就像我们对`addOne(x)`所做的那样，JavaScript 引擎会检查我们在调用时可以访问的变量目录。因为在进行函数调用时，名字`x`和`addOne`都可以在作用域中访问，所以上面的代码不会出错。如果任一名称超出范围，我们将收到一个错误。

让我们稍微修改一下上面的代码，看看它是如何工作的。

```
{
   let b = 2;
   const c = 3;
   var d = 4;
   e = 5;
}function addOne(x){
 console.log(x+1)
}addOne(b) //-> Reference error, 'b' is not defined
addOne(c) //-> Reference error, 'c' is not defined
addOne(d) //-> "5"
addOne(e) //-> "6"
```

哇哦。这里发生了很多事。我使用花括号{}添加了一个**块**，它包含了代码顶部的变量声明。当我们试图在对例子底部的`addOne`的函数调用中访问变量时，对用`let`和`const`声明的变量的调用失败了，而其他的调用成功了。**这是因为用** `**let**` **和** `**const**` **声明的变量是块范围的。**它们的值只能在封闭块中访问。如果没有封闭块，那么它们的值可以在全局范围内访问。如果我们对`addOne` 的调用出现在花括号内，代码就工作了！参见:

```
{
   let b=2;
   const c=3;
   addOne(b) //-> 3
   addOne(c) //-> 4}function addOne(x){
 console.log(x+1)
}
```

同时，用`var`声明的变量仍然可以在块外访问。那是因为用 `**var**` **声明的**变量是函数作用域的。在包含它们的函数内部的任何地方都可以访问它们的名字。如果它们是在函数外部声明的，那么它们是全局范围的，可以在代码中的任何地方访问。****

看一下下面的代码。注意用`var`声明的变量在函数`myFunc`中的任何地方都是可访问的，但在`myFunc`之外是不可访问的。

```
function myFunc(){
  {
    var a = 1; 
    let b = 2;
    const c = 3;
    e = 5; } console.log(a); // -> 1
  console.log(b); // -> Reference Error, b is not defined
  console.log(c); // -> Reference Error, c is not defined
  console.log(e); // -> 5
}myFunc(); console.log(a); // -> Reference Error, a is not defined
console.log(e); // -> 5
```

这个例子概述了 JavaScript 变量的另一个特征。当在函数内部声明时，JavaScript 变量被 ***局部*** 到那个函数。这意味着只有当它们的父函数被调用时，它们的名字才是可访问的，并且这些名字的作用域是它们各自的块和函数。

因此，用`let`和`const`声明的变量可以在它们的封闭块中的任何地方访问，用`var`声明的变量可以在它们的封闭函数中的任何地方访问。如果没有封闭块或函数，这三个变量都是全局范围的。

但是未声明的变量`e`呢？它也可以在它的封闭块之外被访问，从上面的例子来看，`e`甚至可以在它的封闭函数之外被访问！像`e`这样的变量通常被称为***隐式全局变量，它们对开发者来说尤其阴险。***

*为了演示为什么隐式全局变量对我们的代码如此有害，让我们假设我们有一个名为 T3 的函数，它返回一个随机的游戏控制台。我们的函数创建了一个`console`变量，并使用 Math.random()方法从游戏控制台列表中随机选择。该变量被隐式创建并赋给随机选择的结果，然后由`randomConsole`返回。*

```
*function randomConsole(){
  let consoles = ['Xbox', 'PlayStation', 'Wii']; 
  console = consoles[Math.floor(Math.random()) * 2];
  return console;
}console.log(randomConsole()); 
//-> Type Error: console.log is not a function*
```

*粗略地看一下，人们会认为上面的代码能够完整地工作。然而，有趣的是，当我们试图记录`randomConsole`的结果时，我们得到一个错误，说“console.log 不是一个函数”。这就是隐式全局变量的问题所在。与显式声明的变量不同，隐式变量不是局部变量。它们存在于全局范围内，并将覆盖已经存在于全局范围内的任何其他变量—在本例中是控制台对象。*

*为了演示上述函数的工作原理，让我们使用`alert()`方法来显示`randomConsole`函数的值。*

```
*function randomConsole(){
  let consoles = ['Xbox', 'PlayStation', 'Wii']; 
  console = consoles[Math.floor(Math.random()) * 2];
  return console;
}console.log('test') // -> 'test'
alert(randomConsole()); //-> 'PlayStation' is alerted!
alert(console); // -> 'PlayStation' is alerted, too!*
```

*注意，即使在声明了`randomConsole`函数之后，我们仍然可以使用`console.log`方法。这是因为在函数内部定义的变量只有在函数被调用时才被声明。在上面的例子中，当我们试图访问最后一个警报中的`console`对象时，我们返回‘PlayStation ’,因为调用了`randomConsole`函数，并且在调用过程中控制台对象被重新分配。如果显式声明所有变量，这种行为就不必担心了。*

*作用域的强大之处在于我们可以控制代码中变量名的可访问性。因为有了局部作用域的变量，我们不必担心一个函数的变量会踩到另一个函数的变量。这大大减轻了跟踪在整个代码中声明的变量名的负担。我们可以愉快地在函数中重用变量名，而不用担心 JavaScript 引擎会误解我们的变量引用。*

*这一规则也有一些例外，例如当存在多个级别的功能和块范围时会发生什么。在这些情况下，当在内层块中访问变量时，任何拥有相同变量名的外层块都将被忽略。**只访问最内部的变量声明和赋值。**你可以在下面的例子中看到这一点。*

```
*let fruit = ['raspberry', 'tomato']; 
let vegetables = ['celery', 'spinach']; }
  let fruit = ['apple', 'blackberry', 'orange']; 
  console.log(fruit); // -> ['apple', 'blackberry', 'orange']
  console.log(vegetables); // -> ['celery', 'spinach'];
}*
```

*全局变量`fruit`和`vegetable`可以在程序块内部访问。然而，当变量查找发生在`console.log`期间时，在块内声明的水果变量优先于全局水果变量。这是因为 JavaScript 在查找变量时接受名称的协议。JavaScript 总是首先在直接作用域中搜索变量名。如果在当前范围内没有找到匹配的名称，JavaScript 就会搜索下一级封闭范围。在上面的例子中，当查找`fruit`变量时，在直接作用域中找到了块作用域的 fruit 变量，查找过程终止。当在直接作用域中查找`vegetable`变量时，没有找到匹配的变量名。JavaScript 然后在下一个包含范围内搜索，在那里找到全局可用的`vegetable`变量。这是 JavaScript 作用域的一个微妙但强大的特性，它允许我们控制嵌入函数和块中变量的名称空间。如果你想知道哪个变量赋值优先，只要记住**内部定义优先**。*

*总结一下:用`let`和`const`声明的变量是块范围的。用`var`声明的变量是函数范围的。不要隐式声明变量，因为这些变量将是全局范围的，并将覆盖任何其他全局范围的值。最后，通过搜索最内部的作用域来找到变量名，如果没有找到匹配的变量，那么将依次搜索外部的作用域。*

*感谢阅读，乡亲们。接下来:**关闭** …*