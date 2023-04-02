# 更多面向初学者的 JavaScript 最佳实践

> 原文：<https://javascript.plainenglish.io/more-javascript-best-practices-for-beginners-401e0375afb9?source=collection_archive---------14----------------------->

![](img/e819774799bdff52dbd9d65c2b8ea40e.png)

Photo by [Alex Makarov](https://unsplash.com/@alex_makarov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 使用{}而不是新对象()

我们应该使用对象文字而不是`new Object()`。

构造函数并不比对象文字更好，而且它更长。

而不是写:

```
let o = new Object();
o.name = 'james';
o.lastName = 'smith';
o.greet = function() {
   console.log(this.name);
}
```

我们写道:

```
const o = {
   name: 'james',
   lastName = 'smith',
   someFunction() {
      console.log(this.name);
   }
};
```

我们可以写:

```
const o = {};
```

创建一个空对象。

# 使用[]而不是新数组()

`Array`构造函数比较长，并没有带来太大的好处。

但是，我们可以用给定数量的空槽创建一个数组。

所以我们可以用它和一个数字参数来创建一个空数组。

除此之外，我们使用数组文字。

例如，不要写:

```
const a = new Array();
a[0] = "james";
a[1] = 'smith';
```

我们写道:

```
const a = ['james', 'smith'];
```

要用`Array`构造函数创建一个空的，我们写:

```
const a = new Array(10);
```

这将创建一个有 10 个插槽的空数组。

# 省略关键字，用逗号代替

如果我们必须声明一个很长的变量列表，我们可以用逗号来分隔它们。

例如，我们可以写:

```
let foo = 'some string',
  bar = 'another string',
  baz = 'one more string';
```

而不是:

```
let foo = 'some string';
let bar = 'another string';
let baz = 'one more string';
```

它比较短，所以节省了一些打字的时间。

# 总是使用分号

我们应该使用分号，这样 JavaScript 就不会替我们插入分号。

这样，我们可以避免错误。

所以与其写:

```
const someItem = 'some string'
function foo() {
  return 'something'
}
```

我们写道:

```
const someItem = 'some string';
function foo() {
  return 'something';
}
```

# “For in”语句

如果我们使用 for-in 循环，我们应该使用`hasOwnProperty`来检查属性是否是我们正在循环的对象的非继承属性。

例如，我们可以写:

```
for (let key in object) {
   if (object.hasOwnProperty(key)) {
      //...
   }
}
```

我们用 eh `key`作为参数调用对象上的`hasOwnProperty`进行检查。

# 使用计时器功能来优化我们的代码

我们可以使用`console.time`和`console.timeEnd`函数来检查代码的时序。

例如，我们可以写:

```
console.time("timer");
for (let x=5000; x > 0; x--) {}
console.timeEnd("timer");
```

这两种方法的参数都是计时器名称。它用于确定何时开始或结束计时器。

# 学习新东西

每年都有新的 JavaScript 特性出现。

它们通常是我们想要使用的有用特性。

所以我们应该经常阅读以跟上他们。

我们可以去 [MDN](https://developer.mozilla.org/en-US/) 网站了解一下。

# 自动执行功能

我们可以用圆括号将函数括起来，然后调用它们，这样就可以让函数在页面加载时自动运行。

例如，我们可以写:

```
(() => {
   return {
      name: 'james',
      lastName: 'smith'
   };
})();
```

我们可以创建函数来返回一些东西。

此外，我们可以在其中保存私有变量。

# 原始 JavaScript 总是比使用库更快

如果在普通的 JavaScript 中有可用的东西，那么我们应该使用它。

浏览器对标准 JavaScript 库进行了速度优化。

因此，我们可能希望使用 Fetch API，而不是使用第三方 HTTP 客户端。

我们可以不使用 Lodash 数组方法，而是使用内置数组方法。

# JSON。从语法上分析

让我们将 JSON 字符串解析成 JavaScript 对象。

我们可以通过编写以下代码在 JSON 字符串上使用它:

```
JSON.parse(jsonStr);
```

其中`jsonStr`是字符串。

# 删除语言和类型

如果我们包含 JavaScript 脚本，我们不需要添加`language`和`type`属性。

例如，不写:

```
<script type="text/javascript" language="javascript" src="script.js"></script>
```

我们写道:

```
<script src="script.js"></script>
```

![](img/0646caef76d5b5fe145e915b6ee322cf.png)

Photo by [Nick & Djalila](https://unsplash.com/@nickanddjalila?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该跟上新的结构。

他们可能会节省我们的时间。

其他好东西包括自调用函数、简化脚本标签和 JavaScript 标准库中的许多东西。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**