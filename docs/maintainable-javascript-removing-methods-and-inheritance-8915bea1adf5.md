# 可维护的 JavaScript —删除方法和继承

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-removing-methods-and-inheritance-8915bea1adf5?source=collection_archive---------13----------------------->

![](img/f44f4fd091f94a324eb1d63cd97da520.png)

Photo by [Joe Caione](https://unsplash.com/@joeyc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你想继续使用代码，创建可维护的 JavaScript 代码是很重要的。

在本文中，我们将通过避免改变我们不拥有的对象来了解创建可维护的 JavaScript 代码的基础。

# 不要删除方法

从我们没有创建的对象中移除方法也很容易。

例如，我们可以写:

```
document.getElementById = null;
```

然后我们制造了`document.getElementById` `null`。

`getElementById`现在不再是一个功能。

所以我们不能称之为。

此外，我们用`delete`运算符调用删除属性。

`delete`算子作用于正则对象，所以我们可以写:

```
let person = {
  name: "james"
};
delete person.name;
```

我们去掉了`name`属性，所以`person.name`将是`undefined`。

但是，此运算符对内置的库方法没有影响。

所以如果我们写道:

```
delete document.getElementById
```

那没有效果。

移除现有的方法绝对是一种糟糕的做法。

开发人员希望该对象具有库文档中描述的方法。

现有的代码可能正在使用这些方法，因为每个人都希望它们在那里。

如果我们想删除一个方法，那么我们应该反对它们，这样它们就不会被用于新的代码，并且会从现有的代码中删除。

一旦它们都消失了，我们就可以移除它。

移除将是最后一步。

# 更好的解决方案

修改对象绝对不是一个好主意。

我们应该采用一些常见的设计模式来避免修改我们不拥有的对象。

# 基于对象的继承

扩展现有对象的一种方法是创建一个新的对象，用我们想要的任何对象作为原型。

例如，我们可以写:

```
const person = {
  name: "jane",
  sayName() {
    console.log(this.name);
  }
};const student = Object.create(person);
```

我们称`Object.create`为`person`对象，以此作为`student`对象的原型。

所以我们可以在`student`对象上调用`sayName`，因为它是从`person`继承的:

```
student.sayName();
```

然后我们得到`'jane'`日志。

然后我们可以在`student`上通过书写定义我们自己的`sayName`方法:

```
const person = {
  name: "jane",
  sayName() {
    console.log(this.name);
  }
};const student = Object.create(person);
student.sayName = function() {
  console.log('joe');
}student.sayName()
```

然后我们会看到`'joe'`被记录而不是`'jane'`。

一旦物体被创造出来，我们就拥有它。

因此，我们可以用它做任何我们想做的事情，而不会影响其他代码片段和使人困惑。

# 基于类型的继承

基于类型的继承的工作方式类似于基于对象的继承。

我们从原型继承属性。

但是我们创建子类或构造函数，而不是对象。

有了 ES6，我们可以使用`extends`关键字创建子类。

例如，我们可以通过编写以下代码来创建`Error`的子类:

```
class Person {
  constructor(name) {
    this.name = name;
  }
}class Author extends Person {}
```

我们创建了`Author`类，它是`Person`的子类。

`extends`关键字表明它是一个子类。

这让我们可以灵活地创建对象。

我们可以在子类中定义任何新的属性和方法来扩展`Person`类。

![](img/30e323e5ab81810a973c505e57098f6a.png)

Photo by [Celine Sayuri Tagami](https://unsplash.com/@celine_sayuri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该从我们不拥有的对象中移除方法，这样就不会有人感到困惑。

此外，我们不想破坏现有的代码。

我们可以扩展对象和类来创建我们想要的对象和类。