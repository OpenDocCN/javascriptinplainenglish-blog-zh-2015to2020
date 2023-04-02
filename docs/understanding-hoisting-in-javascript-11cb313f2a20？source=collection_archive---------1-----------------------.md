# 理解 JavaScript 中的提升

> 原文：<https://javascript.plainenglish.io/understanding-hoisting-in-javascript-11cb313f2a20?source=collection_archive---------1----------------------->

![](img/b6dc83e84ce7dea9bfdab229a09e0749.png)

也许用这个词来描述正在发生的事情并不恰当，因为它会让人联想到小型起重机将变量从代码中吊到顶部的画面，但是这里有一篇关于提升意味着什么的快速文章。

当 V8 引擎(读取 javaScript 代码并将其编译成 C++的工具)解析任何 javaScript 文件时，您可能已经知道它是从上到下逐行处理的。

但在此之前，V8 会快速扫描文件，寻找任何变量和函数声明。例如:

```
const a = 23;function greet(){
  console.log("Hello");
}
```

在上面的代码中，在文件被执行之前，V8 知道`a`是一个常量，一个名为`greet`的函数存在。

如果您打开一个快速的 [REPL](https://repl.it/) 实例并将下面的代码粘贴到那里，您可以看到这一点。

```
console.log(a);var a = 23;//=> undefined
```

(注意那是一个`var`，而不是`const`或`let`。稍后会有更多内容)

a 是`undefined`的原因是 V8 ' *提升*所有的变量声明，并在内存中给它们分配空间。注意，我说的是“变量声明”(简单地说，`var a`)，而不是变量赋值(应该是`var a = 23`)，这就是为什么它知道`a`的存在，但不知道 a 的值是 23。

V8 的行为就好像变量是在任何东西被赋值之前就在页面顶部声明的，因此得名— **提升。**

# 功能也被提升

在下面的示例中:

```
sayHello();function sayHello(){
  console.log("Hello");
}//=> Hello
```

整个函数`sayHello`都是被吊起来放在内存里的，这也是为什么你可以在声明之前调用它，它仍然像在被调用之前就声明了一样工作。

# ..但是函数赋值没有被提升

让我们从上面取同样的代码，使它成为一个匿名函数并把它赋给变量`sayHello`，然后在赋值前调用它。

```
sayHello();var sayHello = function(){
  console.log("Hello");
}//=> sayHello is not a function
```

这里，变量`sayHello`被提升，但是它在执行时的值是`undefined`。

# 快速忽略变量和范围

这和提升没有必然的联系，但是我在 for 循环中发现了一个惊人的现象。

```
console.log('value of i before', i);for(var i = 0; i < 6; i++){
  console.log("Hello");
}console.log('value of i after', i);//=> value of i before undefined
//=> value of i after 5
```

你可能会认为`i`的作用域应该在那个`for`循环块内部，结果是 I 在外部也是可用的。

事实上，如果你尝试注销`window.i`，你仍然会得到`5`(它绑定到窗口对象)。

这是一个在代码中使用`let`而不是`var`的好例子，这似乎到处都在泄漏范围。

```
console.log('value of i before', i);for(let i = 0; i < 5; i++){
  console.log(i);
}console.log('value of i after', i);//=> ReferenceError: i is not defined
```

# const and let

有趣的是，`const`和`let`的升起方式与`var`不同。

例如:

```
console.log(a);const a = 23;//=> ReferenceError: a is not defined
```

[这篇由](https://www.vojtechruzicka.com/javascript-hoisting-var-let-const-variables/) [Vojtech Ruzicka](https://www.vojtechruzicka.com/) 撰写的文章比我说的更有说服力:

> 在 let/const 的情况下，直到声明实际出现的那一行才会初始化为 undefined。而且只有在没有立即分配的情况下。在上面的行中，变量位于时间死区，访问它会导致引用错误。