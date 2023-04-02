# JavaScript 问题——复制数组项目、丰富多彩的控制台日志等等

> 原文：<https://javascript.plainenglish.io/javascript-problems-copying-array-items-colorful-console-logs-and-more-943a40f029f6?source=collection_archive---------13----------------------->

![](img/5e3c464ae87e11acb0f396c968edbf16.png)

Photo by [Dimitri Tyan](https://unsplash.com/@dimitri_tyan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 将数组项复制到另一个数组中

有几种方法可以将项目从一个数组复制到另一个数组。

我们可以使用`concat`方法传入一个包含我们想要复制的数据的数组。

例如，我们可以写:

```
const arrayA = [1, 2];
const arrayB = [3, 4];
const arr = arrayA.concat(arrayB);
```

`concat`从`arrayB`获取条目，放入`arrayA`并返回新数组。

我们也可以将`push`与扩展操作符一起使用。

例如，我们可以写:

```
const arrayA = [1, 2];
const arrayB = [3, 4];
arrayA.push(...arrayB);
```

`push`将`arrayA`修改到位，所以`arrayA`将是`[1, 2, 3, 4]`而不是`[1, 2]`。

spread 运算符将数组条目扩展到参数中。

同样，我们也可以使用 spread 操作符在没有`push`的情况下做同样的事情。

例如，我们写道:

```
const arr = [...arrayA, ...arrayB];
```

我们用 spread 操作符将两个数组中的数组条目放入一个新的数组中。

那么`arr`就是`[1, 2, 3, 4]`。

# 检查变量是否属于函数类型

我们可以用`typeof`或`instanceof`操作符检查一个函数是否是一个函数类型。

例如，我们可以写:

```
if (typeof v === "function")}{
  //...
}
```

如果`v`是函数，则`typeof`返回`'function'`。

同样，我们可以写:

```
if (v instanceof Function){
  //...
}
```

这通过检查`v`是否是`Function`构造函数的实例来检查它是否是一个函数。

# 如何用 JavaScript 让浏览器导航到 URL

有几种方法可以让浏览器导航到 JavaScript 中的 URL。

例如，我们可以写:

```
window.location.href = 'http://www.example.com';
```

或者:

```
window.location.assign("http://www.example.com");
```

或者:

```
window.location = 'http://www.example.com';
```

他们去 http://www.example.com 做同样的事情。

# 检查单选按钮的选中值

要检查单选按钮的选中值，我们可以使用 jQuery 的`prop`方法。

我们也可以使用`attr`方法。

例如，我们可以写:

```
$("#radio").prop("checked", true);
```

我们使用`prop`方法检查`'checked'`属性，看它是否是`true`。

要使用`attr`方法，我们可以写:

```
$("#radio").attr('checked', 'checked');
```

我们用`attr`检查了`checked`值，而不是`true`。

# 获取元素的实际宽度和高度

我们可以使用`getBoundingClientRect`方法得到实际的宽度和高度。

例如，我们可以写:

```
document.getElementById('id').getBoundingClientRect()
```

然后我们可以从返回的对象中用`width`属性得到宽度，用`hjeight`属性得到高度。

# 检查移动浏览器的简单方法

我们可以检查一下 b browser 的用户代理，看看是什么样的浏览器。

例如，我们可以写:

```
const isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);
if (isMobile) {
  //...
}
```

`navigator.userAgent`有用户代理字符串。

因此，我们可以用它来检查移动浏览器。

然而，用户代理字符串很容易被欺骗，所以我们在使用时应该小心。

# 从节点的 console.log 获取完整的对象

`util`模块有`inspect`方法，它将把整个对象的内容输出到控制台。

例如，我们可以写:

```
const util = require('util')console.log(util.inspect(obj, { showHidden: false, depth: null }))
```

我们在`showHidden`设置为`fakse`的情况下检查`obj`，以隐藏不可枚举的属性。

`depth`指定递归对象的次数。

默认为`null`或`Infinity`，表示检查所有级别。

# JavaScript 控制台中的颜色

我们可以将 CSS 样式添加到`console.log`方法的参数中来添加颜色。

为此，我们添加了`%c`标签来添加样式。

例如，我们可以写:

```
console.log('%c hello world', 'background: #222; color: white');
```

现在我们在黑色背景上显示白色文本。

![](img/e0728a0fefa1d5f1542458db744641f6.png)

Photo by [John Mccann](https://unsplash.com/@jmacca88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们用`util.inspect`方法检查节点控制台中的对象。

控制台日志可以有颜色。

有许多条目可以将条目从一个数组复制到另一个数组中。

还有几种方法可以检查变量是否是函数。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道，解码**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**