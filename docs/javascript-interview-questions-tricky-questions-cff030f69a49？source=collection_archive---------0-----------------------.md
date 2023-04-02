# JavaScript 面试问题——棘手的问题

> 原文：<https://javascript.plainenglish.io/javascript-interview-questions-tricky-questions-cff030f69a49?source=collection_archive---------0----------------------->

![](img/e2cfb639889aa6654d52c315c699f091.png)

Photo by [Hello I'm Nik 🇬🇧](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了得到一份前端开发人员的工作，我们需要搞定编码面试。

在这篇文章中，我们将看看各种各样可能会让任何人困惑的问题。

# 意外的全局变量创建

在下面的代码片段中，控制台日志中记录的值是什么？

```
function foo() {
  let a = b = 0;
  a++;
  return a;
}foo();console.log(typeof a);
console.log(typeof b);
```

`typeof a`应该返回`'undefined'`所以第一个`console.log`是`'undefined'`。

然而，自`b`以来的第二个`console.log`日志`'number'`是一个全局变量。

不要让`let`关键字欺骗了我们，`b`仍然是一个全局变量，因为它前面没有关键字。

`b = 0`同`window.b = 0`。

为了避免这种棘手的情况，通过在 out 代码的顶部添加`'use strict'`来使用严格模式。然后我们会得到错误'`b`未定义'。

模块总是使用严格模式，所以这不是问题。

# 数组长度属性

控制台日志的最后一行在下面的代码中显示了什么？

```
const fruits = ['apple', 'orange'];
fruits.length = 0;fruits[0];
console.log(fruits[0])
```

因为我们将`fruits`的`length`属性设置为 0，清空数组，所以`console.log`应该显示`undefined`。

因此，`fruits`成为空数组集，将其`length`设置为 0。

然后我们显示`undefined`。

# 棘手的数字数组

控制台在日志下面的代码片段中记录了什么？

```
const length = 4;
const numbers = [];
for (var i = 0; i < length; i++); {
  numbers.push(i + 1);
}console.log(numbers);
```

`numbers`数组应该是`[5]`，因为`i`在没有运行任何东西的情况下递增，因为我们有:

```
for (var i = 0; i < length; i++);
```

然后我们有:

```
{
  numbers.push(i + 1);
}
```

所以我们有一个`for`循环，它除了增加`i`之外什么也不做，还有一个块将`i + 1`推到`numbers`。

`var`关键字使`i`在`for`循环之外可用。因此，我们可以推`i + 1`，它是 5，因为`i`是 4。

所以，`numbers`就是`[5]`。

如果我们用`let`替换`var`，那么我们就不会有这个问题，因为当我们推的时候会得到一个错误，因为`i`在`for`循环之外是不可用的。

# 自动分号插入

控制台日志的最后一行显示什么？

```
function foo(item) {
  return
    [item];
}console.log(foo(5));
```

它应该显示`undefined`，因为我们运行`return`没有返回任何内容，因为在 JavaScript 的最后一行自动插入了一个分号。

`[item]`是故不曾然。

为了确保我们不会犯这样的错误，我们应该将我们返回的任何内容放在与`return`关键字相同的行上。

![](img/3babe2ba72d4e81ec0abadf06f4b86af.png)

Photo by [Dylan Collette](https://unsplash.com/@dylanjohn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 巧妙的收尾

控制台中显示的以下代码是什么？

```
let i;
for (i = 0; i < 5; i++) {
  const log = () => {
    console.log(i);
  }
  setTimeout(log, 100);
}
```

上面的代码应该记录`5`五次，因为`setTimeout`在`for`循环完成后运行`log`函数。

`setTimeout`是异步的，所以它在事件循环中排队，并在所有同步代码完成运行时运行。

因此，首先`for`循环运行并创建一个新的`log`函数，该函数捕获变量`i`。

然后`setTimeout`安排它在`for`循环完成后运行。

当`i`为 5 时，循环结束后运行`log`功能。

100 毫秒后，5 个预定的`log`回调被`setTimeout`调用。

`log`读取`i`的当前值，即 5，并以该值运行`log`。

我们可以通过将`i`传入`log`来解决这个问题，这样当前正在循环的`i`的值就保存在`log`中，如下所示:

```
let i;
for (i = 0; i < 5; i++) {
  const log = (i) => {
    console.log(i);
  }
  setTimeout(log, 100, i);
}
```

第二个参数传递到`setTimeout`之后的所有参数都被传递到回调函数中，并可通过参数进行访问。

# 结论

我们应该注意在脚本中意外创建全局变量。任何没有关键字的变量都是全局变量。

此外，我们可以在 JavaScript 中创建没有块的循环，所以要注意不要在右括号和左花括号之间插入分号。

当我们想要返回一个值时，我们应该把它放在关键字`return`之后。

最后，如果我们希望在循环中调用`setTimeout`时在`setTimeout`回调中有一个值，那么我们应该将它传递到回调中，这样当前的循环值就被保留了。

## 简明英语笔记

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****