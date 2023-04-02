# 为工作创建正确的 JS 函数

> 原文：<https://javascript.plainenglish.io/creating-the-correct-function-for-the-job-9e15069dcca3?source=collection_archive---------4----------------------->

![](img/808faddb63993060a39ce3f41b27615e.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 后退一步

编写函数是封装可重用逻辑的一种简单但非常强大的方法，它对于掌握编写真正漂亮的代码非常重要。

用 Javascript 编写函数有很多种方式——我们应该将它们混合在一个代码库中还是坚持一种特定的风格？只有一种方法可以找到答案，那就是尝试所有的方法。

# 函数声明

最直白的一种，但它有一个肮脏的小秘密。

```
console.log(powerOfTwo(2)); // outputs 4function powerOfTwo(num){
    return num * num;
}
```

它们和用`var`声明的变量一样被提升，正因为如此，它们应该被避免。在声明之前使用它没有任何意义，只会降低可读性。棺材上还有最后一颗钉子:

```
console.log(powerOfTwo(2)); // outputs 8function powerOfTwo(num){
    return num* num;
}function powerOfTwo(num){
    return num * num * num;
}
```

我们可以重新声明函数，这可以带来一些有趣的调试会话。

# 函数表达式

声明的表亲，没有提升和重新声明的行为。

```
console.log(powerOfTwo(2)); // ReferenceErrorconst powerOfTwo = function(num) {
    return num * num;
}
```

虽然没有人阻止你改变变量指向的函数:

```
let myFunc = function(num) {
    return num * num;
}myFunc = function(num){
    return -num
}console.log(powerOfTwo(2)); // outputs -2
```

还有另一种风格的函数表达式——命名函数表达式，看起来非常相似，只是在关键字`function`后增加了一个名称:

```
const powerOfTwo = function power(num){
    // trademarked logic
}const powerOfThree = function(num){
    // trademarked logic
}console.log(powerOfTwo.name);   // power
console.log(powerOfThree.name); // powerOfThree
```

如果函数需要引用自身，这些方法会很有用:

```
setTimeout(function named(flag){
    if(flag){
        console.log("this is the end");
    } else {
        console.log("march on");
        named(true);
    }
}, 0);
```

虽然我更喜欢创建一个单独的函数表达式，并将其传递给`setTimeout`，因为我相信这样会使代码更清晰，并很好地分离声明和使用的关系。

```
const toTimeout = function (flag){
    if(flag){
        console.log("this is the end");
    } else {
        console.log("march on");
        toTimeout(true);
    }
}setTimeout(toTimeout, 0);
```

# 箭头函数表达式

它们的外观和行为就像函数表达式，没有`function`关键字，并且在参数括号后添加了一个“粗箭头”:

```
const powerOfTwo = (num) => {
    return num * num
}
```

我们可以把它变成一行程序，单一乘法变成返回语句:

```
const powerOfTwo = (num) => num * num;
```

当我们有一个参数时，我们甚至可以去掉括号，尽管多个参数确实需要它们。

```
const powerOfTwo = nom => nom * nom;
const sum = (x, y) => x + y;
```

如果我们没有参数呢？这也是有办法的。

```
// preffered way
const random = _ => 5;// works, but is more noisy
const rand = i_dont_mattress => Math.random();
```

如果你正在参与 [code-golf](https://en.wikipedia.org/wiki/Code_golf) ，所有这些关于如何编写一个单箭头函数的选项都很棒，但是我相信如果每个函数看起来都稍有不同，会损害可读性。如果所有函数看起来都一样，那么快速解析代码会容易得多，一致性仍然是王道。当我编写箭头函数时，我总是使用“全脂”语法，因为它简洁且易于理解:

```
const myFunc = (myParam) => {
    // truly magnificent code
    return myParam;
}
```

虽然拥有简洁一致的语法很重要，但这还不够。

# 本质

当在我们的函数中使用`this`时，关键的区别就出现了。函数根据函数执行的位置从父对象获取它们的`this`对象，而箭头函数获取词法父作用域。

```
function declaration() {
    console.log(this);  // window object
}const named = function declaration() {
    console.log(this);  // window object
}const expression = function() {
    console.log(this);  // window object
}const arrow = () => {
    console.log(this);          // window object
}
```

*注意:如果用 NodeJS 执行，对于箭头* `*this*` *将* `*{}*` *作为 Node 中的全局作用域是模块本身，而不是浏览器中的全局对象。*

如果函数嵌套在对象中会怎样？

```
const obj = {
    a: arrow,
    f: expression
}obj.a(); // {} or window
obj.f(); // { a: [Function: arrow], f: [Function: expression] }
```

箭头表达式仍将有其父作用域，而函数表达式将有父对象，因为它是从父对象执行的。如果我们在一个类中使用这些函数呢？

```
class A {
    value = 5;

    constructor() {
        this.a = arrow;
        this.f = expression;
    }
}const a = new A();
a.a(); // {}
a.f(); // A { value: 5, a: [Function: arrow], f: [Function: expression] }
```

箭头`this`不会改变，而函数表达式`this`会改变。

```
class B {
    value = 5;

    constructor() {
        this.a = () => {
            console.log(this);
        };
        this.f = function() {
            console.log(this);
        };
    }
}const b = new B();
b.a(); B // { value: 5, a: [Function (anonymous)], f: [Function (anonymous)] }b.f(); B // { value: 5, a: [Function (anonymous)], f: [Function (anonymous)] }
```

*重要提示:你不能绑定箭头函数，因为它们没有自己的* `this` *。*

所以我们可以对方法使用函数或者箭头表达式？嗯，不:

```
class C {
    declaration() {
       console.log("declaration");
    }

    expression = function() {
       console.log("expression");
    }arrow = () => {
       console.log("arrow");
    }
}class CC extends C {
    declaration() {
      super.declaration();  // can call
    }

    expression = function() {
        super.expression(); // can't call
    }

    arrow = () => {
        super.arrow();      // can't call
    }
}const cc = new CC();
cc.declaration();
cc.expression();
cc.arrow();
```

对于类方法，最好的方法是使用函数声明，因为这样你就可以从父类中调用该方法，而其他所有方法都不会将函数添加到原型中。如果我们在构造函数上定义方法呢？

```
class C { 
    constructor(){
        this.expression = function() {
            console.log("expression");
        }

        this.arrow = () => {
            console.log("arrow");
        }
    } 
}
```

不能在那里使用声明，表达式也不能使用 super 来调用它们。

# 结论

根据不同的情况，每一种功能都有它自己的位置:

*   函数声明——非常适合类方法，不如函数好
*   函数表达式——作为文字对象的方法很好，但对于类方法就不那么好了
*   箭头函数——很好，当你需要在传递函数的时候有父作用域，但是不适合任何类型的方法

虽然您可以使用 arrow 函数作为对象方法，但是如果您不需要使用`this`，或者冒着被错误地重新声明的风险对纯函数使用函数声明，要点是了解您的工具并为工作选择正确的工具，而不是将所有工具拼凑在一起，然后调试奇怪的错误。