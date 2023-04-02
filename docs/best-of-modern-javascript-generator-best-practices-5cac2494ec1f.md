# 现代 JavaScript 的精华——生成器最佳实践和正则表达式

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-generator-best-practices-5cac2494ec1f?source=collection_archive---------8----------------------->

![](img/5279dd38e5395e2fa90efbc040f8a8b0.png)

Photo by [Gardie Design & Social Media Marketing](https://unsplash.com/@gardiept?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 生成器和新的 regex 特性。

# 星号

用于生成器函数的星号有一个星号。

If 必须在`function`关键字和函数名之间。

它可以有任何间距。

但是通常，它有这样的格式:

```
function* gen() {
  //..
}
```

# 生成器函数声明和表达式

生成器函数声明和表达式都是有效的。

我们可以通过编写以下代码来编写生成器函数声明:

```
function* gen() {
  //..
}
```

我们可以通过将函数声明赋给变量来创建生成器表达式:

```
const gen = function* () {
  //..
}
```

# 生成器方法定义

我们可以通过编写以下代码来添加生成器方法定义:

```
const obj = {
  * gen(x, y) {
    //...
  }
};
```

我们在方法名前加了星号。

这是以下内容的简写:

```
const obj = {
  gen: function*(x, y) {
    //...
  }
};
```

生成器方法定义类似于 getters 和 setters。

例如，我们可以写:

```
const obj = {
  get foo() {
    //...
  },
  set foo(value) {
    //...
  }
};
```

像星号一样，`get`和`set`是`foo`方法的修饰符。

# 递归`yield`

我们可以给`yield`加一个星号，从一个生成器函数调用另一个生成器函数。

例如，我们可以写:

```
function* foo(x) {
  //...
  yield* foo(x - 1);
  //..
}
```

我们用关键字`yield*`递归调用`foo`。

# `Why Use the function*`发生器功能的关键字？

选择关键字`function*`用于生成器函数，因为关键字`generator`可能会被其他东西使用。

# `yield`

`yield`是严格模式下的保留字。

所以不能戴着用。

如果 ES6 代码处于松散模式，那么关键字就变成了上下文关键字，只在生成器内部可用。

# 新的正则表达式功能

新的 regex 特性包括`/y`标志，让我们将搜索的起点锚定到 regex 对象的`lastIndex`属性。

这类似于`^`锚，但是`^`的匹配总是从 0 开始。

这意味着匹配对于使用`g`标志是有用的。

我们可以用它多次匹配一个模式。

紧跟在它的前一个之后查找匹配也很有用。

例如，我们可以使用`regex.prototype.exec`方法从一个字符串中找到所有匹配。

如果我们有:

```
const REGEX = /foo/;REGEX.lastIndex = 8;
const match = REGEX.exec('barfoobarfoo');
console.log(match.index); 
console.log(REGEX.lastIndex);
```

那么`index`将忽略正则表达式的`lastIndex`属性，但是如果我们添加`g`标志:

```
const REGEX = /foo/g;REGEX.lastIndex = 8;
const match = REGEX.exec('barfoobarfoo');
console.log(match.index); 
console.log(REGEX.lastIndex);
```

模式搜索从`lastIndex`索引值开始。

如果`y`被设置:

```
const REGEX = /foo/y;REGEX.lastIndex = 9;
const match = REGEX.exec('barfoobarfoo');
console.log(match.index);
console.log(REGEX.lastIndex);
```

如果我们将`lastIndex`设置为 9，那么我们将得到与该索引匹配的`'foo'`。

索引必须精确到比赛的起点，以便`y`旗拾取比赛。

`lastIndex`找到匹配后会更新到 12。

一起设置`/g`和`/y`与设置`y`相同。

![](img/f399e3fa84a4d70e5ebead63b9ad353e.png)

Photo by [Candice Picard](https://unsplash.com/@candice_picard?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

生成器代码风格可能会有所不同。

此外，JavaScript regex 对象现在可以使用`y`标志来查找标志后的精确匹配。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**