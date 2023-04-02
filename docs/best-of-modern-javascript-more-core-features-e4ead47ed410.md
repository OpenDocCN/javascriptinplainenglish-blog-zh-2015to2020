# 现代 JavaScript 的精华—更多核心特性

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-more-core-features-e4ead47ed410?source=collection_archive---------6----------------------->

![](img/5b6844f32b865999f62236e668d34f47.png)

Photo by [Wim van 't Einde](https://unsplash.com/@wimvanteinde?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 箭头函数的函数表达式

从 ES6 开始，箭头函数允许我们创建更短的函数，并且不绑定到它自己的`this`。

这使得创建新功能更加容易和简洁。

例如，不写:

```
function Button() {
  var _this = this;
  var button = document.getElementById('myButton');
  button.addEventListener('click', function() {
    console.log('clicked');
    _this.handleClick(); 
  });
}Button.prototype.handleClick = function () {
  //···
};
```

我们创建了`_this`变量，并在函数外部将其设置为`this`，这样我们就可以在`click`监听器中使用它。

这并不理想，因为我们很容易混淆不同层次的`this`的不同值。

使用箭头函数，我们可以写:

```
function Button() {
  const button = document.getElementById('button');
  button.addEventListener('click', () => {
    console.log('clicked');
    this.handleClick();
  });
}
```

我们只需传入箭头函数，就可以使用来自`Button`函数的`this`值。

在使用之前，我们不必将`Button`的`this`设置为变量。

箭头函数对于返回表达式的回调非常有用。

例如，不写:

```
var arr = [1, 2, 3];
var cubes = arr.map(function (x) { return Math.pow(x, 3) });
```

我们写道:

```
const arr = [1, 2, 3];
const cubes = arr.map(x => x ** 3);
```

它短得多，也干净得多。

# 多个返回值

借助现代 JavaScript，我们可以轻松处理多个返回值。

我们可以返回一个对象或数组，并将条目析构为变量。

在 ES6 之前，没有析构赋值，所以我们必须这样写:

```
var matchObj =
  /^(\d\d\d)-(\d\d\d)-(\d\d\d\d)$/
  .exec('222-112-2222');
var areaCode = matchObj[1];
var officeCode = matchObj[2];
var stationCode = matchObj[3];
```

使用 ES6 或更高版本，我们可以编写:

```
const matchObj =
  /^(\d\d\d)-(\d\d\d)-(\d\d\d\d)$/
  .exec('222-112-2222');
const [_, areaCode, officeCode, stationCode] = matchObj;
```

我们使用析构将每个条目分配给一个变量。

这让我们节省了许多行代码。

# 通过对象的多个返回值

我们也可以返回一个对象并将属性析构为变量。

例如，我们可以写:

```
const obj = {
  foo: 'bar'
};const {
  writable,
  configurable
} =
Object.getOwnPropertyDescriptor(obj, 'foo');
```

我们从`getOwnPropetyDescriptor`方法中获得`writable`和`configurable`属性，并将其分配给变量。

# 从`for`和`forEach()`到`for-of`

for-of 循环可以遍历任何可迭代对象。

比`for`通用性差，但比`forEach`更快更通用。

`forEach`仅适用于数组和节点列表。

要使用 for 循环，我们编写:

```
var arr = ['a', 'b', 'c'];
for (var i = 0; i < arr.length; i++) {
  var letter = arr[i];
  console.log(letter);
}
```

用`forEach`，我们可以写:

```
var arr = ['a', 'b', 'c'];
arr.forEach(function(letter) {
  console.log(letter);
});
```

使用 for-of，我们可以写:

```
const arr = ['a', 'b', 'c'];
for (const [index, letter] of arr.entries()) {
  console.log(index, letter);
}
```

正如我们所见，我们可以在 for-of 循环中使用析构。

这在其他循环或`forEach`中不可用。

因为`arr.entries()`返回一个数组，每个条目都是一个带有索引和条目的数组，所以我们可以通过析构来获得它们。

![](img/64105c067412cc522967c38271b9800a.png)

Photo by [Laurie Byrne](https://unsplash.com/@lrb22?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

现代 JavaScript 有很好的特性，包括析构和 for-of 循环。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[T3【plain English . io找到一切的链接！](https://plainenglish.io/)