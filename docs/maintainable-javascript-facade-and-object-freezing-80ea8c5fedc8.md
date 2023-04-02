# 可维护的 JavaScript——外观和对象冻结

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-facade-and-object-freezing-80ea8c5fedc8?source=collection_archive---------17----------------------->

![](img/cc09f0d4d9f5ab4e77f29c158436fd73.png)

Photo by [Stephen Bergin](https://unsplash.com/@steve_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看一些设计模式来了解创建可维护的 JavaScript 代码的基础。

# 立面图案

facade 模式是一种设计模式，它允许我们为现有对象创建新的接口。

外观也称为包装器，因为它们用不同的接口包装现有的对象。

例如，我们可以用操作 DOM 对象的方法创建一个类。

我们可以写:

```
class DOMWrapper {
  constructor(element) {
    this.element = element;
  } addClass(className) {
    element.className += ` ${className}`;
  }
}
```

我们有一个接受一个 DOM 元素的对象的构造函数。

然后我们有一个`addClass`方法让我们将`className`添加到`className`字符串属性中。

然后我们可以通过写来使用它:

```
const wrapper = new DOMWrapper(document.querySelector("div"));
wrapper.addClass("selected");
```

包装类对于扩展现有对象的功能很有用。

我们可以完全控制界面，这样我们就可以控制用户可以使用的功能。

此外，我们可以添加比现有方法更容易使用的新方法。

# 多填充物

当 ES5 +和 HTML5 特性被添加到浏览器中时，Polyfills 变得流行起来。

我们可以使用 polyfills 在我们的应用程序添加到浏览器之前使其功能可用。

内置对象的许多方法，如`forEach`、`map`、`filter`等。的数组是在 JavaScript 的第一个版本之后添加的。

这在旧的浏览器中可能不可用，所以我们必须为不可用的浏览器添加 polyfills。

一旦我们的应用程序支持的浏览器实现了这些特性，我们就不再需要 polyfill 了。

当我们只支持具有本机功能的浏览器时，我们可以轻松地删除它们。

当我们选择聚合填充时，我们应该确保它尽可能地匹配本地版本。

# 防止修改

ES5 增加了一些防止修改对象的方法。

几乎所有现代浏览器都支持它们。

我们可以通过使用`Object.preventExtensions`方法来防止向对象添加属性。

`Object.seal`方法具有`preventExtensions`的功能。并且它防止现有的属性和方法被删除。

`Object.freeze`方法具有`seal`的能力，它防止现有的属性和方法被修改。

所以我们可以写:

```
const person = {
  name: "james"
};
Object.preventExtensions(person);
```

然后我们可以调用`Object.isExtensible`方法来检查对象是否被冻结:

```
console.log(Object.isExtensible(person));
```

如果是`false`，那么就是冻住了。

如果我们试图给它添加一个属性:

```
person.age = 50;
```

除非代码启用了严格模式，否则它会悄无声息地失败。

很可能，我们可以调用`Objec.seal()`来密封一个对象。

然后我们可以写:

```
const person = {
  name: "james"
};Object.seal(person);
console.log(Object.isSealed(person));
```

我们密封了对象，所以`isSealed`应该返回`true`。

现在，如果我们尝试添加或删除属性:

```
person.age = 50;
delete person.name;
```

除非严格模式打开，否则它们都会无声地失败。

同样，我们可以使用`Object.freeze`方法来冻结一个对象。

我们可以写:

```
const person = {
  name: "james"
};Object.freeze(person);
```

然后，我们可以检查是否可以通过编写以下代码来修改我们的对象:

```
console.log(Object.isExtensible(person));
console.log(Object.isSealed(person));
console.log(Object.isFrozen(person));person.name = "mary";
person.age = 25;
delete person.name;
```

`isExtensible`应该返回`false`，另外 2 个应该是`true`。

如果关闭了严格模式，对象修改代码应该会自动失败。

如果打开了严格模式，那么我们应该会看到错误消息。

![](img/4c8edce1651f89fae0f862b207ec19d1.png)

Photo by [matthaeus](https://unsplash.com/@matthaeus123?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有各种各样的静态方法来阻止我们改变我们的对象。

此外，我们可以围绕现有代码创建一个外观，为它们创建一个新的接口。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**