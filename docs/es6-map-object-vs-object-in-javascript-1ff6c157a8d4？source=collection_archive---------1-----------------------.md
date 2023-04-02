# JavaScript ES6 功能—地图对象

> 原文：<https://javascript.plainenglish.io/es6-map-object-vs-object-in-javascript-1ff6c157a8d4?source=collection_archive---------1----------------------->

![](img/99d35c301493b3209807976baf6d3211.png)

Photo by [Atikh Bana](https://unsplash.com/@tikh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# ES6 映射对象与对象

> ES6 之前的 JavaScript 开发者就像:
> 
> 在 JavaScript 中，不能保证对象中的属性顺序。需要使用一个数组来保持它的顺序”

> 但是随着 ES6 的引入，事情发生了变化

![](img/e3c926c796829c412e904ac52afc145d.png)

Photo by [Natasha Welingkar](https://unsplash.com/@thefriscobay?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# ES6 之前的对象

*   让我们来看看*对象*的定义

*对象是属性的无序集合*

*   对象是一个**无序的**集合
*   因此，在对象中不能保证属性顺序

> 完成了吗？？

![](img/c12e1bb089d8f4a731d7b7a8930f5263.png)

Photo by [Jon Tyson](https://unsplash.com/@jontyson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# ES6 之后的对象

*虽然* ***ES5*** *没有明确指定顺序，但是****ES6****在某些情况下引入了对象的顺序*

> 我们来看看下面的样品？？我循环遍历对象
> 
> 属性顺序在某种程度上得以保留

> 这个呢？？你注意到什么了吗？？

## 你可能已经注意到了

*   保留对象的插入顺序；**整数键的**除外(如“1”、“2”、“3”)

# 插入顺序保存在**某组规则**中

1.  **升序排列的整数键(以及解析为整数的字符串“1”)**
2.  **字符串键，按插入顺序**
3.  **符号名称，按插入顺序排列**

> 符号到底是什么？

> *符号是 JavaScript 的原始数据类型，还有字符串、数字、布尔、空和未定义……*更多信息可从[https://flaviocopes.com/javascript-symbols/](https://flaviocopes.com/javascript-symbols/)获得

# 这套规则

*   包括 Chrome / v8、Safari 在内的**【大多数】**浏览器都遵循**这套规则**
*   并且**所有的浏览器都遵循规则 **2。& 3。**以上**

> **但是“大多数”并不能满足“所有”JavaScript 开发人员**

**![](img/b9fea30723ed550ec2b2cc639107f37c.png)**

**Photo by [Matthew Henry](https://unsplash.com/@matthewhenry?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

> **另外，**对象中的整数键(ES6 之后)没有按照插入顺序排序，我们想要 100%保证保留其插入顺序****

# **ES6 的新特性来了，保证 100%的插入顺序**

# **地图对象**

***你不应该依赖* ***普通对象的属性顺序，因为它们*容易出错****

## **自 ECMAScript 2015 以来，**地图对象**可能是一种替代方案**

*****贴图对象*** *与普通对象**有一些相似之处***

# *****地图对象**和**普通对象**的区别***

## *****>插入顺序*****

*   *****映射** **对象**保存键值对，并记住键的原始插入顺序***
*   *****正常对象**不记得钥匙的插入顺序***

## *****>按键类型*****

*   ***在**地图对象**中，任何值(对象和原始值)都可以用作关键字或值***
*   ***在**正常对象**中，只有一个`String`或一个`Symbol`作为关键点***

## *****>尺寸*****

*   ***在**地图对象**中，`Map`中的项目数量很容易从其`size`属性中检索到***
*   ***在**正常对象**中，`Object`的项数必须手动确定***

## *****>迭代*****

*   *****默认情况下，映射**是可迭代的***
*   ***在**普通对象**中，迭代一个`Object`需要以某种方式获得它的键并迭代它们(即`Object.keys()`***

> ****我写了一篇关于 JavaScript 中 5 种不同对象迭代方法的文章****
> 
> ***[*https://medium . com/JavaScript-in-plain-English/performance-test-for-5-JavaScript-object-iterations-587 df 56 f 5266*](https://medium.com/javascript-in-plain-english/performance-test-for-5-javascript-object-iterations-587df56f5266)***

## *****>性能*****

*   *****Map** 在频繁添加和删除键值对的场景中表现更好***
*   *****普通对象**没有针对频繁添加和删除键值对进行优化***

## ***使用**贴图对象**重写上面的例子***

*****> >例 1*****

*****> >例 2*****

*   ***`for ... of`循环为每次迭代返回一个`[key, value]`数组***

# ***设置地图对象中的条目***

***![](img/fb128763ae7160d2298b8425a5f93ff7.png)***

***Photo by [Henrikke Due](https://unsplash.com/@henrikkedue?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)***

****厌倦了手工。集合运算？****

```
*let map = new Map();map.set("foo", "foo");map.set("5", "5");// ... forever ???*
```

***![](img/5f16f78ac39a296f20a4dc98e891b042.png)***

***Photo by [Xavi Cabrera](https://unsplash.com/@xavi_cabrera?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)***

# ***在地图对象中设置条目的两种不同方法***

*****> 1)地图对象内部的嵌套数组(2d-array)*****

```
*let map = new Map( [
    ["foo", "foo"],
    ["5", "5""],
    ... ])*
```

```
***let array = 
  [ ["foo", "foo"], ["5", "5"],
    ...]***
```

# *****迭代地图对象的两种不同方法*****

*******> 1)用** `**for .. of**`迭代 `**Map**`*****

```
*****let myMap = new Map()
myMap.set(0, 'zero')
myMap.set(1, 'one')

for (let [key, value] of myMap) {
  console.log(key + ' = ' + value)
}
// 0 = zero
// 1 = one

for (let key of myMap.keys()) {
  console.log(key)
}
// 0
// 1

for (let value of myMap.values()) {
  console.log(value)
}
// zero
// one

for (let [key, value] of myMap.entries()) {
  console.log(key + ' = ' + value)
}
// 0 = zero
// 1 = one*****
```

*********> 2)用** `**forEach()**`迭代 `**Map**`*******

```
*******myMap.forEach(function(value, key) {
  console.log(key + ' = ' + value)
})
// 0 = zero
// 1 = one*******
```

> *******感谢您的阅读:)*******

*******![](img/5d922c22c2f4709de82f9d7f91ac5181.png)*******

*******Photo by [Anthony DELANOIX](https://unsplash.com/@anthonydelanoix?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*******