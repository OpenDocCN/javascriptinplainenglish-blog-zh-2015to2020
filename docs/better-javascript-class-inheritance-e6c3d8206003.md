# 更好的 JavaScript —类继承

> 原文：<https://javascript.plainenglish.io/better-javascript-class-inheritance-e6c3d8206003?source=collection_archive---------10----------------------->

## 用实例理解 JavaScript 中的类继承

![](img/fc4e145882e75fb11b7d99803fa0c8ad.png)

Photo by [Adeolu Eletu](https://unsplash.com/@adeolueletu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

在现代 JavaScript 中，类是创建对象的模板。它们建立在原型的基础上，并且它们提供了一个简单的语法糖来为 JavaScript 中的面向对象编程编写构造函数。除此之外，类继承使得从另一个类继承方法更加容易，而不必使用原型方法。

在本文中，我们将介绍 JavaScript 中的类继承。但是首先，让我们看看如何创建类。

![](img/0f2699cefd9c3c65827b93f00e12cc70.png)

Photo by [Adeolu Eletu](https://unsplash.com/@adeolueletu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 创建一个 JavaScript 类

要创建一个 JavaScript 类，你必须使用关键字`**class**` 并使用其中的构造函数方法。看看下面的例子:

```
class Car {
  constructor(name, year) {
    this.name = name;
    this.year = year;
  }
}
```

现在，您可以使用该类创建一个名为“myCar”的新对象，示例如下:

```
let myCar = new Car("Ferrari", 2020);
```

# 类继承

用类继承创建的类继承了另一个类的所有方法。要创建一个类继承，使用`**extends**`关键字。看看下面的例子，我们将创建一个名为“Model”的类，它将继承“Car”类的方法。

```
class Car {
  constructor(brand) {
    this.carname = brand;
  }
  present() {
    return 'I have a ' + this.carname;
  }
}

class Model extends Car {
  constructor(brand, mod) {
    super(brand);
    this.model = mod;
  }
  show() {
    return this.present() + ', it is a ' + this.model;
  }
}

let myCar = new Model("Ford", "Mustang");
```

**方法**指的是父类。通过调用它，我们可以访问 car 类(父类)的所有属性和方法。

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

JavaScript 中的类对于可读性和可重用性非常有用。继承使得在不使用原型的情况下从其他类继承方法和属性变得更加容易。感谢阅读这篇文章，我希望你觉得有用。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**