# 面向对象的 JavaScript——类的一部分

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-parts-of-a-class-37eceab91f4e?source=collection_archive---------7----------------------->

![](img/be432946f0de6294de876e80e1c550f3.png)

Photo by [George Tseganis](https://unsplash.com/@george_tse?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看 JavaScript 类的各个部分。

# 构造器

`constructor`是一个特殊的方法，用来创建和初始化我们用类创建的对象。

类构造函数可以用`super`方法调用它的父类构造函数。

`super`用于创建子类。

# 原型方法

原型方法是类的原型属性。

它们由一个类的实例继承。

例如，我们可以写:

```
class Car {
  constructor(model, year) {
    this.model = model;
    this.year = year;
  }

  get modelName() {
    return this.model
  }

  calcValue() {
    return "2000"
  }
}
```

在我们的`Car`类中有 2 个原型方法。

一种是`modelName`消气法。

另一种是`calValue`方法。

一旦实例化了类，我们就可以使用它们:

```
const corolla = new Car('Corolla', '2020');
console.log(corolla.modelName);
console.log(corolla.calcValue());
```

我们创建了`Car`实例。

然后我们得到 getter 作为属性。

我们调用`calcValue`来获取值。

我们可以创建具有动态名称的类方法。

例如，我们可以写:

```
const methodName = 'start';class Car {
  constructor(model, year) {
    this.model = model;
    this.year = year;
  } get modelName() {
    return this.model;
  } calcValue() {
    return "2000"
  } [methodName]() {
    //...
  }
}
```

我们将变量`methodName`传递给方括号，使`start`成为方法的名称。

# 静态方法

我们可以添加可以直接从类中运行的静态方法。

例如，我们可以写:

```
class Plane {
  static fly(level) {
    console.log(level)
  }
}
```

我们有了`fly`静态方法。

要运行静态方法，我们可以编写:

```
Plane.fly(10000)
```

# 静态属性

没有办法在类体内定义静态属性。

这可能会添加到 JavaScript 的未来版本中。

# 生成器方法

我们可以在类中添加生成器方法。

例如，我们可以创建一个实例是可迭代对象的类。

我们可以写:

```
class Iterable {
  constructor(...args) {
    this.args = args;
  } *[Symbol.iterator]() {
    for (const arg of this.args) {
      yield arg;
    }
  }
}
```

创建接受可变数量参数的`Iterable`类。

然后我们用`Symbol.iterator`方法迭代`this.args`并返回参数。

`*`表示该方法是一种生成器方法。

所以我们可以通过编写来使用该类:

```
const iterable = new Iterable(1, 2, 3, 4, 5);
for (const i of iterable) {
  console.log(i);
}
```

然后我们得到:

```
1
2
3
4
5
```

我们已经创建了一个`Iterable`类的实例。

然后我们循环遍历迭代器项并记录值。

![](img/d84d0446976f47c6be6a1dd7e84a0705.png)

Photo by [George Tseganis](https://unsplash.com/@george_tse?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

一个类可以有一个构造函数、实例变量、getters、实例方法、静态方法和生成器方法。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**