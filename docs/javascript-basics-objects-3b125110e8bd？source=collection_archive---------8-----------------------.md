# JavaScript 基础—对象

> 原文：<https://javascript.plainenglish.io/javascript-basics-objects-3b125110e8bd?source=collection_archive---------8----------------------->

![](img/6ae150e8a8a5b56f5a733f9fbc54a0d7.png)

Photo by [virginia lackinger](https://unsplash.com/@nowyouknowgini?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在这篇文章中，我们将看看对象。

# 数据

在大多数程序中，我们必须存储数据，而不是像数字和文本这样的简单值。

为此，我们必须将它们存储在对象中。

在 JavaScript 中，我们可以用对象文字符号动态创建对象。

例如，我们可以写:

```
const obj = {
  a: 1
}
```

我们创建了一个简单的`obj`对象来存储值为 1 的属性`a`。

# 性能

JavaScript 中几乎所有的值都有属性。

例外情况是`null`或`undefined`。

要访问一个属性，我们可以使用`.`操作符:

```
obj.a
```

那么它返回 1，因为在上面的属性中`a`的属性是 1。

我们也可以写:

```
obj['a']
```

访问同一个属性。

计算括号内的表达式，并使用具有计算名称的属性来检索属性。

我们可以在括号里放一个字符串、符号或变量。

例如，我们可以写:

```
foo['foo bar']
```

访问名为`'foo bar'`的对象属性。

# 方法

对象可以有方法。

它们是具有作为值的功能的属性。

例如，我们可以这样定义它:

```
const obj = {
  foo() {
    console.log('foo');
  }
}
```

那么我们可以这样称呼它:

```
obj.foo();
```

`foo`方法是通过使用括号保存参数和花括号保存函数体来定义的。

对于参数，我们写:

```
const obj = {
  greet(greeting) {
    console.log(greeting);
  }
}
```

# 目标

对象只是属性和方法的集合。

我们可以用对象作为属性值。这将使嵌套的对象。

在大括号中，我们用逗号分隔属性。

我们可以通过编写以下内容来添加多个属性:

```
const obj = {
  foo() {
    console.log('foo');
  },
  bar: 1
}
```

外部大括号表示对象的开始和结束。

内部大括号表示一个方法。

我们可以使用`delete`操作符来删除一个属性。

例如，我们可以写:

```
delete obj.bar;
```

然后我们从`obj`中移除`bar`属性。

设置属性和删除的区别在于，设置为`undefined`仍然保留对象。

将其设置为 undefined 不会删除该属性。

`in`操作符让我们通过检查名称来检查属性是否在对象中。

例如，我们可以写:

```
'bar' in obj
```

在我们从`obj`中删除它之前，它会返回`true`。

之后，它会返回`false`。

要找到一个对象有什么属性，我们可以使用`Object.keys`方法。

我们向它传递一个对象，然后得到一个字符串属性名的数组。

例如，我们可以写:

```
const obj = {
  x: 1,
  y: 2
}const props = Object.keys(obj);
```

那么我们得到`props`是:

```
["x", "y"]
```

`Object.assign`方法将所有属性从一个对象复制到另一个对象。

例如，我们可以写:

```
const obj = {
  x: 1,
  y: 2
}const copy = Object.assign({}, obj);
```

那么`copy`是:

```
{x: 1, y: 2}
```

只有顶级属性被复制，而其余属性引用原始对象。

![](img/c57a2a06c5bc7f125bfdec98ac1e6200.png)

Photo by [Jerry Zhou](https://unsplash.com/@chn008?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 易变性

对象值可以修改，这不同于原始值，原始值是不可变的。

例如，我们可以写:

```
const obj = {
  x: 1,
  y: 2
}
```

然后，我们可以通过编写以下代码来更改属性`x`的值:

```
obj.x = 3;
```

我们像处理变量一样使用赋值操作符。

那么`obj.x`就是 3。

如果我们有两个对象，那么它们只有在内存中引用同一个对象时才被认为是相同的。

例如，如果我们有:

```
const obj = {
  x: 1,
  y: 2
}const obj2 = {
  x: 1,
  y: 2
}
```

然后`obj === obj2`返回`false`，即使它们看起来一样。

只有赋予相同的引用，它们才是相同的。

例如，如果我们有:

```
const obj = {
  x: 1,
  y: 2
}const obj2 = obj;
```

那么`obj === obj2`将返回`true`。

# 结论

对象是数据结构，它让我们保存具有各种属性的数据。

它们内部也可以有函数作为属性值。

它们是可变的，如果它们存在，它们的属性可以被复制或检查。

此外，它们也可以被删除。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**