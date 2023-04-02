# JavaScript 最佳实践—操作

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-operations-9d14b36b6e34?source=collection_archive---------14----------------------->

![](img/7e5256b41eba2791a4109c65ef5e9f60.png)

Photo by [virginia lackinger](https://unsplash.com/@nowyouknowgini?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 放置？:在三元运算符中，如果表达式很长，则在各自的行中

如果表达式很长，我们应该把`?`和`:`放在它们自己的行上。

例如，我们可以写:

```
const apiUrl = env.development
  ? 'localhost'
  : 'http://www.api.com'
```

或者:

```
const apiUrl = env.development ? 'localhost' : 'http://www.api.com'
```

# 将每个 var 声明写在它们自己的语句中

为了使声明者清楚，我们应该把他们放到他们自己的行上。

例如，我们写道:

```
var foo = true
var bar = true
```

而不是:

```
var foo = true, bar = true;
```

或者:

```
var foo = true, 
    bar = true;
```

现在我们知道`foo`和`bar`都是用`var`声明的。

# 用附加括号将条件赋值换行

我们应该用额外的括号把条件赋值括起来，让读者清楚我们的意图。

例如，我们写道:

```
while ((m = text.next(expr))) {
  // ...
}
```

而不是写:

```
while (m = text.next(expr)) {
  // ...
}
```

这样，读者就会知道作业是有意的。

# 在单行块中添加空格

为了更好的可读性，我们应该在单行代码块中添加空格。

例如，不要写:

```
function foo () {return true}
```

我们写道:

```
function foo () { return true }
```

# 命名变量和函数时使用 camelCase

将 camelCase 用于变量和函数是普遍接受的约定，因此我们应该在代码中使用它以保持一致性。

例如，我们应该写:

```
let myVar = 'hello';
```

或者:

```
function myFunction () { }
```

# 尾随逗号

尾部逗号可能会被删除，以保持浏览器之间的兼容性。

例如，我们可以写:

```
const obj = {
  msg: 'hello'
}
```

哪一个比以下选项更兼容:

```
const obj = {
  msg: 'hello',
}
```

# 逗号必须放在当前行的末尾

我们应该在当前行的末尾加逗号。

如果我们这样做，就不会那么混乱了。

例如，我们不应该写:

```
const obj = {
  foo: 'foo'
  ,bar: 'bar'
}
```

相反，我们写道:

```
const obj = {
  foo: 'foo',
  bar: 'bar'
}
```

# 点应该与属性在同一行上

点应该和属性放在同一行，以表明我们正在访问它。

例如，我们写道:

```
console
  .log('hello')
```

而不是写:

```
console.
  log('hello')
```

# 文件应该以新的一行结束

我们应该在每个文件的末尾添加一个新行，这样它们就可以正确地连接起来。

同样，如果我们在文件末尾有一个空行，它们可以被适当地追加。

用新行将文件输出到终端也不会干扰 shell 提示符。

因此，我们应该在每个文件的末尾有一个新行。

# 函数标识符和它们的调用之间没有空格

函数标识符和它们的调用之间不应该有空格。

例如，我们写道:

```
console.log('hi')
```

而不是:

```
console.log ('hi')
```

# 在键-值对中的冒号和值之间添加一个空格

为了可读性，我们应该在键-值对中的冒号和值之间添加一个空格。

例如，我们写道:

```
const obj = { 'key': 'value' }
```

![](img/7635b92380ba72126b03ceddcbeba53c.png)

Photo by [Han Chenxu](https://unsplash.com/@hanchenxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 构造函数名必须以大写字母开头

根据普遍接受的 JavaScript 约定，构造函数名必须以大写字母开头。

因此，我们应该在代码中包含这一点。

例如，我们写道:

```
function Animal () {}
const cat = new Animal();
```

# 不带参数的构造函数必须用括号调用

我们应该调用一个没有带括号的参数的构造函数。

尽管在 JavaScript 中我们可以调用不带圆括号的构造函数，但是我们不应该这样做。

这就造成了混乱。

所以我们应该写:L

```
const dog = new Animal()
```

而不是:

```
const dog = new Animal
```

# 结论

为了更好的可读性，我们应该在代码中增加间距。

然而，我们应该在适当的地方添加它们。

此外，我们应该遵循共同的约定。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到他们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**