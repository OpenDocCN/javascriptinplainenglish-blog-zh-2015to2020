# 可维护的 JavaScript——意外的全局变量

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-accidental-globals-3c1f68e6328b?source=collection_archive---------14----------------------->

![](img/58ab2dbf3e9b45b70018dfc1a273251a.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过避免意外创建全局变量来了解创建可维护 JavaScript 代码的基础。

# 避免意外全局

我们应该避免意外的全局变量。

如果我们正在编写 JavaScript 脚本，那么如果我们在没有使用任何关键字的情况下给变量赋值，我们将默认创建全局变量。

例如，如果我们有:

```
count = 10;
```

那么`count`就是一个全局变量。

如果我们有一个像 JSHint 或 ESLint 这样的 linter，那么我们会看到一个警告。

此外，严格模式将防止我们意外地创建全局变量。

所以如果我们有:

```
'use strict';
count = 10;
```

然后我们会得到一个错误。

如果我们运行上面的代码，我们会得到“未捕获的引用错误:计数未定义”。

几乎所有现代浏览器都有严格模式，所以我们应该使用它。

默认情况下，模块有严格模式，所以如果我们试图创建新的全局变量，我们总是会得到错误。

现有的全局变量应该被视为只读的。

我们不应该添加任何新的属性，以避免错误。

例如，如果我们使用像`window`或`document`这样的全局变量，那么我们不应该给它们设置任何属性。

如果我们使用较旧的代码，我们应该尽可能地更新它们，并启用严格模式。

# 一个全局对象

许多库为我们提供了它们自己的全局对象，我们可以在代码中使用它们。

jQuery 有`$`和`jQuery`对象。

添加后者是为了与其他使用`$`的库兼容。

Vue 有一个`Vue`全局变量让我们创建一个新的 Vue 实例。

我们创建一个具有唯一名称的全局对象，这样它就不太可能与应用程序中的其他库冲突。

例如，我们可以通过编写以下代码来创建自己的构造函数:

```
function Person(name) {
  this.name = name;
}Person.prototype.speak = function(speech) {
  console.log(`${this.name}: ${speech}`)
};const james = new Person("james");
const mary = new Person("mary");
const jane = new Person("jane");
```

我们用`speak`原型方法创建了一个`Person`构造函数。

它接受`name`参数并将其分配给`this.name`。

此外，它还有`speak`实例方法。

然后我们可以用`new`操作符来使用它。

这会创建许多全局范围的变量。

我们没有把它们都放在全局范围内，而是放在一个对象中，这样它们就不再是全局的了。

例如，我们可以写:

```
const obj = {};obj.Person = function(name) {
  this.name = name;
}obj.Person.prototype.speak = function(speech) {
  console.log(`${this.name}: ${speech}`)
};const james = new obj.Person("james");
const mary = new obj.Person("mary");
const jane = new obj.Person("jane");
```

我们将`Person`构造函数放在`obj`对象中，这样`Person`构造函数就不在全局范围内。

这样，我们就不会不小心更改或覆盖它。

![](img/6599a211e0035836ad222f007f550c69.png)

Photo by [Andrew Butler](https://unsplash.com/@drewbutler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们把代码放在一个对象中，这样它们就不会在全局范围内。

同样，在严格模式下应该避免意外的全局变量。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**