# JavaScript 最佳实践——你应该避免的 12 条无用语句

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-useless-statements-7d5afe6a1c35?source=collection_archive---------13----------------------->

![](img/1b154446da9693fccc223eeefe73ae1c.png)

Photo by [Quinn Buffing](https://unsplash.com/@qbuffing?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 标识符中没有悬空下划线

悬空下划线用于标识私有变量，不管它们实际上是不是私有的。

最好不要用这个，因为如果它们不是私人的，人们可能会认为这是私人的。

所以与其写:

```
foo._bar();
```

我们写道:

```
foo.bar();
```

# 没有令人困惑的多行表达式

当需要分隔语句和表达式时，我们应该用分号分隔它们。

例如，我们不应该像这样编码:

```
const foo = bar
[1, 2, 3].forEach(log);
```

我们不知道这两条线是否应该在一起。

相反，我们写道:

```
const foo = bar;
[1, 2, 3].forEach(log);
```

把它们清楚地分开。

# 没有未修改的循环条件

我们不应该在循环中使用未修改的条件。

这就造成了无限循环，这大概是不想要的。

例如，我们不应该写:

```
while (item) {
  doWork(item);
}
```

相反，我们写道:

```
for (let j = 0; j < items.length; j++) {
  doWork(items[j]);
}
```

# 当存在更简单的选择时，没有三元运算符

当有更简单的选择时，我们不应该使用三元表达式。

例如，我们不应该写:

```
const isOne = answer === 1 ? true : false;
```

相反，我们写道:

```
const isOne = answer === 1;
```

我们不需要显式地写`true`和`false`。

# 在`return`、`throw`、`continue`和`break`语句后没有不可达代码

我们不应该在`return`、`throw`、`continue`和`break`语句后有不可达的代码，因为它们永远不会运行。

例如，不写:

```
function foo() {
  return true;
  console.log("end");
}
```

我们写道:

```
function foo() {
  return true;
}
```

# `finally`块中没有控制流语句

我们不应该在`finally`块中使用类似`return`语句的控制流语句。

相反，我们应该把它们放在别处。

控制流语句暂停，直到`finally`运行。

因此`try`或`catch`中的控制流语句将被`finally`中的语句覆盖。

因此，与其写:

```
try {
  return 1; 
} catch (err) {
  return 2;
} finally {
  return 3;
}
```

我们写道:

```
try {
  return 1; 
} catch (err) {
  return 2;
} finally {
  console.log('finally');
}
```

# 不否定关系运算符的左操作数

我们不应该否定关系运算符的左操作数，因为它们只否定操作数，而不是整个表达式。

例如，不写:

```
if (!key in obj) {
  // ...
}
```

我们写道:

```
if (!(key in obj)) {
  // ...
}
```

# 没有未使用的表达式

如果一个表达式没有被使用，那么我们应该删除它们。

例如，我们不应该写:

```
{0}
```

或者:

```
c = a, b;
```

相反，我们写道:

```
f()
```

或者:

```
a = 0
```

等等。

# 没有未使用的变量

如果我们有未使用的变量，我们应该删除它们。

所以如果我们有:

```
var x;
```

并且在其他地方也没有使用，那么我们应该删除它们。

# 不要过早使用变量和函数

我们不应该在代码中声明变量或函数之前使用它们。

例如，我们可以写:

```
console.log(a);
var a = 10;
```

或者:

```
f();
function f() {}
```

我们可以在声明函数和变量之前使用它们。

函数声明可以被调用，但是如果变量在声明之前就被使用了，那么它们将会被`undefined`。

为了消除混淆，我们应该写:

```
var a = 10;
console.log(a);
```

并且:

```
function f() {}
f();
```

# 没有不必要的`.call()`和`.apply()`

我们不应该不必要地使用`call`和`apply`。

如果我们不改变`this`的值，那么我们就不应该使用它们。

相反，我们只是直接调用它。

所以与其写:

```
foo.call(undefined, 1, 2, 3);
foo.apply(undefined, [1, 2, 3]);
obj.foo.call(obj, 1, 2, 3);
obj.foo.apply(obj, [1, 2, 3]);
```

我们写道:

```
foo(1, 2, 3)
obj.foo(1, 2, 3)
```

# 没有不必要的附带条款

如果我们的`catch`子句只重新抛出一个异常，那么我们不需要它们。

所以我们不应该写:

```
try {
  doWork();
} catch (e) {
  throw e;
}try {
  doWork();
} catch (e) {
  throw e;
} finally {
  cleanUp();
}
```

我们写道:

```
try {
  doWork();
} catch (e) {
  handleError(e);
}
try {
  doWork();
} catch (e) {
  handleError(e);
} finally {
  cleanUp();
}
```

![](img/3008aa6e0c242e6b1f4b3b4be585ea18.png)

Photo by [Robin Benzrihem](https://unsplash.com/@robinoode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该写无用的语句。

它们使我们的代码更加复杂，并且是多余的。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**