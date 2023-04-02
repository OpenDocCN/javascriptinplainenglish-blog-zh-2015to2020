# JavaScript 中的 var、let 和 const 指南

> 原文：<https://javascript.plainenglish.io/a-guide-to-var-let-and-const-in-js-25c23e7859fe?source=collection_archive---------6----------------------->

![](img/8b05fe1b030dcd11ba7549d87d1d8aa8.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

在 Javascript 中有三种方法来声明变量:`var`、`let`和`const`——但是为什么呢？为了理解这一点，我们应该先快速浏览一下这门语言的历史。

# 一堂非常简短的历史课

那是 1995 年，web 是完全静态的，每次交互都需要重新加载整个页面。计算机最多只能配备最高时钟速度为`450Mhz`的单核`Pentium II`处理器。

Java 小程序刚刚出现，看起来似乎是一条出路，但它们需要大量的专业知识才能获得最佳结果。

网景公司是第一个开发浏览器的公司，它认为网络需要一种更简单的语言，来做像`DOM`操作、动画和一般交互这样的事情——供低层程序员和设计师使用。

时间是至关重要的，这种语言仅用了六个月就被创造出来，吸收了来自 [Scheme](https://en.wikipedia.org/wiki/Scheme_(programming_language)\) 、 [Self](https://en.wikipedia.org/wiki/Self_(programming_language)) 的影响，使它看起来像 Java。它简单、动态、易于掌握，并且很好地完成了它的任务。

考虑到这一点，让我们转移到手头的主题..

# var 来了

乍看之下，声明变量的历史方法似乎很简单，就像任何其他语言一样，但是当你开始使用它时，你迟早会遇到让你挠头的情况:

```
for(var i=0; i<5; i++) {
    setTimeout(() => {
        console.log(i);
    }, 100);
}// The output is 5, 5, 5, 5, 5
```

出现这种行为是因为`var`如果在函数中声明，那么它就是函数范围的，否则就是全局的。在这种情况下——它是全局的，如果我们想让它每个超时都有自己的值，我们必须把它放在一个闭包里:

```
for(var i=0; i<5; i++) {
    ((i) => {
        setTimeout(() => {
           console.log(i)
        }, 100);
    })(i);
}
```

我们创建一个新函数并立即执行它，将`i`的当前值作为参数传递。由于数字是一个原语，新函数获得了该值的副本，我们得到了预期的结果，尽管语法上看起来有点混乱。

还有更多这方面的来源:

```
var x = 4;
{
    var x = 5;
    {
        var x = 7;
    }

    var x = 10;
}console.log(x);
```

输出会是什么？会是`10`，因为没有块作用域，只有函数作用域。

如果我们愿意，我们甚至可以重新声明变量:

```
var x = 5;
var x = 6;console.log(x); // The output is 6
```

您甚至可以不声明变量，这将使它成为全局变量，并可能覆盖某些内容:

```
function func() {
    y = 1;
}func();console.log(y); // The output is: 1
```

这些`var`语句在任何代码运行之前被执行，这导致了一个被称为`hoisting`的行为，这意味着将所有的声明移动到顶部:

```
console.log(tongueBender);
var tongueBender = "She sells seashells by the sea shore";
```

输出将是`undefined`，因为只有声明被上移，赋值停留在原来的位置。

在较新版本的 Javascript(从 ECMAScript 第 5 版开始)中，可以用`use strict`来执行它，这将不允许使用未声明的变量，但仍将`hoist`并允许您重新声明变量。它还做其他事情，比如不允许写入不可写的全局变量，抛出错误，而不是在某些情况下保持沉默，完整的描述可以在这里找到。

```
"use strict"
console.log(fancyNumber);
fancyNumber = 42; // ReferenceError: fancyNumber is not defined
```

# 与时俱进

考虑到所有这些问题，ECMAScript version 5 中出现了两种声明变量的新方法:`let`和`const`——这让 Javascript 开发人员的生活变得更加愉快。

用这些关键字声明或初始化的变量有:

*   块范围
*   不允许重新声明
*   未悬挂

这意味着我们可以预测其他语言的行为:

```
for(let i=0; i<5; i++) {
    console.log(i)
}// The output is: 0, 1, 2, 3, 4let x = 5;
let x = 6;// SyntaxError: Identifier 'x' has already been declaredlet y = 4;
{
    let y = 5;
    {
        let y = 7;
    }
}console.log(y) // The output is: 4
```

顾名思义，`const`关键字声明了一个常量，这意味着它的值以后不能更改。

```
const a = 4;
a = 5;// TypeError: Assignment to constant variable.
```

不过，在处理对象时，要记住一件事:

```
const obj = {};
obj.a = 5; // Will work just fine
obj = {};  // TypeError
```

您仍然可以修改对象，该对象被声明为`const`，因为它仍然是同一个对象。如果使用`Object.defineProperty`，属性也可以保持不变:

```
const obj = {};Object.defineProperty(obj, "a", {
    value: 4,
    writable: false,
});obj.a = "5"console.log(obj); // The output is: {}
```

如果`use strict`被设置为这个代码片段，就会引发一个类型错误:`TypeError: Cannot assign to read only property ‘a’ of object`

# 结论

Java 小程序已经过时很久了，Javascript 现在可以用于更多可以想象到的事情，谢天谢地，随着时间的推移，它的缺点已经得到解决，新的特性还在不断出现，`private`class field 有人知道吗？

那么，应该用什么来声明一个变量呢？这个问题的答案很简单——如果可以，总是使用`const`，否则使用`let`。通过使用`const`,代码库变得更加可预测，甚至可以通过编译器优化来提高性能(在其他语言中也是如此)。

千万不要用`var`，除非你喜欢危险的生活。严格模式也是首选。

我推荐在这里阅读 Javascript 的全部历史，这是一本非常有趣的书。让你想知道接下来会发生什么:)