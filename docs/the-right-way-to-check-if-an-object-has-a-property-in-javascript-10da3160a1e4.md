# 在 JavaScript 中检查对象是否有属性的正确方法

> 原文：<https://javascript.plainenglish.io/the-right-way-to-check-if-an-object-has-a-property-in-javascript-10da3160a1e4?source=collection_archive---------5----------------------->

![](img/bbbab6fc99628470add15052bb51d73c.png)

您可能认为这可以通过使用`in`操作符很容易地完成。

```
const user = {
  name: 'Olivier',
}'name' in user // true
```

`in`操作符的问题在于它检查属性是在指定的对象**中还是在它的原型链**中。这意味着:

```
const user = {
  name: 'Olivier',
}'name' in user // true
'age' in user // false
'constructor' in user // true
```

因为`constructor`实际上是`object`原型的一个属性。

幸运的是，有一个`hasOwnProperty`方法可以指示对象**是否将指定的属性作为自己的属性**。

所以你认为你可以这样使用它:`myObject.hasOwnProperty('id')`但是你猜怎么着？
建议用:`Object.prototype.hasOwnProperty.call(myObject, 'id')`代替。这两种语法有什么区别？

第一个调用对象`hasOwnProperty`方法，而第二个调用对象原型`hasOwnProperty`方法。

区别是什么，为什么要使用第二种语法？

1.  你可以这样用 javascript 创建没有任何原型的对象:
    `const myObject = Object.create(null)`。为什么你会这样做而不是`const myObject = {}`？因为这显然是在引入地图数据结构之前模拟地图数据结构的一种方式(你仍然会得到一个对象，但是没有从原型继承的任何属性或方法)。
2.  您可以覆盖`hasOwnProperty`方法(或任何其他方法),因此:

```
const user = { name: 'Olivier' }
user.hasOwnProperty = null
user.hasOwnProperty('name') // Uncaught TypeError
Object.prototype.hasOwnProperty.call(user, 'name') // true
```

最后一句话。请注意，可以覆盖原型方法:

```
Object.prototype.hasOwnProperty = null
const user = { name: 'Olivier' }
Object.prototype.hasOwnProperty.call(user, 'name') // Uncaught TypeError
```

但是你不应该心甘情愿的这么做，除非你真的知道自己在做什么！