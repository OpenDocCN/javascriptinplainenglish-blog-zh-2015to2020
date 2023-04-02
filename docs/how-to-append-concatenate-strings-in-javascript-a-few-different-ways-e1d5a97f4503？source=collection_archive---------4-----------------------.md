# 在 JavaScript 中追加(连接)字符串的 4 种方法

> 原文：<https://javascript.plainenglish.io/how-to-append-concatenate-strings-in-javascript-a-few-different-ways-e1d5a97f4503?source=collection_archive---------4----------------------->

![](img/da46f0e84dff54a9d8746246a5909337.png)

Photo by [Startup Stock Photos](https://www.pexels.com/@startup-stock-photos?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/working-woman-technology-computer-7374/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

不知道如何在 JavaScript 中从头开始连接或追加(换句话说，将一个字符串添加到另一个字符串的末尾)没有什么不好；也许你只是刚刚开始学习，或者也许你是在搜索之后来到这里的，因为你只是忘记了在 JavaScript 中如何做的语法。

这两种途径我们都经历过，所以在本文中，我们将介绍几种在 JavaScript 中追加字符串的不同方法。

# 1.通过用+运算符相加进行追加

如果你必须完全猜测的话，我喜欢认为这种方法是最合理的。这就是它的作用:

原因是因为我认为，从概念上讲，为了得到一个附加的结果，把字符串加在一起是有意义的。注意，在这个例子中，在`fry`之前有一个空格字符，因为否则输出将是`Frenchfry`。这是实践中超级容易忘记的事情。

# 2.通过添加速记+=运算符进行追加

这实际上与方法#1 密切相关，因为本质上是一样的；事情是这样的，这本书通过使用`+=`简写法来缩短内容:

对于`+=`，`str1 += str2`相当于`str1 = str1 + str2`。它通常只会为您节省一两行代码。

# 3.使用 concat()方法追加

我知道过去我不得不查找这个，因为每种语言都有一点不同的内置函数。`concat()`基本上是一个包含了 JavaScript 字符串的函数，它可以接受任意多的参数，并追加或连接它们。如 [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/concat) 所述，官方语法如下:

```
str.concat(str2 [, ...strN])
```

## 只是将一个字符串附加到另一个字符串上

对于两个字符串，可以添加如下内容:

基本上可以认为是把`str2`串联到`str1`。左手边加上了右手边。

## 将两个或多个字符串追加到另一个字符串

你不必就此打住。您可以继续将它们作为逗号分隔的参数添加到`concat()`函数中。

…您可能会认为我写这篇文章就是为了构建这个字符串，您不会完全错了。

## 警告:性能

`concat()`方法的[性能比赋值操作符](https://stackoverflow.com/a/34465939) ( `+`和`+=`)差，所以一般来说最好只使用操作符。

# 4.用 join()连接字符串数组

实际上，从技术上讲，您可以通过首先将字符串放入或放入数组中，然后使用 JavaScript 数组附带的`join()`函数来追加字符串:

`join()`函数中的`''`参数表示您正在连接数组中的所有元素，没有分隔符；如果你愿意，你可以传递一个——例如，`' '`会在所有的字符串之间加一个空格，而`'.'`会在所有的字符串之间加一个句号。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)