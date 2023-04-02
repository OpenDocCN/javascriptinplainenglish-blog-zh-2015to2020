# JavaScript Void 运算符

> 原文：<https://javascript.plainenglish.io/javascript-void-operator-3c0a788f0618?source=collection_archive---------1----------------------->

![](img/7d6634d25449b1db1761f8dc25848f0f.png)

BG Photo by [John Daines](https://unsplash.com/@johnmdaines?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/space?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 有一个一元 void 运算符。它计算给定的表达式，然后返回 undefined。

```
void expression
```

> 它计算 JavaScript 表达式而不返回值。它丢弃函数的默认返回值，并显式返回`undefined.`

常用于获取未定义的原始值，通过写 ***void 0*** 或 ***void(0)*** 的方式。

```
// Outputs: undefinedconsole.log(void 0);
console.log(void "test");
console.log(void {});
```

因为 void 总是产生真正的未定义值，所以它可以在代码缩减中作为一种写未定义的较短方式使用。

请在下面找到一些 void 的使用案例。

# 1.条件语句

它可用于将未定义的**变量或数据与`undefined.`进行比较**

```
if(user.session === void(0)) { // take to login }
```

# 2.立即调用的函数表达式

在生活中，void 可以用来强制 function 关键字被当作一个表达式，而不是一个声明。

```
void function iife() {
    var doSomething = function () {};
    var doSomething1 = function () {};
    var doSomething2 = function () {
        doSomething();
        doSomething1();
     };
    var someFunction = function () {};doSomething2();
    someFunction();
}();
```

它让我们可以写出这样一种另类的生活:

```
void function iife() {
 console.log('hello');
}();// is the same as...(function iife() {
    console.log('hello');
})()
```

void 的一个限制是表达式的求值是 void(未定义)！

```
const iifeFunction = void function doSomething() {
 return 'doSomething function()';
}();
console.log(iifeFunction);
**// iifeFunction is undefined**const iifeFunction = function doSomething() {
 return 'doSomething function()';
}();
console.log(iifeFunction);
**// iifeFunction is "doSomething function()"**
```

# 3.无泄漏箭头功能

使用箭头函数，我们可以返回一个表达式。在少数情况下，这可能会导致意外的副作用，因为它会返回以前不返回任何内容的函数调用的结果。

因此，为了处理这种情况，当不使用返回值时，我们可以使用 void 操作符来确保返回的结果不会有任何意外的影响。

```
button.onclick = () => void doSomething();
```

在***do something()***之前使用 void 语句可以确保，无论***do something()***的返回值如何，代码的行为都不会改变。

# 4.带 void 的异步调用

我们也可以使用 void 和 async。我们可以将它用作代码的异步入口点。

```
void async someFunction() { 
    try {
        const response = await fetch('someAPI'); 
        const result = await response.result();
        console.log(result);
    } catch(e) {
        console.error(e);
    }
}()**// or this**(async () => {
    try {
        const response = await fetch('someAPI'); 
        const result = await response.result();
        console.log(result);
    } catch(e) {
        console.error(e);
    }
})();
```

## 参考

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void)