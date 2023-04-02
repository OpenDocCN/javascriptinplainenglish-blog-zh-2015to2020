# JavaScript 基础——错误和错误

> 原文：<https://javascript.plainenglish.io/javascript-basics-bugs-and-errors-3ccfbee89619?source=collection_archive---------6----------------------->

![](img/812630eccb35a7e554aac56eae00fa9f.png)

Photo by [Qurratul Ayin Sadia](https://unsplash.com/@qurratulayin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究如何处理 JavaScript 程序中的错误。

# 疯狂的

我们计算机程序中的缺陷被称为 bug。

# 语言错误

很多 bug 都是语言错误造成的。

简单的语法或逻辑错误都会导致错误。

例如，如果我们试图将一个数字乘以一个字符串，那么我们得到`NaN`。

JavaScript 不会抱怨很多错误。

它抱怨语法错误，比如拼写错误。

如果我们试图引用一个未定义变量的属性，它也会抛出一个错误。

然而，它确实隐藏了许多错误，这使得它们更容易受到 bug 的攻击。

# 严格模式

严格模式使得 JavaScript 解释器在解释代码时更加严格。

我们在顶部使用`'use strict'`指令，让解释器知道我们想要打开严格模式。

例如，我们可以写:

```
for (i = 0; i < 10; i++) {
  console.log("hi");
}
```

然后它会运行，但是它会创建一个全局变量`i`，因为我们没有严格模式。

如果我们有严格模式，我们写:

```
"use strict";
for (i = 0; i < 10; i++) {
  console.log("hi");
}
```

然后我们得到了'`i`未定义'的错误。

不要意外地创建全局变量是一个好主意，因此这是打开严格模式的一个好理由。

`this`绑定是`undefined`在 JavaScript 严格模式开启的情况下，不作为方法调用的函数中。

否则，它将被绑定到全局对象，这是不好的。

例如，如果关闭了严格模式，我们编写:

```
function foo(){
  return this;
}console.log(foo());
```

我们得到了`window`对象。

另一方面，如果严格模式打开:

```
'use strict'function foo(){
  return this;
}console.log(foo());
```

我们得到`undefined`。

同样，如果我们忘记了使用带有构造函数的`new`，那么在没有严格模式的情况下，我们不会得到错误。

例如，我们可以写:

```
function Person(name){
  this.name = name;
}Person('james')
console.log(name);
```

我们错误地调用了`Person`构造函数，因此创建了`name`全局变量，其值为`'james'`。

因此控制台日志会显示`'james'`。

这绝对不是我们想要的。

如果我们打开了 JavaScript 严格模式，那么我们会得到一个错误:

```
"use strict";function Person(name) {
  this.name = name;
}Person("james");
console.log(name);
```

由于`this`是`undefined`，我们将得到“无法设置未定义的属性”名称。

严格模式也不允许给一个函数多个同名的参数。

它还禁用了一些不应该使用的语言功能，如`with`。

# 类型

JavaScript 变量是动态类型的，所以我们只能从代码判断数据类型是什么。

为了使我们的生活更容易，我们可能想用类型前缀来命名它们。

例如，我们可能想用前缀`num`来命名一个数字变量。

# 测试

JavaScript 解释器不会捕捉很多错误。

它会捕捉一些错误，比如语法和基本类型错误，但是很多逻辑错误不会被捕捉到。

要检查逻辑错误，我们必须自己测试。

最简单的方法是编写自动测试来检查逻辑错误。

这样，我们就不用手工做所有的测试了。

最初，编写测试比手工测试更费工夫，但是我们不必反复手工测试我们的代码，所以它们值得编写。

例如，我们可以通过编写以下代码来为我们的代码编写一些测试:

`sum.js`

```
const sum = (a, b) => {
  return a + b;
}
module.exports = sum;
```

`sum.test.js`

```
const sum = require('./sum');test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

我们在`sum.js`中有一个简单的函数，在`sum.test.js`中有相应的测试代码来测试`sum`函数。

现在我们不用亲自检查了。

测试是防止错误的好方法。

![](img/fe6fe6a50934ed318a8a6894b51f2635.png)

Photo by [Olia Gozha](https://unsplash.com/@olia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

bug 是我们程序中的缺陷。

JavaScript 解释器可以捕捉一些基本错误。

为了防止 JavaScript 程序中的错误，我们可以打开严格模式，禁用一些不好的特性。

此外，我们可以编写测试来确保一切工作正常。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**