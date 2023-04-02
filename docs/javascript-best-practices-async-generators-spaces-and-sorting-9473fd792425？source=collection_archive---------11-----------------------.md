# JavaScript 最佳实践—异步、生成器、空格和排序

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-async-generators-spaces-and-sorting-9473fd792425?source=collection_archive---------11----------------------->

![](img/71267e22063e61c220bf97d61a97fe36.png)

Photo by [Vincent van Zalinge](https://unsplash.com/@vincentvanzalinge?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 没有导致 await 或 yield 竞争条件的赋值

`await`总是异步的，而`yield`也可以是异步的，所以如果我们并行赋值，就会导致竞争条件。

例如，如果我们有:

```
let length = 0;async function addLength(pageNum) {
  length += await getPageLength(pageNum);
}Promise.all([addLength(1), addLength(2)]).then(() => {
  console.log(totalLength);
});
```

然后我们不知道每个电话的`addLength`什么时候结束。

我们应该按如下顺序运行这些:

```
async function addLengths() {
  const length1 = await addLength(1);
  const length2 = await addLength(2);
  return length1 + length2;
}
```

# 没有没有`await`表达式的异步函数

我们不应该有没有`await`表达式的`async`函数。

如果没有`await`，这意味着我们没有使用 promises it，所以我们不需要`async`关键字。

例如，不写:

```
async function foo() {
  doWork();
}
```

我们写道:

```
async function foo() {
  await doWork();
}
```

# JSDoc 注释

我们可以使用 JSDoc 注释来解释参数和描述函数的作用。

例如，我们可以写:

```
/**
 * Multiplies two numbers together.
 * @param {int} num1 The first number.
 * @param {int} num2 The second number.
 * @returns {int} The product of the two numbers.
 */
function multiply(num1, num2) {
  return num1 * num2;
}
```

`param`解释参数，`returns`解释返回的结果。

顶部有功能描述。

# 在正则表达式上使用`u`标志

我们应该在正则表达式中使用`u`标志，以便正确处理 UTF-16 字符对。

例如，不写:

```
/^[👍]$/.test("👍")
```

我们写道:

```
/^[👍]$/u.test("👍")
```

第一个返回`false`，是错的。

但是第二个返回`true`，是对的。这是因为我们添加了`u`标志。

# 没有没有 Y 的发生器函数`ield`

没有`yield`的生成器函数不需要是生成器。

所以与其写:

```
function* foo() {
  return 10;
}
```

我们写道:

```
function* foo() {
  yield 20;
  return 10;
}
```

# Rest 算子和 Spread 算子之间的间距及其表达式

在 rest 和 spread 操作以及它们的表达式之间不需要空格。

例如，不写:

```
let arr2 = [1, 2, 3];
arr1.push(... arr2);
```

或者:

```
fn(... args)
```

我们写道:

```
let arr2 = [1, 2, 3];
arr1.push(...arr2);
```

或者:

```
fn(...args)
```

# **自动分号插入(ASI)**

我们应该自己加上分号，这样我们就知道每一行从哪里开始或结束。

例如，我们可以写:

```
let n = 0
const increment = () => {
  return ++n
}
```

我们写道:

```
let n = 0;
const increment = () => {
  return ++n;
}
```

# 分号前后的间距

如果分号后面有东西，我们应该在分号后面加一个空格

例如，不写:

```
var c = "foo";var e = "bar";
```

我们写道:

```
var c = "d"; var e = "f";
```

# 分号的位置

我们应该把分号放在有意义的地方。

例如，不写:

```
foo()
;[1, 2, 3].forEach(bar)
```

我们写道:

```
foo();
[1, 2, 3].forEach(bar);
```

更常规，更清晰。

# 导入排序

我们可以对进口商品进行分类，以便更容易跟踪。

例如，不写:

```
import b from 'foo.js';
import a from 'bar.js';
```

我们写道:

```
import a from 'bar.js';
import b from 'foo.js';
```

# 排序对象关键字

我们可以按字母顺序对对象键进行排序，以便更容易找到它们。

所以与其写:

```
let obj = {
  a: 1,
  c: 3,
  b: 2
};
```

我们写道:

```
let obj = {
  a: 1,
  b: 2,
  c: 3,
};
```

# 排序变量

我们可以对变量进行排序，使它们更容易找到，

例如，不写:

```
var b, a;
```

我们写道:

```
var a, b;
```

![](img/7f634865ad4cd36ee18c9a6cc3b84f80.png)

Photo by [Darius Cotoi](https://unsplash.com/@dariuscotoi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以对对象属性、变量、导入等进行排序。让他们更容易被找到。

此外，我们把分号放在传统的地方，使他们更容易阅读。

`u`标志应该在正则表达式中，以使 JavaScript 正确搜索字符对。

如果没有`await`，那么我们就不需要`async`函数。

如果没有`yield`，那么我们就不需要一个生成器函数。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**