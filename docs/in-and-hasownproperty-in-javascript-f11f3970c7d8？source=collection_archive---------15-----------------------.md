# “in”和“hasOwnProperty”函数在 JavaScript 中的工作方式

> 原文：<https://javascript.plainenglish.io/in-and-hasownproperty-in-javascript-f11f3970c7d8?source=collection_archive---------15----------------------->

## JavaScript 中'和'**中的'**和'**的基本概念，它们的外观以及区别**

![](img/0cb28a9ac5b6c63ca483640438c40128.png)

Photo by [Evgeni Tcherkasski](https://unsplash.com/@evgenit?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在

如果指定的属性在指定的对象或其原型链中，操作符中的**返回 true。**

```
const fruit = { name: 'Mango', type: 'Alfonso', sold:50 };
console.log('name' in fruit); //true
```

## 语法:

> 对象中的属性

*   **属性:**表示属性名或数组索引的字符串或符号。
*   **对象**:检查其(或其原型链)是否包含具有指定名称(属性)的属性的对象

# hasOwnProperty()

**hasOwnProperty()** 方法返回一个布尔值，表明对象是否将指定的属性作为自己的属性(相对于继承它)。

```
const fruit = { name: 'Mango', type: 'Alfonso', sold:50 };
console.log(fruit.hasOwnProperty('name'));//true
```

## 语法:

> object.hasOwnProperty(属性)

*   **属性:**表示属性名或数组索引的字符串或符号。
*   **对象**:检查是否包含指定名称的属性(property)的对象

既然我们已经完成了基本的工作，你一定想知道使用其中一个相对于另一个有什么好处，因为 中的 ***和 ***中的【hasOwnProperty】***()似乎在做同样的事情——我们试图确定一个属性是否是对象的一部分。现在让我们来看看区别。***

![](img/1cde576883631f52b56d12f5a7184c98.png)

Photo by [Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在 vs hasOwnProperty 中

关键区别在于 中的 ***对于继承的属性将返回 true，而***hasOwnProperty()***对于继承的属性将返回 false。***

例如，JavaScript 中的基本对象有一个 [toString 属性](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)，一个[构造器属性](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)和几个其他属性。 运算符中的 ***将为这些属性返回 true，但***hasOwnProperty()***将返回 false。***

```
const fruit = { name: 'Mango', type: 'Alfonso', sold:50 };console.log('name' in fruit); //true
console.log('toString' in fruit); //true
console.log('constructor' in fruit); //trueconsole.log(fruit.hasOwnProperty('name')); //true
console.log(fruit.hasOwnProperty('toString')); //false
console.log(fruit.hasOwnProperty('constructor')); //false
```

# 结束语:

*   中的 ***和 ***都返回布尔值。******
*   使用 中的 ***，首先在当前对象中搜索属性，然后在其原型链中搜索***
*   使用***hasOwnProperty()***，仅在当前对象中搜索该属性，如果没有找到，则返回 false。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****