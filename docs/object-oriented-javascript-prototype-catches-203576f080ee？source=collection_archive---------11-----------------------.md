# 面向对象的 JavaScript——原型捕获

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-prototype-catches-203576f080ee?source=collection_archive---------11----------------------->

![](img/46623b4eb957b0693860a9ff3892fc04.png)

Photo by [Jongsun Lee](https://unsplash.com/@sarahleejs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 在一定程度上是一种面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究对象属性。

# 枚举属性

我们可以用 for…in 循环枚举一个对象的属性。

例如，如果我们有:

```
const obj = {
  a: 1,
  b: 2
};
```

我们可以写道:

```
const obj = {
  a: 1,
  b: 2
};for (const key in obj) {
  console.log(key, obj[key]);
}
```

然后我们得到:

```
a 1
b 2
```

`key`为键，`obj[key]`为值。

不是所有的属性都是可枚举的，我们可以设置`ebumerable`属性描述符使属性不可枚举。

我们可以通过`propertyIsEnumerable()`方法来检查一个属性是否可以枚举。

我们可以通过以下方式进行检查:

```
console.log(obj.propertyIsEnumerable('a'))
```

然后我们应该得到`true`因为对象属性是可枚举的，除非另有说明。

# 使用 isPrototypeOf()方法

对象也有`isPrototypeOf`方法，告诉特定的对象是否被用作另一个对象的原型。

例如，如果我们有:

```
const monkey = {
  hair: true,
  feeds: 'bananas',
  breathes: 'air'
};function Person(name) {
  this.name = name;
}Person.prototype = monkey;const james = new Person('james');
console.log(monkey.isPrototypeOf(jane));
```

然后控制台日志应该记录`true`，因为我们将`monkey`设置为`Person`构造函数的原型。

# __proto__ Property

自从 ES6 以来,`__proto__`属性是一个标准属性，让我们设置一个对象的原型。

它也可以用作获取属性的吸气剂。

例如，我们可以写:

```
const monkey = {
  hair: true,
  feeds: 'bananas',
  breathes: 'air'
};function Person(name) {
  this.name = name;
}Person.prototype = monkey;const james = new Person('james');
console.log(james.__proto__ === monkey);
```

然后我们得到`true`，因为我们已经有了`james`的原型，对象就是我们设定的`monkey`:

```
Person.prototype = monkey;
```

我们可以通过以下方式获得相同的结果:

```
james.constructor.prototype
```

我们也可以用它来设定一个物体的原型，这样我们就可以写:

```
const monkey = {
  hair: true,
  feeds: 'bananas',
  breathes: 'air'
};const person = {
  name: 'bob',
  __proto__: monkey
}
```

然后我们得到具有`__proto__`属性的原型，或者我们可以使用`Object.getPrototypeOf()`来检查:

```
Object.getPrototypeOf(person)
```

然后我们得到`monkey`对象。

# 扩充内置对象

我们可以向内置对象添加功能。

我们可以使用聚合填充向某些环境中不可用的对象添加标准功能。

# 原型的问题

我们应该知道原型链是活动的。

当我们改变`proottype`对象时，它就会改变。

`prototype.constructur`房产不可用。

我们可以随心所欲地设置`constructor`顶部。

如果我们改变原型，那么我们必须改变构造函数。

例如，我们可以写:

```
function Dog() {}Dog.prototype = {
  paws: 4,
  hair: true
};const dog = new Dog();
console.log(dog.constructor === Dog);
```

然后我们得到`false`。

我们必须将构造函数设置为`Dog`使其成为`true`，这样它才能正确报告:

```
Dog.prototype.constructor = Dog;
```

现在:

```
console.log(dog.constructor === Dog);
```

如我们所料。

![](img/084f320f24438534e3498bdc5c875acd.png)

Photo by [Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

使用原型时，我们必须小心。

我们可以设置它，有时我们可能会得到意想不到的结果。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**