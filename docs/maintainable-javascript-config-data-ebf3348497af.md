# 可维护的 JavaScript —配置数据

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-config-data-ebf3348497af?source=collection_archive---------15----------------------->

![](img/4d16b84ca5c757398b383e1d4753c916.png)

Photo by [NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看外部化配置数据的方法来了解创建可维护的 JavaScript 代码的基础。

# 检测属性的好方法

我们可以使用`in`操作符来检查对象属性是否存在于对象中。

例如，我们可以写:

```
const object = {
  count: 0,
};if ("count" in object) {
  // ...
}
```

检查`count`属性是否被添加到了`object`对象中。

`if`块标题中的表达式将返回`true`，因此块将运行。

这将检查`count`属性是否在对象本身中，以及是否在它的原型链中。

为了检查一个属性是否是一个对象的非继承属性，我们可以使用`hasOwnProperty`方法。

例如，我们可以写:

```
const object = {
  count: 0,
};if (object.hasOwnProperty('count')) {
  // ...
}
```

如果`'count'`是`object`的自身属性，则返回`true`，否则返回`false`。

如果我们不确定`hasOwnProperty`是否存在于`object`中，我们可以写:

```
if ("hasOwnProperty" in object && object.hasOwnProperty('count')) {
  // ...
}
```

现在我们在调用`hasOwnProperty`之前就确定它存在了。

# 将配置数据与代码分开

配置数据是我们应用程序中的任何硬编码值。

如果我们有:

```
function validate(value) {
  if (!value) {
    console.log("Invalid value");
    location.href = "/errors/invalid";
  }
}
```

然后我们的代码中有 2 个配置数据。

一个是`'Invalid value'`字符串。

第二个是`'/error/invalid'` URL 字符串。

URL 和消息可能会改变，所以我们可以将它们分开，这样我们就可以为每一个定义一个可重用的变量，然后在其他地方引用它。

作为配置数据的数据包括:

*   资源定位符
*   用户界面中显示的字符串
*   重复的唯一值
*   设置
*   任何可能改变的值

我们不想仅仅为了改变一些配置值而修改源代码的多个部分。

# 外部化配置数据

将配置数据从代码中分离出来的第一步是将配置数据外部化。

这意味着从 JavaScript 代码中间获取数据。

我们写下的不是之前的内容，而是:

```
const config = {
  MESSAGE_INVALID_VALUE: "Invalid value",
  URL_INVALID: "/errors/invalid.php",
};function validate(value) {
  if (!value) {
    console.log(config.MESSAGE_INVALID_VALUE);
    location.href = config.URL_INVALID;
  }
}
```

我们创建了一个`config`对象，

然后我们在代码中引用了它。

`config`中的每个属性都是一段数据。

该属性是大写的，以便我们可以将它们与其他属性区分开来。

最重要的部分是将数据外部化。

剩下的就看我们自己的喜好了。

![](img/614ebcb0b0ca4a54c950265631aa7fa7.png)

Photo by [Stephen Dawson](https://unsplash.com/@srd844?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

配置数据是在多个地方使用的硬编码数据。

我们应该将配置数据具体化，这样我们就可以在多个地方使用它们，而不会重复。

这样一来，我们可以一次改完，不用担心。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**