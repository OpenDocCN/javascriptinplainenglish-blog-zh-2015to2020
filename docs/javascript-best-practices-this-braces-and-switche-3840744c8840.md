# JavaScript 最佳实践— this、括号和开关

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-this-braces-and-switche-3840744c8840?source=collection_archive---------8----------------------->

![](img/baacd37bb3b6ab6878c9adb5fb2a2f68.png)

Photo by [Jeffrey Buchbinder](https://unsplash.com/@jbuchbinder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 这是一致的名称

如果我们把`this`赋给一个变量，我们应该给它取一个一致的名字。

我们可以选择`that`、`self`或`me`。

例如，我们写道:

```
const that = this;
jQuery('p').click(function(event) {
  that.setFoo(42);
});
```

或者:

```
const  self = this;
```

或者:

```
let me;function f() {
  me = this;
}
```

坚持一个名字可以减少混淆。

# 构造函数中对`super()`的调用

在将实例变量赋值之前，我们需要调用`super`。

例如，我们写道:

```
class A extends B {
  constructor() {
    super();
    this.foo = 'bar';
  }
}
```

我们在`foo`赋值之前调用`super`。

这样，我们就不会出错。

我们需要为子类调用`super`。

对于父类，我们不需要它:

```
class A {
  constructor() {}
}
```

# 遵循花括号约定

我们应该在我们的程序块中加上花括号。

而不是写:

```
if (foo) foo++;
```

或者:

```
if (foo) {
  baz();
} else qux();
```

我们写道:

```
if (foo) {
  foo++;
}
```

或者:

```
if (foo) {
  baz();
} else {
  qux();
}
```

那么我们就不会被一个区块的起点和终点搞混了。

# Switch 语句中的默认情况

我们应该在`switch`语句中放一个`default` case。

这样，我们就能处理意料之外的案子。

例如，不写:

```
switch (foo) {
  case 1:
    doWork();
    break; case 2:
    doWork();
    break;
}
```

我们写道:

```
switch (foo) {
  case 1:
    doWork();
    break; case 2:
    doWork();
    break; default:
    // do nothing
}
```

# 最后一个默认参数

如果我们把默认参数放在最后，那么我们知道默认参数总是最后的。

这使得辨别论点更加容易。

例如，不写:

```
function createUser(isMember = false, id) {}
```

我们可以写:

```
function createUser(id, isMember = false) {}
```

现在我们没有任何困惑。

# 在点前后强制换行

换行符通常在点之前。

例如，我们可以写:

```
const a = universe
  .earth;
```

而不是:

```
const a = universe.
  earth;
```

用第一种方式写比较常规，比较不容易混淆。

# 点符号

大多数属性访问代码应该使用点符号。

括号符号应该用于变量属性或不是有效标识符的属性。

例如，我们写道:

```
const x = foo.bar;
```

而不是:

```
const x = foo["bar"];
```

但是我们可以写:

```
const x = foo[bar];
```

或者:

```
const x = foo['bar baz'];
```

标识符不能有空格，所以我们可以使用括号符号来访问它们。

# 文件末尾换行

文件末尾的换行符使得正确地将文件连接在一起更加容易，所以我们应该使用它们。

例如，不写:

```
function work() {
  var foo = 2;
}
```

我们写道:

```
function work() {
  var foo = 2;
} 
```

# Return 语句可以不返回任何内容

我们不必返回带有`return`的东西。

它只能用来结束一个函数的执行。

例如，我们可以写:

```
function doWork(condition) {
  if (condition) {
    return;
  } else {
    return true;
  }
}
```

`return`可以返回 nothing 来提前结束一个函数。

这减少了嵌套，因为上面的示例与以下示例相同:

```
function doWork(condition) {
  if (condition) {
    return;
  }
  return true;
}
```

# Require ===而且！==

`===`和`!==`应用于比较。

他们比较类型和内容，不做类型强制。

例如，不写:

```
[] == false
```

我们写道:

```
[] === false
```

而不是写:

```
'hello' != 'world'
```

我们写道:

```
'hello' !== 'world'
```

# “for”循环更新条款将计数器向正确的方向移动

我们应该确保循环计数器向正确的方向移动，这样我们就不会得到无限循环。

例如，不写:

```
for (var i = 0; i < 10; i--) {
}
```

我们写道:

```
for (var i = 0; i < 10; i++) {
}
```

![](img/083bb0c6301f09844e664899d3a96199.png)

Photo by [Pascal Mauerhofer](https://unsplash.com/@pascaloutdoor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该确保不要意外地做无限循环。

此外，我们应该确保`super`和`this`在课堂上正确使用。

`default`案也适用于`switch`陈述。

## 简单英语中的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**