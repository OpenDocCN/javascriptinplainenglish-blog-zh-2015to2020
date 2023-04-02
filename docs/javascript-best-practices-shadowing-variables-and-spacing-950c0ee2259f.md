# JavaScript 最佳实践—隐藏变量和间距

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-shadowing-variables-and-spacing-950c0ee2259f?source=collection_archive---------8----------------------->

![](img/a628741126ac970d393862088be3acb9.png)

Photo by [Boris YUE](https://unsplash.com/@ybs9641?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 外部作用域中声明的隐藏变量没有变量声明

我们不应该有在外部作用域声明的隐藏变量的变量声明。

例如，我们不应该这样写:

```
var a = 3;function b() {
  var a = 10;
}
```

相反，我们写道:

```
function b() {
  let a = 10;
}
```

我们不想声明它们两次，因为函数外的`a`在全局范围内，这意味着它们会引起混淆。

# 没有受限名称的阴影

我们不应该用受限的名字给变量赋值。

例如，我们不应该写这样的语句:

```
const undefined = "foo";
```

相反，我们写道:

```
const foo = "foo";
```

# 函数标识符和它们的应用程序之间没有空格

函数标识符和它们的调用之间不应该有空格。

所以我们不应该有这样的事情:

```
fn ()

fn
()
```

相反，我们写道:

```
fn()
```

# 没有稀疏数组

稀疏数组有空槽。

这些可能是错误，所以我们可能想要避免它们。

例如，不写:

```
const items = [,,];
```

我们写道:

```
const colors = [ "red", "green"];
```

# 节点同步方法

如果我们正在编写一个应用程序，我们绝对不应该使用同步方法。

由于 Node 是单线程的，所以它们用一个操作就能完成整个应用程序。

他们仍然适合写剧本。

所以与其写:

```
function foo(path) {
  const contents = fs.readFileSync(path).toString();
}
```

我们写作；

```
async(function() {
  // ...
});
```

在应用程序中。

# 没有标签

制表符的间距不如空格，因为它们在文本编辑器和平台之间可能不同。

所以我们写空格而不是制表符。

我们可以用空格自动替换制表符。

# 常规字符串中没有模板文字占位符语法

常规字符串中的模板文字占位符语法是无用的。

因此，我们应该只在模板字符串中使用它们。

例如，不写:

```
const greet = "Hello ${name}!";
```

我们写道:

```
const greet = `Hello ${name}!`;
```

# 三元运算符

三元运算符对于有条件地返回行中的数据很有用。

例如，我们可以写:

```
const foo = isBar ? baz : bar;
```

它很短，也不太难读。

# 在构造函数中调用`super()`之前没有使用`this` / `super`

我们应该在子类构造函数中分配`this`属性之前调用`super`。

例如，不写:

```
class A extends B {
  constructor() {
    this.a = 0;
    super();
  }
}
```

我们写道:

```
class A extends B {
  constructor() {
    super();
    this.a = 0;
  }
}
```

我们将在第一个例子中得到一个错误。

# 限制可以作为异常抛出的内容

当使用`throw`时，我们显示 throw `Error`实例，

例如，不写:

```
throw "error";
```

我们写道:

```
throw new Error("error");
```

`Error`对象有更多有用的东西，比如堆栈跟踪和消息，这是其他对象或值所没有的。

# 行尾的尾随空格

我们不应该在行尾有尾随空白。

如果有尾随空格，我们应该删除它们。

# 没有未声明的变量

我们不应该在代码中使用未声明的变量。

如果没有严格模式，它们将被视为全局变量。

如果严格模式打开，它将被视为一个错误。

所以 w 不应该写这样的东西:

```
const bar = a + 1;
```

如果`a`没有声明。

相反，我们写道:

```
const a = 2;
const bar = a + 1;
```

# 否初始化为未定义

没有赋值声明的变量默认初始化为`undefined`，所以我们不需要显式设置。

例如，我们不需要写:

```
let foo = undefined;
```

相反，我们写道:

```
let foo;
```

# 没有使用`undefined`变量

我们不应该给`undefined`设置一个值。

在严格模式下它会抛出一个错误。

例如，我们不应该编写这样的代码:

```
const undefined = "hi";
```

给`undefined`赋值是没有意义的。

![](img/952d78250f441a148ce6b3b0ba38ee55.png)

Photo by [Oli Woodman](https://unsplash.com/@braintax?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该给受限标识符赋值。

此外，我们应该确保函数调用和代码以传统方式格式化，以避免混淆和其他问题。

节点同步方法适用于脚本，但不适用于应用程序。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**