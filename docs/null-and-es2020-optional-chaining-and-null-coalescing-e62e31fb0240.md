# ES2020 和 null:可选链接和 Null 合并

> 原文：<https://javascript.plainenglish.io/null-and-es2020-optional-chaining-and-null-coalescing-e62e31fb0240?source=collection_archive---------6----------------------->

![](img/8c3271cb9725593f67316799e78840a8.png)

Plenty of Question Marks

当 Javascript 开发人员想到 ECMA 时，他们会提到 2015 年的变化，这些变化改善了语法，并为语言添加了新功能。然而，ECMA 仍然更新语言！今年早些时候，他们发布了 ES11，其中有两个我最喜欢的工具:可选链接和空合并！

# **简史*null***

*null* 是一种非常有趣的数据类型。它是一个[原语](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)类型，以及字符串、数字、布尔、符号和未定义。(边注:从 ES11 开始，BigInt 现在是另一个原语类型了！)原始类型是不可变的，而对象(包括数组)是可以变异的。

但是， *null* 是奇数的原始输出。当您在任何数据类型上使用`typeof`时，您都会得到一个合理的预期响应:`typeof 3`是一个`Number` , `typeof {foo: "bar"}`是一个`Object`，等等。由于 Javascript 的开发方式，当`typeof`应用于 null*时，它被认为是一个`Object`。查看阿克塞尔·劳施迈尔博士撰写的这篇精彩的[文章](https://2ality.com/2013/10/typeof-null.html)，了解更多细节！*

*null* 是一个假值，大致等于 *undefined (* `undefined == null // TRUE`)，最重要的是[表示故意缺少任何对象值](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null)。当一个值未被定义时，它将总是接收*未定义*的值。当 Javascript 开发人员声明一个变量但希望它为空时，他们会将值设置为 *null* 。 *null* 从不被 Javascript 赋值，只被开发者赋值；如果它没有任何有意义的数据，给出一个空值是一个有用的工具。得到一个空值意味着你的变量被声明并且确实存在，自动通知你不需要调整变量声明，提升，或者范围问题:这个值是可用的，只是没有任何信息。

![](img/753e009e3cd076483ed7b66f079c7f6b.png)

Visual Demonstration

# 可选链接

你遇到过这种情况吗？！

```
const friendData = props.user.friends.map(callback)Uncaught TypeError: cannot read property 'friends' of undefined
```

您的应用程序需要访问一些数据，您依赖一系列方法或属性来评估您真正想要的数据。可选链接是来拯救你的！

可选链接允许您保留嵌套链，而不必添加一大堆条件。如果该属性不能被访问，剩余的方法或属性不求值，返回值是*未定义的*。您所要做的就是在可能为空的值后面添加一个`?`, JS 会完成剩下的工作

```
const friendData = props.user?.friends.map(callback)console.log(friendData)
// undefined
```

然而，这可能会产生误导；我的值声明了，我不希望它被*未定义。这就是 Nullish 合并的用武之地！*

# 无效合并

Nullish 合并将检查一个值是否是 nullish(也就是说，或者是 *null* 或者是 *undefined* )，并允许进行一些其他的评估。使用上面的例子，如果方法链变得*未定义*，新的 nullish 合并操作符`??`将改变值。

```
const friendData = props.user?.friends.map(callback) ?? []console.log(friendData)
// []
```

我可以让这个链计算一个数组，或者空值，这取决于后续数据的需要。这个新操作符也适用于条件句！

```
const value = previousVariable ?? false
```

如果左侧的值为假值，如`"", false, 0`，则不会触发 nullish 合并运算符，但前提是该值为 *null* 或 *undefined* 。

始终关注 ECMAScript 的新特性；ES6 不是 JavaScript 的最终形式！我希望这些新的 ES11 特性对你有所帮助！