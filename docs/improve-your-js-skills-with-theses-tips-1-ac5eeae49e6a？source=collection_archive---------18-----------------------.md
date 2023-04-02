# 通过以下建议提高您的 JavaScript 技能

> 原文：<https://javascript.plainenglish.io/improve-your-js-skills-with-theses-tips-1-ac5eeae49e6a?source=collection_archive---------18----------------------->

![](img/9d442f62331df31d611b8854811dda33.png)

在这篇文章中，我将与你分享一些可以提高你技能的 JavaScript 技巧。

# 避免多重检查的 includes 方法

你可以很容易地替换它

```
if (value === 'a' || value === 'b' || value === 'c') { ... } 
```

有了这个(更干净，你可以避免在你的`if`中有太多的条件)

```
if (['a', 'b', 'c'].includes(value)) { ... }
```

# 双`!`操作符将任何变量转换成布尔值

`!`(非)运算符可以使用两次`!!`。为了将任何变量转换成布尔值(像布尔函数)，当你需要在处理它之前检查一些值时非常方便。

```
const toto = null

!!toto // false
Boolean(toto) // false

if (!!toto) { } // toto is not null or undefined
```

# 可选链接

在 JavaScript 中，在处理对象之前，您需要经常检查对象的某些属性是否存在。很多开发人员都使用这个:

```
const toto = { a: { b: { c: 5 } } }

if (!!toto.a && !!toto.a.b && !!toto.a.b.c) { ... } // toto.a.b.c exist
```

您可以使用*可选链接*来避免使用以上示例中的多重检查。

```
const toto = { a: { b: { c: 5 } } }

if (!!toto.a?.b?.c) { ... } // toto.a.b.c exist

// If the key doesn't exist, it will return undefined
const test = toto.a?.b?.c?.d // undefined
```

# 在 if 中返回值时避免使用 Else

我多次看到这种情况:

```
if (...) { // the condition is not important in this example
  return 'toto'
} else {
  return 'tutu'
}
```

如果要有条件地返回值，可以用以下代码替换上面代码的 else 部分:

```
if (...) { // the condition is not important in this example
  return 'toto'
}

return 'tutu'
```

# 避免 ForEach，使用更多的过滤器，映射，减少，每一个和一些功能

这篇文章给了我最好的建议。作为初学者，我们经常使用 forEach 函数，但是 JavaScript 为您提供了很多选择。此外，这些函数是函数式编程的一部分。

这些功能是什么？我会向你解释什么时候使用它们。

# 过滤器

顾名思义，它允许我们根据您的条件过滤数组中的一些值。

```
const toto = [1, 2, 3, 4]

const evenValue = toto.filter(currentValue => {
   return currentValue % 2 == 0 // remove all value that return false with this condition
}) // [2, 4]
```

# 地图

当您需要将项目中的所有项目转换为另一个项目时，请使用 map。例如，如果您想要转换所有的数字，并将它们乘以 2。

```
const toto = [1, 2, 3, 4]

const valueMultiplied = toto.map(currentValue => {
   return currentValue * 2 // remove all value that return false with this condition
}) // [2, 4, 6, 8]
```

# 减少

最难理解的，不同于其他函数，reduce 还有另外一个东西，`accumulator`。这是什么，什么时候用？

知道是否需要使用`reduce`的一个好方法是:

`Do you need to get a single value from your array ?`

例如，如果你想将一个数组中的所有数字相加为一个值，你将需要`reduce`，而`accumulator`就是总和！和任何值一样，你需要初始化它！

```
const toto = [1, 2, 3, 4]

const sum = toto.reduce((accumulator, currentValue) => {
   return accumulator += currentValue // you always need to return the accumulator
}, 0) // 10
```

# 一些&每个

我的最爱之一，我不是每天都用它们，但是我非常喜欢它们！

`some`将检查您的所有项目，当其中一个项目*符合您的条件*时，它将返回`true`，否则它将返回`false`。

`every`将检查您的所有项目，当其中一个项目*不符合您的条件*时，它将返回`false`，否则它将返回`true`。

但是什么时候使用它们呢？

例如，如果您需要确保您的所有项目都符合条件！

```
const toto = [ 2, 4 ]

if (toto.every(val => val % 2 === 0)) { } // You are sure that your array contains only even value

const falsyToto = [ 2, 4, 5 ]

falsyToto.every(val => val % 2 === 0) // return false since array contain a odd value !
```

你可以在相反的上下文中使用`some`,例如，如果你想确保你的数组包含一个奇数值

```
const toto = [ 2, 4, 5 ]

toto.some(val => val % 2 !== 0) // return true
```

希望你喜欢这篇读物！

🎁如果你在[推特](https://twitter.com/code__oz)上关注我并给我发邮件，你可以免费得到我的新书**被低估的 javascript 技能，改变现状**😁

或者在这里得到它

🎁[我的简讯](https://www.getrevue.co/profile/code__oz)

☕️你可以[支持我的作品](https://www.buymeacoffee.com/CodeoZ)🙏

🏃‍♂️，你可以跟着我👇

🕊 [推特](https://twitter.com/code__oz)

👨‍💻 [Github](https://github.com/Code-Oz)

你可以标记🔖这篇文章！

*更多内容请看*[***plain English . io***](http://plainenglish.io/)