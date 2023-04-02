# 用例子解释 JavaScript ES6 类

> 原文：<https://javascript.plainenglish.io/javascript-es6-classes-explained-with-examples-8d3d6b5717ad?source=collection_archive---------0----------------------->

## 通过实例了解 JavaScript 中的 ES6 类

![](img/8bd4b86b6554cc2558536a723b05727d.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

ES6 类是用于创建 JavaScript 对象的模板。他们用代码封装数据来处理这些数据。然而，JavaScript 类只不过是现有的基于原型的继承和构造函数的语法糖衣。

在本文中，我们将通过一些实际例子来学习 JavaScript 中的 ES6 类。让我们开始吧。

# 类别语法

为了创建一个 JavaScript 类，我们声明了关键字`class`,其中包含了方法`constructor`。当关键字`**new**`被调用来创建一个新对象时，这个构造函数方法被调用。

这里有一个例子:

```
**class** UserInfo {
  **constructor**(name, age) {
    this.name = name;
    this.age = age;
  }
}const newUser = **new** UserInfo("Mehdi", 19);
```

如您所见，我们用构造函数定义了一个类`UserInfo`。然后我们使用关键字`new`调用构造函数来创建我们的对象`newUser`。

注意，按照惯例，应该对 ES6 类名使用 UpperCamelCase，就像上面使用的`UserInfo`一样。

# 向类中添加方法

创建类方法的语法与创建对象方法的语法相同。您可以向您的类中添加任意数量的方法。

```
class ClassName {
  constructor() { ... }
  method_1() { ... }
  method_2() { ... }
  method_3() { ... }
  }
}
```

下面是一个例子:

```
class UserInfo {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  **fullName()**{
    return this.name + ' Aoussiad';
  }}
const newUser = new UserInfo("Mehdi", 19);// We can access the method fullName as we do with objects.
console.log(**newUser.fullName()**);    // Mehdi Aoussiad
```

用关键字`new`创建对象后，你可以像我们处理对象一样访问你的方法。如果需要，还可以向类方法发送参数。

# 静态方法

静态方法是在类中定义的函数，无需创建该类的新对象就可以访问它。因为静态方法是在类级别定义的，所以您可以使用类名直接调用它们。

在 JavaScript 中，必须使用`static`关键字来定义静态方法:

```
**class** UserInfo {
  constructor(speed, age) {
    this.speed = speed;
    this.age = age;
  }

  **static** information(info){
    **return new UserInfo(info, info);**
  }}
const information = UserInfo.**information(20)**;
information.speed; // 20
information.age; // 20
```

# 类继承

用类继承创建的类继承了另一个类的所有方法。要创建一个类继承，使用`**extends**`关键字。

看看下面的例子，我们将创建一个名为“Model”的类，它将继承“Car”类的方法。

```
**class** Car {
  constructor(brand) {
    this.carname = brand;
  }
  present() {
    return 'I have a ' + this.carname;
  }
}

**class** Model **extends** Car {
  constructor(brand, mod) {
    **super**(brand);
    this.model = mod;
  }
  show() {
    return this.present() + ', it is a ' + this.model;
  }
}

let myCar = new Model("Ford", "Mustang");
```

如您所见，关键字`extends`用于确保我们想要继承父类的属性和方法。方法`**super()**` 引用父类。通过调用它，我们可以访问 car 类(父类)的所有属性和方法。

# 分级提升

JavaScript 中的类是不提升的，这意味着你不能在声明一个类之前使用它。

```
//You cannot use the class yet.
//myCar = new Car("Ford")
//This would raise an error.

class Car {
  constructor(brand) {
    this.carname = brand;
  }
}

//Now you can use the class:
let myCar = new Car("Ford")
```

# 结论

如您所见，JavaScript 中的 ES6 类对于可读性和可重用性非常有用和重要。它们提供了用构造函数编写对象的语法糖。比起使用 ES5 构造函数，我更喜欢类。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/javascript-es6-modules-explained-with-examples-3a85e693d56e) [## JavaScript ES6 模块举例说明

### 通过实例了解 JavaScript 中的 ES6 模块

medium.com](https://medium.com/javascript-in-plain-english/javascript-es6-modules-explained-with-examples-3a85e693d56e)