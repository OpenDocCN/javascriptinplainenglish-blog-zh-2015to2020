# JavaScript 基础——原型

> 原文：<https://javascript.plainenglish.io/javascript-basics-prototypes-bd5ba0663157?source=collection_archive---------6----------------------->

![](img/c8262b67748a735e23ea41d570baaf82.png)

Photo by [Ivan Jevtic](https://unsplash.com/@ivanjevtic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究 JavaScript 原型。

# 原型

原型是其他对象继承的对象。

这是 JavaScript 中继承的方式。

例如，如果我们创建一个空对象:

```
const empty = {};
```

我们可以在上面调用`toString`方法。

我们可以写:

```
console.log(empty.toString());
```

如果我们调用它，我们会在控制台日志中记录`[object Object]`。

即使我们的对象没有定义任何东西，它们仍然有继承的属性。

`toString`方法来自`Object.prototype`属性，默认情况下，丢失的对象从该属性继承。

如果一个属性不在对象本身中，那么 JavaScript 解释器将在它的原型中搜索该属性。

我们可以使用`getPrototypeOf`方法来获得一个对象的原型。

例如，我们可以写:

```
console.log(Object.getPrototypeOf(empty));
```

我们得到一个记录了一堆函数的对象。

如果我们记录了`empty`对象，我们可以看到`__proto__`属性也有同样的内容。

许多对象并不直接继承自`Object.prototype`。

例如，函数继承自`Function.prototype`，而后者继承自`Object.prototype`。

数组继承自`Array.prototype`，而`Array.prototype`继承自`Object.prototype`。

# 对象.创建

我们可以使用`Object.create`来创建一个对象，并把原型传递给方法。

例如，我们可以写:

```
const protoDog = {
  speak(words) {
    console.log(words);
  }
}const dog = Object.create(protoDog);
```

然后我们看到，当我们记录`dog`的值时，我们看到了带有`speak`方法的`__proto__`属性，这就是我们在`protoDog`中拥有的。

# 班级

JavaScript 中的类只不过是其原型继承模型之上的语法糖。

一个类通过列出它的方法和属性来定义一种对象的形状。

从类创建的对象称为类的实例。

要创建一个给定类的实例，我们必须创建一个从正确原型派生的对象。

类只是语法更好的构造函数。

我们可以定义构造函数以 fikkiwsL 的形式返回一个对象

```
function Dog(name) {
  this.name = name;
}Dog.prototype.speak = function(words) {
  console.log(`${this.name} says '${words}'`);
};
```

我们的原型中有带有`speak`方法的`Dog`构造函数。

然后我们可以通过编写以下代码来创建一个`Dog`实例:

```
const dog = new Dog('james');
dog.speak('hello');
```

所有函数都会自动获得一个名为`prototype`的属性，我们可以将实例方法放入其中。

构造函数的名称被大写，这样就可以与其他函数区分开来。

如果我们检查原型，我们会得到:

```
console.log(Object.getPrototypeOf(dog) === Dog.prototype);
```

日志`true`。

# 更好的语法

为了让我们的生活更简单，我们可以使用类语法来重写我们的`Dog`构造函数。

例如，我们可以写:

```
class Dog {
  constructor(name) {
    this.name = name;
  } speak(words) {
    console.log(`${this.name} says ${words}`);
  }
}
```

一个类以`class`关键字开头。

然后我们用`constructor`来代替构造函数体。

`speak`方法现在在类中，而不是将其添加到`prototype`属性中。

类声明只允许将方法添加到原型中。

类可以用在语句和表达式中，因为它们只是构造函数。

所以我们可以写下这样的东西:

```
const obj = new class {
  hello() {
    return "hello";
  }
};
```

我们创建了一个`class`表达式，它可以立即与`new`关键字一起使用。

那么我们可以这样称呼`obj.hello`:

```
console.log(obj.hello());
```

![](img/655966126bd4d9b0cb7a0b134b73e4c5.png)

Photo by [Maarten Deckers](https://unsplash.com/@maartendeckers?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 覆盖继承的属性

我们可以通过使用`extends`关键字来覆盖继承的属性。

例如，我们可以通过编写以下内容从一个类创建一个子类:

```
class Dog {
  constructor(name) {
    this.name = name;
  }
  speak(words) {
    console.log(`${this.name} says ${words}`);
  }
}class GoodDog extends Dog {
  speak(words) {
    console.log(`good dog ${this.name} says ${words}`);
  }
}
```

我们创建了`GoodDog`类，它是`Dog`的子类。

`extends`关键字表示我们继承了`Dog`原型的所有属性。

# 结论

JavaScript 使用原型继承。

我们继承来自对象原型的属性。

大多数物体都以`Object.prototype`为基本原型。

要创建具有固定成员集的对象，我们可以使用构造函数。

为了使创建构造函数更容易，我们可以使用类语法。

## 简单英语中的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似的内容吧！**