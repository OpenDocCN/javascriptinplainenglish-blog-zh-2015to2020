# JavaScript 中的提升是什么？

> 原文：<https://javascript.plainenglish.io/https-medium-com-javascript-in-plain-english-what-is-hoisting-in-javascript-a63c1b2267a1?source=collection_archive---------0----------------------->

![](img/6d1c322f6041bc022fc6faed9bca36af.png)

Ahoy(st) there sailor!

## 了解 JavaScript 中的提升意味着什么，并提供代码示例来帮助解释这一切

JavaScript 的许多怪癖之一就是所谓的提升。

现在，如果你是 JavaScript 编程新手，很可能你还没有写好你的代码。因此，很有可能你的吊装也不完美。😉

*你觉得了解这类内容有用吗？如果有，通过* [***订阅我的 YouTube 频道***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) ***获取更多类似内容！***

## 但是什么是吊装呢？

基本上，当 JavaScript 编译所有代码时，所有使用`*var*`的变量声明都被提升到它们的函数/局部作用域的顶部(如果在函数内声明)或全局作用域的顶部(如果在函数外声明)，而不管实际声明是在哪里进行的。这就是我们所说的*吊装*。请记住，这种“提升”的概念不会真的出现在您的代码中，而是象征性地出现，与 JavaScript 编译器如何读取您的代码有关。希望这对你有意义。不管怎样，请继续阅读，记住当我们想到“提升”时，我们可以直观地想象任何被提升的东西都被移动到顶部，但理论上，没有代码真正移动。

函数声明也会被提升，但是它们会在最上面，所以会位于所有变量声明之上。

说够了，让我们给你看一些基本的代码例子来演示提升的影响。

如果我们要在全局范围内编写以下内容:

```
console.log(myName);
var myName = ‘Sunil’;
```

## 突击测验！您认为 console.log 会输出什么？

***1。*** `*Uncaught ReferenceError: myName is not defined*`

**2。** `*Sunil*`

**3。** `undefined`

事实证明，这第三个选项其实才是正确答案。

正如我们前面提到的，当 JavaScript 在运行时编译时，变量被移动到其作用域的顶部(如果我们排除 NodeJS 的使用，在非常基本的层面上，这仅仅意味着当你的网页正在加载时)。然而，需要注意的一个关键点是，唯一移动到顶部的是变量声明，而不是赋予变量的实际值。

为了澄清我们的意思，如果我们有一段代码，假设在第 10 行，我们有`var myName = 'Sunil'`，当 JavaScript 被编译时，`var myName`将被移动到它的作用域的顶部，而`myName = 'Sunil'` 将停留在第 10 行(或者如果`var myName`被提升到第 1 行，现在可能是第 11 行)。

## 让我们看看前面的相同代码块，但是看看 JavaScript 编译器将如何在运行时输出代码:

```
var myName;
console.log(myName);
myName = ‘Sunil’;
```

这就是为什么 console.log 能够输出' **undefined** '，因为它识别出变量 **myName** 存在，但是 **myName** 直到第三行才被赋值。

## 顺便说一下…

我们将变量命名为`myName`，而不是简单的`name`，因为浏览器中的“窗口”对象已经有了一个`name`属性。如果我们要在浏览器中测试这一点，在全局范围内创建的任何变量实际上都是“窗口”对象的一部分。因此，创建`var name = 'Sunil';`与执行`window.name = 'Sunil';`相同，因此，创建`var name = ‘Sunil';`也可以通过键入`window.name`来引用。

因此，由于 window.name 已经存在(出于您的兴趣，window.name 只是返回一个空字符串——至少在 Chrome Dev 工具中是这样),我们并没有真正理解提升是如何工作的。因此，我们选择使用`myName`来代替！如果你只是一时糊涂，不要担心，吊装就是这样！

## 当我们爬出兔子洞时…

我们之前也简单提到过，函数也被提升到顶部(就在顶部，在变量声明被提升的上方)。

## 所以如果我们看下面的例子:

```
function hey() {
console.log('hey ' + myName);
};
hey();
var myName = 'Sunil';
```

**hey()** 函数调用仍会返回 undefined，因为实际上 JavaScript 解释器在运行时要编译到以下内容:

```
function hey() {
console.log('hey ' + myName);
};
var myName;
hey();
myName = 'Sunil';
```

所以当函数被调用时，它知道有一个名为`myName`的变量，但是这个变量还没有被赋值。这有几个变体，当使用**life 的**的变量表达式时会发生([如果你想阅读关于 IIFEs](https://medium.com/javascript-in-plain-english/https-medium-com-javascript-in-plain-english-stop-feeling-iffy-about-using-an-iife-7b0292aba174) 的早期文章，请单击此处)，但是试图一下子理解所有这些并不理想，所以我将让你自己研究关于**函数表达式**和**life 的**的提升。

话虽如此，上面提到的其他一切应该有助于你更好地理解起重是如何工作的。

提升的概念就是为什么你有时会遇到其他人的代码，在这些代码中变量被声明在最上面，然后在后面被赋值。这些人只是试图让他们的代码非常类似于解释器将如何编译它，以帮助他们最小化任何可能的错误。

## 但是 Let 和 Const 呢？

它们也被提升了——事实上，`var`、`let`、`const`、`function`和`class`声明都被提升了——但是我们必须记住，托管的概念不是一个字面上的过程(也就是说，声明本身不会移动到文件的顶部——它只是 JavaScript 编译器首先读取它们以便为它们在内存中创建空间的一个过程)。

`var`、`let`和`const`声明之间的区别在于它们的**初始化** — 简单来说，这仅仅意味着它们被赋予的开始值。

可以在没有值的情况下初始化`var`和`let`的实例，而如果你试图声明一个而没有同时给它赋值的话，`const`会抛出一个`Reference error`。所以`const myName = 'Sunil'`可以工作，但是`const myName; myName = 'Sunil';`不行。使用`var`和`let`，你可以在赋值之前尝试使用一个`var`值，它将返回`undefined`。然而，如果你对`let`做了同样的事情，你会得到一个`Reference Error`。

## 那么 var、`let`和`const`在吊装方面有区别吗？

是的，如果你在顶层(全局层)创建一个`var`，它会在全局对象上创建一个属性——在浏览器的情况下，这可能是窗口对象。所以创建`var myName = 'Sunil';`也可以通过调用`window.myName`来引用。

然而，如果你写了`let newName = 'Sunny';`，这将不能在全局窗口对象中访问——因此，你将不能使用`window.newName`作为对`'Sunny'`的引用。

## 下面的问题也是理解提升如何影响你的代码库的基础。

用`var`做的声明可以从它们的初始范围之外访问，而用`let`和`const`做的声明则不能。

正如我们在下面的例子中看到的，用`var`做的声明返回`undefined`，而用`let`和`const`做的声明返回错误(用*贷记*[*gvlachos*](https://medium.com/u/d3f5beae318?source=post_page-----a63c1b2267a1--------------------------------)*)来产生和写入下面的*):

```
console.log(‘1a’, myName1); // undefined
if (1) {
 console.log(‘1b’, myName1); // undefined
 var myName1 = ‘Sunil’;
}console.log('2a', myName2); // error: myName2 is not defined
if (1) {
    console.log('2b', myName2); // undefined
    let myName2 = 'Sunil';
}console.log('3a', myName3); // error: myName3 is not defined
if (1) {
    console.log('3b', myName3); // undefined
    const myName3 = 'Sunil';
}
```

# 我们做到了！

如果你喜欢这篇文章，我们也已经开始创建视频内容！通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**

*文章于 2019 年 3 月 28 日更新，以进一步解释 let 和 const 方面的提升，并修复文章中我引用“name”而不是“myName”的错别字。这篇文章于 2019 年 9 月 22 日再次更新，因为其中一个代码块中有一个关于 let 使用的错误。增加了一段额外的文字来解释“吊装”的概念确实是一个概念，而不是一个字面上的过程。*