# 现代 JavaScript 的精华—子类

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-child-classes-316aa9e59358?source=collection_archive---------12----------------------->

![](img/b4f0fcb992a20a953d9fd95d9bf2bc6b.png)

Photo by [Caleb Woods](https://unsplash.com/@caleb_woods?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何用 JavaScript 定义类。

# 计算方法名称

我们可以添加带有计算属性名的方法。

例如，我们可以写:

```
class Foo {
  ['foo' + 'Bar']() {}
}
```

我们将返回字符串或符号的表达式传递到方括号中，以创建具有给定标识符的方法。

我们也可以通过书写来传递一个符号:

```
class Foo {
  [Symbol.iterator]() {
    //...
  }
}
```

# 生成器方法

我们可以向类中添加生成器方法。

例如，我们可以写:

```
class Iterable {
  constructor(arr) {
    this.arr = arr;
  } *[Symbol.iterator]() {
    for (const a of this.arr) {
      yield a;
    }
  }
}
```

我们用`constructor`创建一个`Iterable`类，用`Symbol.iterator`方法创建一个方法。

这使得我们的`Iterable`实例是可迭代的。

该方法是由`*`符号表示的生成器。

我们使用`yield`来返回和暂停每个项目。

然后，我们可以通过编写以下代码将它与 for-of 循环一起使用:

```
class Iterable {
  constructor(arr) {
    this.arr = arr;
  }*[Symbol.iterator]() {
    for (const a of this.arr) {
      yield a;
    }
  }
}for (const x of new Iterable(['foo', 'bar', 'baz'])) {
  console.log(x);
}
```

我们记录了传入数组的每个条目。

# 子类

我们可以从基类创建子类。

关键字`extends`让我们创建子类。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this.name = name;
  }
  toString() {
    return `(${this.name})`;
  }
}class Employee extends Person {
  constructor(name, title) {
    super(name);
    this.title = title;
  }
  toString() {
    return `(${super.toString()}, ${this.title})`;
  }
}
```

我们用关键字`extends`创建了`Employee`类，从而创建了`Person`的子类。

在`Employee`的`constructor`中，我们调用`super`从那里调用`Person`构造函数。

在我们访问子类中的`this`之前，这是必需的。

我们用`title`参数的值设置`this.title`属性。

在`toString`方法中，我们调用`super.toString`来调用`Person`实例的`toString`。

如果我们创建一个`Employee`的实例，我们可以检查创建它的类。

例如，我们可以写:

```
const emp = new Employee('jane', 'waitress');
console.log(emp instanceof Person);
console.log(emp instanceof Employee);
```

他们都回来了，正如我们所料。

`Person`是基类`Employee`是子类。

# 子类的原型是超类

类语法只是创建构造函数的一种方便的方法，所以它坚持原型继承模型。

子类的原型是超类。

如果我们有:

```
class Person {
  constructor(name) {
    this.name = name;
  }
  toString() {
    return `(${this.name})`;
  }
}class Employee extends Person {
  constructor(name, title) {
    super(name);
    this.title = title;
  }
  toString() {
    return `(${super.toString()}, ${this.title})`;
  }
}
```

然后:

```
console.log(Object.getPrototypeOf(Employee) === Person)
```

日志`true`。

子类从基类继承静态属性。

例如，如果我们有:

```
class Foo {
  static baz() {
    return 'baz';
  }
}class Bar extends Foo {}
```

然后我们可以调用:

```
Bar.baz()
```

我们也可以在子类中调用静态方法:

```
class Foo {
  static baz() {
    return 'baz';
  }
}class Bar extends Foo {
  static baz() {
    return `child ${super.baz()}`;
  }
}
```

![](img/f09e657a7ca54ac97c3f32484be80046.png)

Photo by [Robert Collins](https://unsplash.com/@robbie36?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加计算方法名、静态成员，并从子类中获取父类成员。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**