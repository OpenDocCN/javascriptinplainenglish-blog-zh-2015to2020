# 在 JavaScript 中更新和删除对象属性

> 原文：<https://javascript.plainenglish.io/update-and-deleting-properties-from-an-object-in-javascript-af8f01d0983e?source=collection_archive---------0----------------------->

## 你怎么做到的？

![](img/736bdbfdc888beefe45a86e834e51df4.png)

Photo by [Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

首先，我们在 JavaScript 中定义一个对象。

```
var Person = function (name) {
    this.name = name
    this.getName = function() {
        return this.name
    }
    this.go = function () {
        return "I'm walking now."
    }
}
```

## 更新属性

我们可以在任何时候使用赋值来改变属性的值。例如，我们可以像这样更改 *name* 属性的值。

```
person = new Person("Trung Anh Dang")
console.log(person.name)<< Trung Anh Dangperson.name = “Guest”
console.log(person.name)
<< Guest
```

让我们记住，如果对象还没有那个属性名，那么对象会被增强，比如。

```
console.log(person.age)
<< undefinedperson.age = 18
console.log(person.age)
<< 18
```

## 删除属性

就像属性可以随时添加或更新到对象一样，它们也可以被删除。使用*删除*操作符可以从对象中删除任何属性。例如，如果我们想从 *person* 对象中删除 *go* 方法，我们可以输入以下内容。

```
delete person.go
<< trueconsole.log(person.go)
<< undefined
```

请记住，如果我们将一个属性设置为 null，我们实际上并没有从对象中完全删除该属性。这类似于用 null 值调用 update 属性。

```
person.go = null
console.log(person.go)
<< null
```

对于我们来说，从 Javascript 对象中删除和更改属性值太容易了。

## 如果你不知道…

如何…？

*   在 JavaScript 中定义一个对象>[阅读更多…](https://medium.com/javascript-in-plain-english/the-3-ways-to-create-a-javascript-object-33406794e9e5)
*   在 JavaScript 中查找对象的属性>[了解更多…](https://medium.com/javascript-in-plain-english/how-to-detect-properties-of-an-object-in-javascript-754cae0dbe97)

感谢阅读！不要忘记👏跟着走。

## **用简单英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。