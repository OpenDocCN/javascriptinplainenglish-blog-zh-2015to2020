# JavaScript 中的可选链接

> 原文：<https://javascript.plainenglish.io/optional-chaining-in-javascript-cb6abbb43bb5?source=collection_archive---------7----------------------->

![](img/9c3fc87d9f309adafd34c98fd8b83fb5.png)

可选链接是 JavaScript 中的一个新特性。它实际上是一个 ES2020 规格。可选的链接允许开发人员编写可读性更强、更简洁的代码。

# 这是什么？

可选链接，顾名思义，帮助我们有选择地链接对象的属性。也就是说，我们可以将对象的属性链接到许多级别，而不必明确验证链中的每个引用都是有效的。

# 它解决什么问题？

考虑下面的代码示例。

```
let person = { name: "John Doe", address: { place: "New York", city: "New York" }}
```

我们可以像这样进入这个人的地方

```
let place = person.address.place
```

但是如果缺少`address`属性，这将抛出一个错误。

```
Uncaught TypeError: Cannot read property 'place' of undefined
```

因此，我们必须在访问其子属性之前检查`address`属性是否存在

```
let place = person.address && person.address.place
```

好吧，但是如果我们必须访问`place`的 children 属性呢？我们还必须为位置添加验证检查。

```
let place = person.address && person.address.place && person.address.place.name
```

当我们必须访问深度嵌套的对象或属性时，这就成了一场噩梦。

所以让我们看看如何使用可选链接来解决这个问题。

```
let place = person.address?.place?.name
```

可读性更强、更整洁的🥰

# 它是如何工作的？

`.?`操作符的功能类似于`.`链接操作符，除了当属性或引用是`null`或`undefined`时，表达式用值`undefined`短路。

对于上面的一行代码，它是这样工作的。

首先，它检查`person.address`是否为`nullish`(空或未定义)。如果是 nullish，那么表达式立即返回`undefined`而不执行剩余部分。否则，它继续执行表达式的下一部分。

# 告诉我更多

可选链接是一个很好的特性，可以方便地访问深度嵌套的对象属性，而不需要中间条件检查，但不限于此。

可选链接在其他一些场合变得非常方便。

## 函数调用的可选链接

可选的链接操作符也可以验证函数调用。

```
let message = resultObject.getMessage?.()
```

## 处理可选的回调

可选的链接也可以用来检查是否定义了回调。

```
function fetchApi(*url*, *onSuccess*, *onError*) { *// if error occured* onError?.('An error occured')}
```

# 访问数组项目

它还可以验证数组是否具有指定的索引。

```
let item = arr?.[*1*]
```

# 浏览器支持

在撰写本文时，只有少数浏览器支持可选链接。您可以在此查看详细的浏览器兼容性

*原载于*[*https://jinoantony.com*](https://jinoantony.com/blog/optional-chaining-in-javascript)*。*

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****