# JavaScript 基础:原型继承

> 原文：<https://javascript.plainenglish.io/javascript-fundamental-prototypal-inheritance-9153ab434aae?source=collection_archive---------1----------------------->

![](img/5fc81d80f3625df8ad10b9d557fdfaf3.png)

[Image Credit](http://www.tokkoro.com/2943280-photography-depth-of-field-sea-water-chains-water-drops.html)

作为一名 JavaScript 开发人员，了解 JavaScript 中的继承是如何工作的是一项基本知识，在面试时会非常方便(提示提示)。今天，我想花点时间介绍一下 JavaScript 中的继承以及 JavaScript 继承的好与坏。

说了这么多，让我们开始吧！

# 遗产

> 什么是遗产？

继承是面向对象编程的四个基本概念之一(*抽象*、*封装*、*继承*和*多态*)。继承使一个对象能够获取另一个现有对象的属性。

> 【JavaScript 有继承性吗？

是啊！具体来说，JavaScript 使用一种称为原型继承的继承类型，这与 Java 等语言使用的经典继承非常不同。

# 原型遗传

> **什么是原型继承？**

为了理解原型继承，我们首先需要理解*原型链*。*原型链*是每个对象与其*原型*之间的一系列链接，原型在某种程度上充当“父对象”,直到链到达对 *null* 的引用。此外，可以使用 *__proto__* 手动设置对象的原型。让我们举个例子:

```
const vehicle = { numWheels: 4}const car = { numDoors: 4}// Setting vehicle as car object's prototype
car.__proto__ = vehicle
console.log(car.numWheels)
=> 4
```

注意，car 对象不包含名为 *numWheels* 的属性，但是当我们将 vehicle 对象设置为其原型时， *car.numWheels* 等于 4。在原型继承中，如果一个属性在原始对象中不存在，JavaScript 将开始从原始对象的原型中搜索该属性，并继续沿着原型链向上移动，直到找到该属性或到达空值。

> **好还是坏？**

这可能因人而异。我个人认为它同时是好的和坏的，原因如下:

## 很好…

1.  简单易行
2.  符合 JavaScript 的动态特性
3.  较少的冗余是代码基础

## 不好…

1.  对于习惯于传统继承(例如 Java)的人来说，可能很难理解
2.  JavaScript 的原型继承使用一种构造器模式，其中对象从另一个对象的构造器继承，而不是从整个对象继承，我认为这更符合原型继承试图实现的目标

如果你像我一样，想从创作者(Brendan Eich)本人那里了解更多关于 JavaScript 是如何设计/创作的，你也可以看看他的[技术演讲](https://www.youtube.com/watch?v=GxouWy-ZE80)。

> “我接到营销指令，要让它看起来像 Java，但又不显得太大。只是这种傻傻的小兄弟语言，对吧？Java 的伙伴。”—Brandan Eich(JavaScript 的创始人)