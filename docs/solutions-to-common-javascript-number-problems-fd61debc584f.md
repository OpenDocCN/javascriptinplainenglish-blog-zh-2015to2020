# 常见 JavaScript 数字问题的解决方案

> 原文：<https://javascript.plainenglish.io/solutions-to-common-javascript-number-problems-fd61debc584f?source=collection_archive---------4----------------------->

![](img/57e70e30363026967382466a284fdc43.png)

Photo by [Jim Bread](https://unsplash.com/@jim_bread?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们经常需要用 JavaScript 来处理数字。

在本文中，我们将探讨 JavaScript 中数字常见问题的解决方案。

# 舍入数字

我们可以使用数字的`toFixed`方法将其四舍五入到给定的小数位数。

它需要一个参数，即舍入一个数字的小数位数。如果它没有参数，那么我们根据舍入数字的通常数学规则将数字舍入到最接近的整数。

在转换它所调用的具有给定小数位数的数字后，它返回一个具有给定小数位数的新数字。

例如，我们可以使用`toFixed`方法对数字进行舍入，如下所示:

```
const n1 = (21.1).toFixed();
const n2 = (21.6).toFixed();
```

然后我们得到`n1`是 21，`n2`是 22，正如我们所料。

要将一个数字四舍五入到指定的小数位数，我们可以将一个参数传递给`toFixed`方法，如下所示:

```
const n1 = (21.1).toFixed(3);
const n2 = (21.6).toFixed(2);
```

那么我们得到`n1`是 21.100`n2`是 21.60。

参数可以在 0 到 100 之间。

还有一种`Math.round`方法，它总是将一个数字四舍五入到最接近的整数。

它采用一个参数和我们想要舍入的数字，并返回舍入后的数字。

例如，我们可以如下使用它:

```
const n = Math.round(0.8);
```

那么`n`就是 1。

# 格式化要在给定区域显示的数字

我们可以使用`toLocaleString`方法返回一个根据给定区域设置格式化的字符串。

如果没有传递区域设置字符串，那么它将根据设备操作系统中设置的区域设置的约定进行格式化。

例如，如果我们按如下方式调用它，并且我们计算机的语言环境设置为美国英语:

```
const str = (21.1).toLocaleString();
```

那么我们得到的`str`就是`'21.1'`。

另一方面，如果我们将类似于`'fr'`的区域设置字符串传递给方法，如下所示:

```
const str = (21.1).toLocaleString('fr');
```

那么`str`就是`21,1`。

它还支持任何地区的扩展代码。所以我们可以传入类似下面的代码:

```
const str = (21.1).toLocaleString('zh-Hans-CN-u-nu-hanidec');
```

我们传递了`nu`扩展键来指定我们返回一个中文十进制格式的字符串，所以`str`的值是:

```
二一.一
```

# 将数字转换为指数记数法

我们可以调用数字的`toExponential`实例方法来返回一个字符串，该字符串将数字格式化为指数符号。

它采用一个可选参数，该参数是一个整数，用于指定小数点后的位数。

例如，我们可以这样称呼它:

```
const str = (21.1).toExponential();
```

我们得到`str`是`‘2.11e+1'`。

我们可以传递小数点后要包含的位数，如下所示:

```
const str = (21.1).toExponential(3);
```

那么`str`就是`'2.110e+1'`。如果我们传入的位数小于数字的原始位数，就不会这样。精度只会降低。

![](img/3eb9bbd4d11f70e399f4bcb741267749.png)

Photo by [Sandy Millar](https://unsplash.com/@sandym10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将数字转换为指定的小数位数

我们可以调用 number 实例的`toPrecision`方法，用参数中指定的给定小数位数返回原始数字的字符串表示。

我们传递给参数的数字必须是 0 到 100 之间的整数。

例如，我们可以编写以下代码来调用它:

```
let num = 5.356789;
console.log(num.toPrecision())    
console.log(num.toPrecision(5))   
console.log(num.toPrecision(2))   
console.log(num.toPrecision(1))
```

如果我们忽略了数字的个数，那么它返回一个保留原始数字的小数位数的字符串。

因此第一个控制台日志记录了 5.356789。如果我们指定了要保留的位数，那么该方法会将数字截断为指定的位数，并将其作为字符串返回。

因此，其他 3 个日志记录了以下输出:

```
5.3568
5.4
5
```

# 结论

我们可以使用`Math.round`方法对数字进行舍入，将任意数字舍入到最接近的整数。

此外，我们可以调用`toFixed`方法将一个数字四舍五入到指定的小数位数。

我们可以使用`toLocaleString`方法返回一个字符串，其中的数字被格式化为给定的语言环境。

`toExponential`方法返回一个以指数记数法表示给定数字的字符串。

最后，`toPrecision`方法返回一个数字的字符串表示，该数字被截断到指定的小数位数。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****