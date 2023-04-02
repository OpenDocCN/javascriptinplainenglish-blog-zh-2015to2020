# JavaScript 最佳实践—删除重复项

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-removing-duplicates-562e293437c9?source=collection_archive---------6----------------------->

![](img/caf32267322fa403525e9b87732d98fd.png)

Photo by [Rachel Hisko](https://unsplash.com/@rachelhisko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 对象文本中没有重复的键

我们不应该在对象文本中有重复的键。

这将产生意想不到的结果。

例如，我们不应该写:

```
const user = {
  name: 'james',
  name: 'joe'
}
```

相反，我们应该删除它们或给它们起不同的名字。

# switch 语句中没有重复的 case 标签

在我们的`switch`语句中不应该有重复的`case`标签。

例如，我们不应该写:

```
switch (id) {
  case 1:
    // ...
  case 1:
    //...
}
```

只有第一个匹配`id`值的才会运行。

因此，我们应该将它们合并或删除。

# 每个模块使用单一导入

在我们的代码中，每个模块应该只有一个`import` statemen。

例如，我们不应该:

```
import { f1 } from 'module'
import { f2 } from 'module'
```

相反，我们写道:

```
import { f1, f2 } from 'module'
```

我们节省空间并删除重复的内容。

# 正则表达式中没有空字符类

我们的正则表达式中不应该有空的字符类。

拥有它们可能是个错误。

例如，我们不应该写:

```
const myRegex = /^abc[]/;
```

# 没有空的析构模式

如果我们有空的析构模式，这可能是一个错误。

此外，我们可能打算将一个空对象的默认值赋给一个变量。

例如，我们不应该写:

```
const { a: {} } = foo
```

相反，我们写道:

```
const { a = {} } = foo
```

如果`foo.a`是`undefined`，它将分配一个空对象给`a`。

或者我们可以写作；

```
const { a: { b } } = foo;
```

将嵌套在`a`中的东西赋给另一个变量。

# 不要使用 eval

`eval`让我们从字符串中运行代码。

这是一个安全风险，因为我们可以在字符串中运行任何代码。

此外，它还会阻止 JavaScript 解释器优化我们的代码。

因此，我们千万不要使用`eval`功能。

# catch 子句中没有重新分配异常

我们永远不应该在 catch 子句中重新分配异常。

这将丢弃原始异常数据。

比如，我们不应该写；

```
try {
  // ...
} catch (e) {
  e = 'value'
}
```

相反，我们写道:

```
try {
  // ...
} catch (e) {
  const newVal = 'value'
}
```

将我们的值赋给一个新变量比将某些东西赋给异常对象要好得多。

# 不要扩展本机对象

本机对象不应该被扩展。

因此，我们应该只是内置的。

在原生对象中拥有额外的属性会让人们感到困惑，如果它们不存在于其他地方，他们可能会感到惊讶。

例如，我们应该有这样的代码:

```
Object.prototype.age = 2
```

# 没有不必要的函数绑定

如果我们需要在函数内部改变`this`的值，我们只需要在函数上调用`bind`。

因此，如果我们不需要这样做，就不应该调用它。

所以我们不应该写:

```
const name = function () {
  getName()
}.bind(user)
```

相反，我们写道:

```
const name = function () {
  this.getName()
}.bind(user)
```

# 没有不必要的布尔转换

如果我们已经有了一个布尔值，那么我们就不应该强制转换它们。

例如，如果我们有:

```
const result = true;
```

那我们就不应该写:

```
if (!!result) {
  // ...
}
```

我们可以按原样使用它:

```
if (result) {
  // ...
}
```

# 函数表达式周围没有不必要的括号

我们不需要用括号将函数括起来。

例如，我们不应该有这样的代码:

```
const foo = (function () { })
```

我们不需要函数表达式周围的括号。

相反，我们写道:

```
const foo = function () { }
```

# 在交换机情况下使用中断来防止故障

我们应该始终在每个`case`块或语句的末尾添加`break`，这样它下面的代码就不会被执行。

例如，不写:

```
switch (filter) {
  case 1:
    foo()
  case 2:
    bar()
}
```

我们写道:

```
switch (filter) {
  case 1:
    foo();
    break;
  case 2:
    bar();
    break;
}
```

![](img/f803d3ced109ef0950720c84cd16acdf.png)

Photo by [zhengtao tang](https://unsplash.com/@tangzhengtao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

如果我们有像丢失`break`语句这样的错误代码，我们应该添加它们。

如果我们有多余的操作，比如将布尔转换成布尔，那么我们应该删除它们。

重复的成员和变量也应该消除。

## **简单英语 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**