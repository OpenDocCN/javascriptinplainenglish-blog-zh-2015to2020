# JavaScript 面试问题—功能和范围

> 原文：<https://javascript.plainenglish.io/javascript-interview-questions-functions-and-scope-8c8793f2ef5d?source=collection_archive---------6----------------------->

![](img/30961745931b3fe2657cab068d54e97a.png)

Photo by [Louis Hansel](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了得到一份前端开发人员的工作，我们需要搞定编码面试。

在本文中，我们将研究一些基本的函数和范围问题。

# 什么是吊装？

提升意味着一个变量或函数被移动到我们定义该变量或函数的范围的顶部。

JavaScripts 将函数声明移动到它们的作用域的顶部，我们可以在以后引用它们，并获取所有的变量声明，给它们赋值`undefined`。

在执行过程中，被提升的变量被赋值或运行函数。

只有用关键字`var`声明的函数声明和变量被提升。

用`let`和`const`常量声明的变量不会被提升。此外，箭头函数和函数表达式不会被提升。

例如，下面的代码有一个被提升的函数:

```
foo();function foo(){
 console.log('foo')
}
```

`foo`被提升了，所以它可以在被定义之前被调用，因为它是一个函数声明。

下面的变量声明被挂起:

```
console.log(x);
var x = 1;
console.log(x);
```

自`var x`升起后第一个`console.log`输出`undefined`。

然后当`var x = 1`运行时，`x`被设置为 1。然后我们在第二个`console.log`中记录 1，因为`x`之前设置了它的值。

其他的都不吊了。

# 什么是范围？

JavaScript 的作用域是我们可以有效访问变量或函数的区域。

JavaScript 中有三种作用域——全局、函数和块作用域。

具有全局范围的变量和函数在脚本或模块文件中的任何地方都是可访问的。

例如，我们可以声明一个全局变量，如下所示:

```
var global = 'global';const foo = () => {
  console.log(global);
  const bar = () => {
    console.log(global);
  }
  bar();
}
foo();
```

因为我们在代码顶部声明了一个带有`var`的变量，所以它在任何地方都是可访问的。所以两个`console.log`都将输出`'global'`。

函数作用域的变量只在函数内部可用。

我们可以如下定义一个函数范围的变量:

```
const foo = () => {
  var fooString = 'foo';
  console.log(fooString);
  const bar = () => {
    console.log(fooString);
  }
  bar();
}
foo();console.log(fooString);
```

在上面的代码中，我们有函数作用域的变量`fooString`。它也是用`var`声明的，在`foo`函数和嵌套的`bar`函数中都可用。

因此，我们将用`foo`函数中的 2 个`console.log`语句记录`‘foo’`。

底部的`console.log`给了我们一个错误，因为函数范围内的变量在函数之外是不可用的。

块范围的变量仅在块内部可用。也就是说，在`if`块、功能块、循环或显式定义的块中。由花括号分隔的任何内容都是块。

它们要么用`let`定义，要么用`const`定义，这取决于它是变量还是常数。

例如，如果我们写:

```
if (true) {
  let x = 1;
  console.log(x);
}console.log(x);
```

那么`x`只在`if`块内可用。底部的`console.log`给了我们一个`x is not defined`错误。

同样，如果我们有一个循环:

```
for (let i = 0; i <= 1; i++) {
  let x = 1;
  console.log(x);
}console.log(x);
```

那么`x`只在循环内部可用。底部的`console.log`给了我们一个`x is not defined`错误。

我们还可以显式定义一个块，以便将变量与外部隔离开来:

```
{
  let x = 1;
  console.log(x);
}console.log(x);
```

`x`将只在花括号内可用。底部的`console.log`会给我们一个错误。

范围决定了 JavaScript 在多大程度上寻找一个变量。如果它不存在于当前作用域中，那么它将在外部作用域中查找。

如果它在外部作用域中找到它，并且它以一种我们可以访问变量的方式被声明，那么它将获得那个值。

否则，如果找不到它，我们就会得到一个错误。

![](img/8418af18079f1b1a9b1bf8c6358156c8.png)

Photo by [MEAX](https://unsplash.com/@meaxgang?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是闭包？

闭包是记住当前作用域和全局作用域的变量和参数的函数。

在 JavaScript 中，当内部函数可用于外部函数之外的任何范围时。

我们可以使用它以受限的方式公开私有函数或数据。

例如，我们可以编写以下闭包函数:

```
const foo = (() => {
  let x = 'Joe';
  const privateFn = () => {
    alert(`hello ${x}`);
  } return {
    publicFn() {
      privateFn();
    }
  }
})();foo.publicFn();
```

在上面的代码中，我们有一个(立即调用的函数表达式)IIFE，它运行一个函数，该函数返回一个具有`publicFn`属性的对象，该属性被设置为调用`privateFn`的函数，该函数对外部是隐藏的。

此外，调用`privateFn`时在函数内部声明了`x`。

然后我们在将返回的对象赋给`foo`后调用`publicFn`。

这段代码实现的是，我们将私有项隐藏在一个闭包内，同时将我们想要公开的内容公开给外部。

正如我们所看到的，闭包让我们持有驻留在对外部不可用的范围内的项目，同时我们可以公开在闭包之外可以使用的内容。

闭包的一个主要用途是保持一些实体的私有性，同时向外部公开必要的功能。

# 结论

提升是在编译期间将函数和变量拉到代码的顶部。

函数声明是完全提升的，用`var`声明的变量在赋值提升之前拥有一切。

作用域是指一段代码可以有效访问变量或常量的地方。

闭包是一种返回内部函数的方法，其中包含了外部函数的一些实体。这对于在公开某些功能的同时保持某些东西的私密性是很有用的。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，请用你的中级用户名在[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们发电子邮件，我们会把你添加为作者。