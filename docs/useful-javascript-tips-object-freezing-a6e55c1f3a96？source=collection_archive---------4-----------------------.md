# 有用的 JavaScript 技巧——对象冻结

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-object-freezing-a6e55c1f3a96?source=collection_archive---------4----------------------->

![](img/d28361d179c9d5b38ae2fd23ad2aa87c.png)

Photo by [Adrien Olichon](https://unsplash.com/@adrienolichon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 检查对象的原型

我们可以使用一个对象的`isPrototypeOf`方法来检查另一个对象是否是一个对象的原型。

例如，如果我们有:

```
const animal = {
  name: 'james'
}const dog = Object.create(animal);
```

然后我们可以通过写来调用`isPrototypeOf`:

```
animal.isPrototypeOf(dog)
```

这个表达式将返回`true`，因为我们通过`animal`进入了`Object.create`。

另一方面，如果我们有:

```
const cat = {};
```

然后:

```
animal.isPrototypeOf(cat)
```

返回`false`。

# 检查对象是否有给定的属性

我们可以通过使用`hasOwnProperty`方法来检查一个对象是否有给定的属性。

它检查一个属性是否是对象的非继承属性。

例如，我们可以写:

```
const person = { name: 'james' };
const hasName = person.hasOwnProperty('name');
```

然后我们可以检查`name`属性是否在带有`hasOwnProperty`的`person`对象中。

散列`hasName`将是`true`，因为`name`是`person`的属性。

# 从对象中获取所有属性值

我们可以用`Object.values()`方法获得一个对象的所有属性值。

例如，我们可以写:

```
const person = { name: 'james', age: 7 };
const values = Object.values(person);
```

那么`values`就是`[“james”, 7]`。

# 设置对象的原型

可以在创建对象后设置对象的原型。

我们可以用`setPrototypeOf`方法来完成。

例如，我们可以写:

```
const dog = {
  breed: 'poodle'
};const animal = {
  name: 'james',
  age: 7
};Object.setPrototypeOf(dog, animal);
```

`setPrototypeOf`是`Object`的静态方法。

`dog`是我们要设置原型的对象。

`animal`是我们想要设定`dog`的原型。

因此，我们可以这样写:

```
console.log(dog.name);
```

然后我们将`'james'`作为`dog.name`的值，因为`dog`从`animal`继承属性。

# 密封物体

密封一个对象意味着我们阻止一个对象接受新的属性。

此外，我们防止属性被删除。

我们可以使用`Object.seal`方法来防止属性改变。

例如，我们可以写:

```
const dog = {
  breed: 'poodle'
};Object.seal(dog);
```

`dog`是密封的，因此我们不能在其中添加或删除属性。

还有比`seal`做得更少的`Object.freeze`方法。

除了`seal`所做的，它没有使属性不可写。

例如，我们可以写:

```
const dog = {
  breed: 'poodle'
};Object.freeze(dog);
```

还有一个`preventExtensions`方法不允许添加属性。

我们把它叫做`seal`和`freeze`。

# 使用 Object.keys 方法获取非继承的字符串键

我们可以用`Object.keys`方法从一个对象获得非继承的字符串键。

例如，我们可以这样使用它:

```
const dog = {
  breed: 'poodle'
};
const keys = Object.keys(dog);
```

那么`keys`的值就是`[“breed”]`。

这是一个数组，所以我们可以像处理其他数组一样处理它们。

# 检查物体是否密封

`Object.isSealed`方法让我们检查一个对象是否被密封。

例如，我们可以写:

```
const dog = {
  breed: 'poodle'
};
const isSealed = Object.isSealed(dog);
```

然后我们得到`isSealed`是`false`，因为我们没有在`dog`上调用`Object.seal`。

一旦对象被密封，它就不能被解封。

# 检查物体是否冻结

我们可以使用`Object.isFrozen`方法来检查对象是否冻结。

例如，我们可以写:

```
const dog = {
  breed: 'poodle'
};
const isFrozen = Object.isFrozen(dog);
```

现在我们可以检查一个物体是否冻结。

`isFrozen`应该是`false`，因为`dog`没有冻结。

# 检查我们是否可以向对象添加属性

`Object.isExtensible`方法让我们检查是否可以给一个对象添加一个属性。

除非一个对象被传递到`Object.freeze`、`Object.seal`或`Object.preventExtensions`方法中，否则我们可以给它添加属性。

例如，我们可以通过写下来调用`isExtensible`:

```
const dog = {
  breed: 'poodle'
};
const isExtensible = Object.isExtensible(dog);
```

那么`isExtensible`就是`true`了，因为我们没有密封或者冷冻`dog`。

![](img/796fbf559021631f4b9de2315fcbeab4.png)

Photo by [Євгенія Височина](https://unsplash.com/@eugenivy_reserv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以对我们可能错过的物体做很多事情。

我们可以阻止对象更改或添加/删除属性。

此外，我们可以检查对象是否可以更改。

## 坦白地说

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**