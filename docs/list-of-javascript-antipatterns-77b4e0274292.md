# JavaScript 反模式列表

> 原文：<https://javascript.plainenglish.io/list-of-javascript-antipatterns-77b4e0274292?source=collection_archive---------6----------------------->

![](img/87aec8b09e7dd6dc78ab8a5f4e6f2735.png)

Photo by [Cassidy James Blaede](https://unsplash.com/@cassidyjames?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种在很多地方都被广泛使用的语言，包括 web 开发等等。

像任何其他语言一样，当我们用 JavaScript 编程时，很容易提交反模式。

在本文中，我们将研究一些我们应该避免的反模式。

# 污染全局名称空间

污染全局名称空间是不好的，因为它会导致名称冲突，

没有严格模式，也很容易意外声明全局变量。

例如，如果没有严格模式，我们可以这样写:

```
x = 1;
```

这与以下内容相同:

```
window.x = 1;
```

`window`是浏览器 JavaScript 中的全局对象。

全局变量在任何地方都是可访问的，因此我们可以在任何地方改变它们的值。

这并不好，因为我们不想因为意外的重新分配而遇到错误。

意外分配的一个例子是:

```
function foo() {
  return 1;
}function bar() {
  for (i = 0; i < 10; i++) {}
}i = foo();
bar();
```

我们在很多地方都给`i`赋值。`i`被混乱地分配到多个地方。

# 扩展对象原型

我们不应该给`Object.prototype`添加任何新的属性。

这是因为它是全球性的。这将影响其他对象，因为几乎所有对象都扩展了`Object.prototype`。

如果每个人都这样做，人们很容易用自己的代码覆盖`Object`的原型。

相反，如果我们想要控制对象的属性，我们应该使用`Object.defineProperty`来定义带有属性描述符的属性。

# 使用多个 var 声明而不是一个

多个`var`声明可读性较差，速度稍慢，所以不用编写:

```
var a = 1;
var b = 2;
var c = 3;
```

我们应该写:

```
var a = 1,
  b = 2,
  c = 3;
```

# 使用新数组()、新对象()、新字符串()和新数字()

没有理由使用`Array`构造函数。这令人困惑，因为单参数版本和多参数版本做不同的事情。

单参数版本接受一个数字，并返回一个长度由参数设置的数组。

多参数版本将数组的内容作为参数。我们可以传入尽可能多的数据来填充新数组。

例如:

```
const arr = new Array(5);
```

返回一个有 5 个空槽的数组。

另一方面，

```
const arr = new Array(1, 2, 3);
```

返回`[1, 2, 3]`。

相反，我们应该使用数组文字来简化我们的工作:

```
const arr = [1, 2, 3];
```

这样更简单，也不那么混乱。

同样，`new Object()`只是多余的文字。如果要添加属性，我们必须在不同的行上定义属性，如下所示:

```
const obj = new Object();
obj.a = 1;
obj.b = 2;
obj.c = 3;
```

那么`obj`的值就是`{a: 1, b: 2, c: 3}`。

那只是额外的写作。相反，我们应该使用对象文字:

```
const obj = {
  a: 1,
  b: 2,
  c: 3
}
```

打字更少，也很清晰。

`new String()`和`new Number()`的问题是它们给了我们类型`'object'`。我们没有理由需要字符串和数字拥有类型`'object'`。

如果我们想把它们转换回原始值，我们必须调用`valueOf()`。

相反，我们应该使用工厂函数`String()`和`Number()`将东西转换为字符串或数字。

例如，不写:

```
const str = new String(1);
```

我们写道:

```
const str = String(1);
```

另外，不要写:

```
const num = new Number('1');
```

我们写道:

```
const num = Number('1');
```

![](img/d4ef3903e5045fa1c8a1a11d3254347a.png)

Photo by [Berkay Gumustekin](https://unsplash.com/@berkaygumustekin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 依赖于像`.map`和`.filter`这样的迭代器函数的排序

我们不应该依赖`forEach`、`map`、`filter`的迭代顺序。

如果我们将`async`函数作为过滤器传递，迭代也不起作用。

如果我们需要以特定的顺序迭代事物，那么我们应该在迭代之前按照我们想要的方式对它们进行排序。

另外，如果我们想使用条目的索引，我们可以将它传递给回调函数，这样我们就可以可靠地获得索引。

例如，我们可以编写以下代码来获取回调中的`index`和原始数组，如下所示:

```
const nums = [1, 2, 3];
nums.forEach((num, index, nums) => {
  if (index === nums.length - 1) {
    console.log('end');
  }
})
```

这比:

```
const nums = [1, 2, 3];
let count = 0;
nums.forEach((num) => {
  if (count === nums.length - 1) {
    console.log('end');
  }
  count++
})
```

不仅第二个例子更复杂，而且也不太可靠，因为我们可能会不小心在其他地方改变`count`。

另一方面，从回调参数访问索引和原始数组意味着没有机会在外部修改`count`。

这也适用于`map`和`filter`。

# 结论

我们应该避免像全局变量这样的反模式，不必要的构造函数的使用，以及扩展`Object`的原型。

此外，因为我们可以从参数中访问数字，所以我们不应该创建变量来跟踪外部的索引。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****