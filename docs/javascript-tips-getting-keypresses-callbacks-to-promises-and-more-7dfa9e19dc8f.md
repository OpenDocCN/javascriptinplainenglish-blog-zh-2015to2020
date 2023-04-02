# JavaScript 技巧——获取按键、对承诺的回调等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-getting-keypresses-callbacks-to-promises-and-more-7dfa9e19dc8f?source=collection_archive---------14----------------------->

![](img/eca5926c51811fef01906a1ea92b210d.png)

Photo by [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 将现有的回访转变为承诺

有几种方法可以将现有的回电转化为承诺。

一种方法是使用`Promise`构造函数。

例如，我们可以写:

```
return new Promise((resolve, reject) => {
  window.onload = resolve;
});
```

我们可以将回调代码放在回调函数中，然后在它运行完代码后调用`resolve`。

我们也可以用一个库来做。

jQuery 有`Deferred`属性让我们这样做:

```
$.Deferred((dfrd) => {
   getData(id, dfrd.resolve, dfrd.reject);
}).promise();
```

我们用回调函数调用`Deferred`来返回一个延迟的对象。

然后我们给`promise`打电话以示承诺。

我们在异步代码中调用`resolve`和`reject`，就像我们使用`Promise`构造函数一样。

对于节点样式的回调，我们也可以像以前一样使用`Promise`构造函数:

```
return new Promise((resolve, reject) => {
  getStuff(param, (err, data) => {
    if (err !== null) reject(err);
    else resolve(data);
  });
});
```

当我们遇到错误时，我们调用`reject`，而`resolve`是我们获取数据。

我们还可以使用像 Bluebird 这样的库来将节点样式的回调转换为承诺:

```
Promise.promisifyAll(asyncCode);
```

`Promise`是蓝鸟`Promise`对象，它有`promisifyAll`方法将节点回调转换为承诺。

# 将 HTML 画布捕获为图像

我们可以用`toDataURL`方法将 HTML canvas 捕获为图像。

例如，我们可以写:

```
const canvas = document.getElementById("mycanvas");
const img = canvas.toDataURL("image/png");
```

我们可以直接使用`img`字符串作为`img`标签的`src`属性的值。

# 将逗号分隔的字符串转换为数组

要将逗号分隔的字符串转换成数组，我们可以使用`split`方法。

例如，我们可以写:

```
const array = string.split(',');
```

如果`string`是`'foo,baz'baz'`，那么我们得到`['foo', 'bar', 'baz']`作为`array`的值。

# JavaScript 中的静态变量

我们可以在 JavaScriopt 中创建一个静态变量，方法是将它们附加为构造函数的属性。

例如，我们可以写:

```
function Foo(){}
Foo.bar = 'baz';
```

那么`bar`就是`Foo`构造函数的一个静态属性。

对于类，我们可以写:

```
class Foo {
  static foo = 'bar'
}
```

我们使用`static`关键字来定义静态的`foo`属性。

无论哪种情况，我们都可以用`Foo.bar`来访问它。

# 检查数字是浮点数还是整数

我们可以用余数运算符检查浮点数是否是整数。

我们写道:

```
n % 1 === 0;
```

为此，其中`n`是一个数字。

如果它能被 1 整除，那么我们知道它是一个整数。

我们也可以使用`Number.isSafeInteger`法进行检查。

还有一种`Number.isInteger`方法。

他们都带了一个我们要检查的号码:

```
Number.isInteger(1);
Number.isSafeInteger(1);
```

# 混淆 JavaScript

为了混淆 JavaScript，我们可以使用 Uglify JS 包来实现。

我们可以通过运行以下命令来安装它:

```
npm install -g uglify-js
```

丑化的代码可以被颠倒。

此外，私有字符串在被修改后仍然可见。

# window.onload vs document.onload

`window.onload`和`document.onload`有区别。

`window.onload`当整个页面加载时运行，包括图像、CSS、脚本等。

`document.onload`是在 DOM 准备好时调用的，这可以先于图像和其他外部资源被加载。

# 获取用 jQuery 事件键按下了哪个键

我们可以得到用`keyCode`或`which`属性按下的键。

为了检查是否按下了回车键，我们检查代码 13:

```
const code = e.keyCode || e.which;
if (code === 13) { 
  //...
}
```

# 对象文字中的自引用

我们可以使用 getter 返回从对象中的其他属性派生的数据。

例如，我们可以写:

```
const foo = {
  a: 5,
  b: 6,
  get sum() {
    return this.a + this.b;
  }
}
```

我们有`sum`吸气剂，它是通过将`a`和`b`加在一起得到的。

既然吸气剂是方法，我们可以在里面使用`this`。

![](img/a267ddce885ff331c1c22012db897fa2.png)

Photo by [Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有很多方法可以将回调转换成承诺。

我们可以使用吸气剂来创建从其他属性派生的属性。

JavaScript 构造函数可以有静态变量。

我们可以得到按下的键。

## **简单明了的 JavaScript**

你知道我们有四个出版物和一个 YouTube 频道吗？在 [**找到他们，点击**](https://plainenglish.io/) 和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**