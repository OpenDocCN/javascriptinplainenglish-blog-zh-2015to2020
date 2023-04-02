# 使用类型脚本—合并方法和类

> 原文：<https://javascript.plainenglish.io/using-typescript-merging-methods-and-classes-62a280390394?source=collection_archive---------3----------------------->

![](img/b055ba0140a727810caca0a039a486a8.png)

Photo by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将了解如何在 TypeScript 中使用对象和类。

# 合并方法

如果我们有两个具有相同方法名的对象类型，那么 TypeScript 将尝试创建一个函数，其签名是两者的交集。

例如，如果我们有:

```
type Cat = {
  name: string;
  speak: (words: number) => string;
};type Animal = {
  name: string;
  speak: (words: string) => string;
};const person: Cat | Animal = {
  name: "james",
  speak(words) {
    return words;
  }
};
```

然后，我们不能向`words`传递任何东西，因为`words`的类型是`number & string`，这是一个不可能的交集。

因此，不可能向`speak`方法传递任何东西。

# 构造函数

可以用 JavaScript 中的构造函数创建对象。

它们也可以与 TypeScript 代码一起使用，但是支持它们的方式并不直观，也不如处理类的方式优雅。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
}
```

在 JavaScript 中创建一个构造函数。

在 TypeScript 中，如果我们有了`noImplicitAny`，我们会因为`this`有了隐式的`any`类型而得到一个错误。

因此，我们需要一种方法来指定`this`的类型，如果我们已经将它设置为`true`。

例如，我们可以写:

```
function Person(this: { name: string }, name: string) {
  this.name = name;
}
```

`this`参数必须是第一个参数。

它不会出现在构建的 JavaScript 代码中。在编译后的 JavaScript 代码中，`name`参数仍将被视为第一个参数。

# 班级

TypeScript 对构造函数没有很好的支持。

它的重点是改进类语法。

对于熟悉其他编程语言的人来说，类语法有助于熟悉 Typescript。

我们必须在 TypeScrtipt 类中声明实例属性及其类型。

这使得类更加冗长，但是它允许构造函数参数类型不同于它们被赋予的实例属性的类型。

使用`new`关键字从类中创建对象。

编译器理解用于收缩类的`instanceof`关键字的用法。

例如，我们可以写:

```
class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}
```

我们在顶层声明了实例变量，并在构造函数中为它赋值。

# 访问控制关键字

TypeScript 提供访问控制关键字来控制对对象实例属性的访问。

这是 JavaScript 中所没有的。

有 3 个访问控制关键字。

`public`允许自由访问一个属性或方法，这是默认的关键字。

限制对定义属性或方法的类的访问。

`protected`限制对定义属性或方法的类及其子类的访问。

我们在实例变量名称前添加了访问控制关键字:

```
class Person {
  public name: string;
  constructor(name: string) {
    this.name = name;
  }
}
```

构建的 JavaScript 代码不会有这些关键字，所以我们不能依赖它们从外部屏蔽数据。

# 确保实例属性已初始化

可以将`strictPropertyInitialization`编译器选项设置为`true`，以确保初始化所有实例属性。

应该启用`strictNullCheck`选项才能工作。

# 只读属性

`readonly`关键字可用于创建实例属性，其中值仅在构造函数中赋值。

它不能在其他地方赋值。

例如，我们可以写:

```
class Person {
  public name: string;
  readonly id: number;
  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}
```

我们添加了关键字`readonly`,就像我们添加访问修饰符一样。

![](img/9f105225e0660983925b83e542e53ce3.png)

Photo by [mana5280](https://unsplash.com/@mana5280?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 简化类构造函数

我们可以通过将实例变量移到构造函数签名中来简化类构造函数。

例如，我们可以写:

```
class Person {
  constructor(readonly id: number, public name: string) {}
}
```

这和我们之前的例子完全一样。

只是短了点。

所以如果我们如下实例化它:

```
const person = new Person(1, "james");
```

我们得到同样的结果。

# 结论

当我们创建交集类型时，TypeScript 将合并来自类的方法。

此外，TypeScript 对 JavaScript 类语法进行了增强，使一切更加清晰。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**