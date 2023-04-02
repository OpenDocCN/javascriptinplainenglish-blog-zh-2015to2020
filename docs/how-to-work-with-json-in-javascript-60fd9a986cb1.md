# 如何在 JavaScript 中使用 JSON

> 原文：<https://javascript.plainenglish.io/how-to-work-with-json-in-javascript-60fd9a986cb1?source=collection_archive---------6----------------------->

## 通过实际例子了解 JSON

![](img/3579e1e12a637f646bfb4112b7ade05f.png)

Photo by [Tudor Baciu](https://unsplash.com/@baciutudor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

JSON 代表**J**ava**S**script**O**object**N**旋转，它是一种存储和传输数据的格式，当数据从服务器发送到网页时经常使用。JSON 语法源自 JavaScript 对象表示法语法，但是它的格式是纯文本的。生成 JSON 数据的代码可以用任何编程语言编写，而不仅仅是 JavaScript。

在本文中，我们将学习如何在 JavaScript 中正确使用 JSON。让我们开始吧。

![](img/8be9f214a36d059337deee4a39c16b25.png)

Image created with ❤️️ By author.

# JSON 格式

为了帮助您更好地理解这个主题，我给出了一个 JSON 语法示例，它在一个数组中定义了一些 employees 对象。

看看下面的例子:

```
{
"employees":[
  {"firstName":"John", "lastName":"Doe"},
  {"firstName":"Anna", "lastName":"Smith"},
  {"firstName":"Peter", "lastName":"Jones"}
]
}
```

正如您在上面的文件`.json`中看到的，JSON 语法与 JavaScript 对象的语法相同。但是 JavaScript 对象中的键不是引号中的字符串。此外，JavaScript 对象在传递给值的类型方面受到的限制较少，因此它们可以使用函数作为值。

JavaScript 对象只能存在于 JavaScript 语言中，所以当您处理需要通过各种语言访问的数据时，最好选择 JSON。

# 访问 JSON 数据

我们可以像访问 JavaScript 对象一样访问 JSON 中的数据。为了理解这是如何工作的，让我们考虑 JSON 对象`user`:

```
var user = {
  "first_name"  :  "John",
  "last_name"   :  "Doe",
  "online"      :  true
}
```

为了访问任何值，我们将使用如下所示的点符号:

```
user.first_name    //John
user.last_name     //Doe
user.online       //true
```

无论是对于 JSON 对象还是 JSON 数组，都可以使用括号符号。

因此，使用点符号或方括号语法允许我们访问 JSON 格式中包含的数据。

# 将 JSON 转换成 JavaScript 对象

当我们想从服务器获取数据并显示在网页上时，我们需要将 JSON 转换成 JavaScript 对象。

为了做到这一点，我们需要创建一个包含 JSON 语法的 JavaScript 字符串，然后我们还必须使用 JavaScript 内置函数`**JSON.parse()**`将字符串转换成 JavaScript 对象。

看看下面的例子:

```
var text = **'**{ "employees" : [' +
'{ "firstName":"John" , "lastName":"Doe" },' +
'{ "firstName":"Anna" , "lastName":"Smith" },' +
'{ "firstName":"Peter" , "lastName":"Jones" } ]}**'**;
```

现在，使用内置函数`Json.parse()`将字符串转换为对象。

```
var obj = **JSON.parse(text)**;
```

# 将对象转换为 JSON 字符串

要将一个对象转换成 JSON 字符串，我们必须使用函数`JSON.stringify()`。

我们将查看一个 JSON 对象，我们将其赋给变量(obj)，然后我们将使用`JSON.stringify()`绕过`obj`将其转换为函数。我们可以将这个字符串赋给变量`str`。

这里有一个例子:

```
var obj = {"first_name" : "John", "last_name" : "Doe", "location" : "USA"};var str = **JSON.stringify(obj);**
```

现在，如果我们使用变量`str`，我们将得到作为字符串而不是对象的 JSON。

```
'{"first_name" : "John", "last_name" : "Doe", "location" : "USA"}'
```

# 结论

在向服务器发送数据或从服务器获取数据时，JSON 非常重要。它是 JavaScript 中使用的一种自然格式，并且在许多流行的编程语言中有许多可用的实现。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7) [## 你应该知道的 5 个惊人的前端开发工具

### 每个开发人员都应该知道的有用的前端开发工具

medium.com](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7)