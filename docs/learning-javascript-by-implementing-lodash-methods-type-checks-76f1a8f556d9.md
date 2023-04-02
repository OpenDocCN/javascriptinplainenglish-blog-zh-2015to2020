# 通过实现 Lodash 方法学习 JavaScript 类型检查

> 原文：<https://javascript.plainenglish.io/learning-javascript-by-implementing-lodash-methods-type-checks-76f1a8f556d9?source=collection_archive---------14----------------------->

![](img/53bab168dfdb1ec292e5cd171df8357e.png)

Photo by [Simone Scholten](https://unsplash.com/@shscholten?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将看看如何实现一些 Lodash 方法来进行类型检查。

# `isBoolean`

Lodash `isBoolean`方法检查一个值是否是布尔值。

我们可以很容易地用`typeof`操作符检查一个值是否是布尔值。例如，我们可以编写以下代码:

```
const isBoolean = val => typeof val === 'boolean'
```

在上面的代码中，我们只是使用了`typeof`操作符来检查`val`是不是布尔型。

那么我们可以这样称呼它:

```
const result = isBoolean(true);
```

`result`是`true`，因为`true`是布尔值。否则，如果我们运行:

```
const result = isBoolean(1);
```

我们得到`result`是`false`，因为 1 不是布尔值。

# `isArrayBuffer`

Lodash `isArrayBuffer`检查一个对象是否是`ArrayBuffer`构造函数的实例。我们可以使用对象的`constructor.name`属性来进行检查。

因此，我们可以编写自己的函数来做同样的事情，如下所示:

```
const isArrayBuffer = val => val.constructor.name === 'ArrayBuffer'
```

在上面的代码中，我们只是使用了`constructor.name`属性来检查一个对象是否是`ArrayBuffer`构造函数的实例。

我们也可以使用`instanceof`操作符来检查`val`是否是`ArrayBuffer`的实例，如下所示:

```
const isArrayBuffer = val => val instanceof ArrayBuffer
```

那么我们可以这样称呼它:

```
const result = isArrayBuffer(new ArrayBuffer());
```

而`result`应该是`true`，因为它是`ArrayBuffer`的一个实例。

# `isArrayLike`

`isArrayLike`方法通过检查`length`属性是否存在以及其值是否大于或等于 0 或者小于或等于`Number.MAX_SAFE_INTEGER`来检查对象是否是类似数组的对象。

根据这些标准，我们可以实现自己的`isArrayLike`函数，如下所示:

```
const isArrayLike = val => typeof val.length === 'number' && val.length >= 0 && val.length <= Number.MAX_SAFE_INTEGER
```

在上面的代码中，我们用`typeof`操作符检查`val.length`是否是一个数字。然后我们检查`val.length`是否在 0 和`Number.MAX_SAFE_INTEGER`之间。

那么如果我们如下运行我们的`isArrayLike`函数:

```
const result = isArrayLike('foo');
```

我们得到`result`是`true`，因为一个字符串有一个介于 0 和`Number.MAX_SAFE_INTEGER`之间的`length`属性。

另一方面，如果我们用 1 如下调用`isArrayLike`:

```
const result = isArrayLike(1);
```

然后我们得到`result`是`false`，因为 1 没有`length`属性。

# `isElement`

方法检查一个对象是否是一个 HTML 元素。由于所有 HTML 元素都是`HTMLElement`构造函数的实例，我们可以使用`instanceof`操作符来检查一个对象是否是该构造函数的实例，如下所示:

```
const isElement = obj => obj instanceof HTMLElement
```

在上面的代码中，我们通过检查`obj`是否是`HTMLElement`的实例来检查它是否是一个元素。

那么当我们如下运行它时:

```
const result = isElement(document.body);
```

我们得到`result`是`true`，因为`document.body`是一个 HTML 元素，这意味着它是`HTMLElement`构造函数的一个实例。

另一方面，如果我们这样称呼它:

```
const result = isElement('foo');
```

然后我们得到`result`是`false`，因为它不是`HTMLElement`对象。

![](img/eced2b4e4423a8efc562bf8fb248d53e.png)

Photo by [Max Saeling](https://unsplash.com/@maxsaeling?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `isEmpty`

Lodash 的`isEmpty`方法检查对象、集合、映射和集合是否为空对象。

如果对象没有可枚举的自身属性，则对象为空。类似于数组的值，如`arguments`，数组，字符串等。被认为是空的是`length`为 0。

如果地图和集合的`length`为 0，则认为它们是空的。

明确定义条件后，我们可以编写自己的函数来执行`isEmpty`检查。

我们可以如下实现自己的`isEmpty`函数:

```
const isEmpty = obj => {
  if (typeof obj.length === 'number') {
    return obj.length === 0;
  } else if (obj instanceof Map || obj instanceof Set) {
    return obj.size === 0;
  } else {
    return Object.keys(obj).length === 0;
  }
}
```

在上面的代码中，我们检查`length`属性是否是一个数字，以检查`obj`是否是一个类似数组的对象。如果是，那么我们返回`obj.length === 0`。

如果`obj`是`Map`或`Set`构造函数的实例，我们可以检查`size`属性而不是`length`属性。

否则，我们使用带有`obj`的`Object.keys`作为参数，获取`length`属性并检查它是否为 0。

那么如果我们这样称呼它:

```
const result = isEmpty('');
```

我们得到`result`是`true`，因为一个空字符串的长度为 0。

# 结论

我们可以用`typeof`操作符对原始值进行类型检查。对于对象，最好使用`instanceof`操作符以字符串形式获取构造函数的名称。

为了获得类似数组的对象的大小，我们使用了`length`属性。否则，我们对`Map`和`Set`实例使用`size`属性。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的中级用户名和您感兴趣的内容，我们将会回复您！****