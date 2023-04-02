# 使用一些 JavaScript 最佳实践来减少麻烦

> 原文：<https://javascript.plainenglish.io/reducing-headaches-with-some-javascript-best-practices-f7ec0cea5e0b?source=collection_archive---------6----------------------->

![](img/6c59bf33f1fa3452e593cd664b0dee8c.png)

Photo by [Fahim](https://unsplash.com/@fahimhasan999?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 首先声明所有变量

为了更容易找到变量，我们可以在顶部声明它们。

例如，不要写:

```
function foo(){
  let i = 100;
  console.log(i);
  let j;
}
```

我们写道:"

```
function foo(){
  let i = 100;
  let j;
  console.log(i);
}
```

这样，我们在寻找变量定义时就不会有困难。

我们也不用担心使用`var`或函数声明时的提升问题。

# 小心使用类型 of

我们必须知道`typeof null`返回`'object'`。

所以不能用它来查`null`。

另外，`typeof NaN`返回`'number'`，这可能不是我们所期望的。

所以我们不应该用它们来检查这些。

# 小心对待 parseInt

也有一些我们必须考虑的怪癖。

它返回它找到的所有转换成数字的数字字符。

所以如果我们有:

```
parseInt("11");
```

它会返回 11。

如果我们有:

```
parseInt("11james");
```

这也将返回 11。

如果我们有:

```
parseInt("11.22");
```

这也返回 11。

我们应该注意这些怪癖，这样当我们试图用`parseInt`将非数字转换成数字时就不会得到意想不到的结果。

此外，如果数字从 0 开始，它会假设这是一个八进制数。

所以如果我们有:

```
parseInt("08");
```

那么在旧的浏览器中它将返回 0。

但是，在现代浏览器中，它应该被转换为十进制数。

# 不要使用开关失败

在`switch`语句的每个`case`子句中，我们应该在子句的末尾有一个`break`或`return`语句。

例如，我们写道:

```
switch (val) {
  case 1:
    console.log('One');
    break;
  case 2:
    console.log('Two');
    break;
  case 3:
    console.log('Three');
    break;
  default:
    console.log('Unknown');
    break;
}
```

以便 JavaScript 解释器不会运行匹配的`case`子句下的代码。

# 避免 For…循环

对于数组，应该避免 for-in 循环。

它不能保证迭代顺序，而且比标准的 for 循环要慢。

它用于遍历对象的非继承和继承属性。

这意味着以下循环是不等价的:

```
for (let i = 0; i < arr.length; i++) {}for (let i in arr) {}
```

如果我们想以更短的方式遍历数组，我们可以使用 for-of 循环:

```
for (let a in arr) {}
```

`a`是数组条目。

# 避免使用保留的或特殊的词语

我们不应该使用保留字来定义我们自己的标识符。

如果关闭了严格模式，这是允许的。

例如，我们可以定义如下函数:

```
function foo(arguments){}
```

这在严格模式关闭时是允许的。

如果它是开着的，那么我们会得到一个错误。

这是打开严格模式的一个很好的理由。

我们可以避免定义与保留字同名的标识符。

# 保持一致

我们应该保持代码风格的一致性。

用 ESLint 可以很容易做到这一点。

它很擅长为我们解决这些样式问题，所以我们不必担心它们。

![](img/ce32070051448738ab825f1b08e5976a.png)

Photo by [Melissa Askew](https://unsplash.com/@melissaaskew?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过遵循一些简单易行的规则来减少头痛。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**