# JavaScript 最佳实践—术语、承诺、数组和警报

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-ternaries-promises-arrays-and-alerts-2f2b7015d98e?source=collection_archive---------6----------------------->

![](img/496c58fb968f368b6e8638760b77a9df.png)

Photo by [Dan Muir](https://unsplash.com/@danonline1995?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 三元表达式的操作数

三元表达式可以分布在多行之间。

例如，我们可以写:

```
const foo = bar < baz ? value1 : value2;
```

或者:

```
const foo = bar < baz ?
    value1 :
    value2;
```

只要一致，它们都是合适的。

# 构造函数名称应该以大写字母开头

我们应该以大写字母开始构造函数的名字。

例如，我们写道:

```
const friend = new Person();
```

而不是:

```
const friend = new person();
```

这是一个普遍接受的约定，因此我们应该遵循它以保持一致性。

# 调用不带参数的构造函数时使用括号

当调用不带参数的构造函数时，我们应该加上括号。

即使我们可以跳过它，我们也应该添加它，这样人们就知道我们正在调用它。

例如，不写:

```
const person = new Person;
```

我们写道:

```
const person = new Person();
```

# 变量声明后的空行

我们可能希望在声明一组变量后添加一行。

例如，我们可以写:

```
let foo;
let bar;let abc;
```

我们可以用空行将变量分组。

# `return`语句前的空行

我们应该删除`return`语句前的空行。

例如，我们写道:

```
function foo() {
  return;
}
```

而不是:

```
function foo() {

  return;
}
```

多余的空行看起来很奇怪，对清晰没有帮助。

# 方法链中每次调用后换行

如果我们调用一长串的方法，我们应该把它们放在自己的行中。

例如，不写:

```
$("#p").css("color", "green").slideUp(2000).slideDown(2000);
```

我们写道:

```
$("#p")
  .css("color", "green")
  .slideUp(2620)
  .slideDown(2420);
```

减少水平滚动是好的。

差异也更容易阅读。

# 警报的使用

我们不应该使用`alert`进行调试。

`console`物体或`debugger`可以用于此。

相反，我们可以使用`alert`来显示警报。

`console`有多种方法记录表达式值。

在我们的代码中设置一个断点。

# `Array`构造函数

`Array`构造函数有一个很好的用途。

我们可以用它来创建空数组并用数据填充它。

例如，我们可以写:

```
Array(10).fill().map((_, i) => i)
```

创建一个包含从 0 到 9 的条目的数组。

我们用 10 调用`Array`构造函数来创建 10 个空条目。

然后我们使用`map`将条目映射到整数。

# 没有作为承诺执行者的异步函数

`async`函数已经返回了承诺，所以我们不应该把它放在构造函数中。

这是多余的，任何错误都会丢失，不会导致`Promise`构造函数拒绝。

例如，我们不应该写:

```
const result = new Promise(async (resolve, reject) => {
  readFile('foo.txt', (err, result) => {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});
```

相反，我们写道:

```
const result = new Promise((resolve, reject) => {
  readFile('foo.txt', (err, result) => {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});
```

我们只在非`async`功能中使用它。

# `No await`内部循环

我们应该将我们的条目映射到一个承诺数组，并使用`Promise.all`来并行运行它们，而不是遍历每个条目并等待它们解决。

等待要慢得多，我们可以并行运行它们，因为它们彼此不依赖。

例如，不写:

```
const foo = async (things) => {
  const results = [];
  for (const thing of things) {
    results.push(await bar(thing));
  }
  return baz(results);
}
```

我们写道:

```
const = async (items) => {
  const results = items.map(thing => toPromise(item));
  return baz(await Promise.all(results));
}
```

我们用一个函数将`things`映射到一个承诺数组。

然后我们可以用`await`和`Promise.all`同时呼叫他们。

![](img/4ae275dd7acfa3dc70d887bbfe96007e.png)

Photo by [Lauren Kay](https://unsplash.com/@lakael?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该用`Promise.all`来称呼独立的诺言。

变量声明可以用线分组。

三元表达式可以放在一行或多行中。

方法链可以放在多行中。

永远不要将`async`函数放在`Promise`构造函数中。

`Array`构造函数适合创建空数组。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**