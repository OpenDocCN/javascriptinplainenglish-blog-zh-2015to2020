# 面向对象的 JavaScript —错误和可重复项

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-errors-and-iterables-82b808c231a9?source=collection_archive---------11----------------------->

![](img/25cecddc76b046e1ab26f2bf5f5eea62.png)

Photo by [Max Chen](https://unsplash.com/@maxchen2k?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 error 对象和 iterables。

# 错误对象

遇到错误时，JavaScript 抛出`Error`对象。

它们包括各种构造函数。

它们包括`EvalError`、`RangeError`、`ReferenceError`、`SyntaxError`、`TypeError`和`URIError`。

所有这些构造函数都继承自`Error`。

我们可以将可能抛出错误的代码放在`try`块中。

并且我们可以在`catch`块中捕捉错误。

例如，我们可以写:

```
try {
  foo();
} catch (e) {
  //...
}
```

如果`foo`抛出一个错误，那么`catch`模块将捕获该错误。

`e`抛出了错误对象。

我们可以用`e.name`得到名字，用`e.message`得到消息。

我们可以添加一个`finally`子句来运行代码，而不管是否抛出错误。

例如，我们可以写:

```
try {
  foo();
} catch (e) {
  //...
} finally {
  console.log('finally');
}
```

# ES6 迭代器和生成器

迭代器和生成器是 ES6 的新构造。

它们让我们创建各种类型的可迭代对象并使用它们。

# For…of 循环

for…of 循环让我们可以遍历任何类型的可迭代对象。

例如，我们可以用它来循环遍历一个数组:

```
const iter = [1, 2];
for (const i of iter) {
  console.log(i);
}
```

然后我们得到:

```
1
2
```

我们在循环标题中使用了`const`，这样我们就不能将循环变量`i`重新分配给其他变量。

我们可以对字符串等其他可迭代对象做同样的事情:

```
for (const i of 'foo') {
  console.log(i);
}
```

然后我们得到单个字符作为`i`的值。

for…of 循环用于迭代。

# 迭代器和可迭代对象

迭代器是公开`next`以获取集合的下一个条目的对象。

我们可以用生成器函数创建一个迭代器。

例如，我们可以写:

```
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}
```

`yield`通过调用`next`返回迭代器中我们想要返回的下一项。

一旦我们调用了`next`，那么发电机功能将被暂停。

我们可以通过书写来使用它:

```
const gen = genFunc();
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
```

然后我们得到:

```
{value: 1, done: false}
{value: 2, done: false}
{value: 3, done: false}
{value: undefined, done: true}
```

我们得到每个调用产生的每个项目。

`value`有生成的值，而`done`告诉我们生成器是否完成了值的生成。

iterable 是定义其迭代行为的对象。

它们可以与 for…of 循环一起用于迭代。

内置的可迭代对象包括数组和字符串。

所有 iterable 对象都有`@@itrerator`方法，该方法有以`Symbol.iterator`为键的属性。

它必须用`next`方法返回一个迭代器。

所以最简单的方法是使用生成器，这样我们就可以调用`next`并返回值。

我们可以通过编写以下代码来创建一个 iterable:

```
const obj = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  }
}
```

然后，我们可以通过编写以下代码来使用 for…of 循环:

```
for (const i of obj) {
  console.log(i);
}
```

然后我们得到:

```
1
2
3
```

记录在案。

![](img/d6c057f05b3a0c1f3e1fc0421d229276.png)

Photo by [Keagan Henman](https://unsplash.com/@henmankk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES6 可迭代对象，如数组和字符串，使用迭代器返回它所拥有的值。

也可以用 for…of 循环迭代它们。

任何代码都可能抛出错误对象，我们可以用 try-catch-finally 捕获它们。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**