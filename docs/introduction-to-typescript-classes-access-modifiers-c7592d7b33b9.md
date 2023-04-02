# 类型脚本类简介—访问修饰符

> 原文：<https://javascript.plainenglish.io/introduction-to-typescript-classes-access-modifiers-c7592d7b33b9?source=collection_archive---------6----------------------->

![](img/4981c2cf7a395527233d20941e546788.png)

Photo by [freestocks.org](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

与 JavaScript 一样，TypeScript 中的类是其原型继承模型的特殊语法，这是基于类的面向对象语言中的类似继承。类只是添加到 ES6 中的特殊函数，用来模仿其他语言中的关键字`class`。在 JavaScript 中，我们可以有`class`声明和`class`表达式，因为它们只是函数。所以像所有其他函数一样，有函数声明和函数表达式。TypeScript 也是如此。类充当创建新对象的模板。TypeScript 扩展了 JavaScript 类的语法，然后添加了自己的变体。在本文中，我们将研究如何定义 TypeScript 类以及它们如何相互继承。在本文中，我们将研究 TypeScript 中类成员的访问修饰符。

# 公共、私有和受保护修饰符

## 公共

在 TypeScript 中，类成员可以添加访问修饰符。这让我们可以控制定义类成员的类之外的程序的不同部分对类成员的访问。TypeScript 中类成员的默认访问修饰符是`public`。这意味着没有访问修饰符的类成员将被指定为公共成员。例如，我们可以在下面的代码中使用`public`修饰符:

```
class Person {
  public name: string;
  public constructor(name: string) {
    this.name = name;
  } public getName(): string{
    return this.name;
  }
}const person = new Person('Jane');
console.log(person.getName());
console.log(person.name);
```

在上面的例子中，我们将`Person`类中的所有成员指定为`public`，这样我们就可以在`Person`类之外访问它们。我们可以在`Person`实例上调用`getName`方法，也可以直接从类外部获取`name`字段。`constructor`方法上的`public`访问修饰符是额外的，因为`constructor`应该总是公共的，所以我们可以用它实例化这个类。

![](img/e950689c05716c3a6b0880bffd40db56.png)

Photo by [Dayne Topkin](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 私人和受保护的

当一个类的成员被标记为`private`时，它就不能在包含它的类之外被访问。受保护成员仅在具有受保护成员的类的子类中和具有该成员并标有关键字`protected`的类中可用。例如，如果我们的类中有一个`private`成员，如下所示:

```
class Person {
  private name: string;
  constructor(name: string) {
    this.name = name;
  } public getName(): string{
    return this.name;
  }
}const person = new Person('Jane');
console.log(person.getName());
console.log(person.name);
```

然后，当我们试图访问它时，就像我们对上面的`Person`类中的成员`name`所做的那样，我们会得到一个错误。如果我们尝试编译并运行上面的代码，TypeScript 编译器将不会编译该代码，并给出错误消息“Property 'name '是私有的，只能在类' Person '中访问。(2341)“就像我们对私人成员的期望一样。

TypeScript 通过公共成员的结构来比较类型。如果这两种类型列出了相同的公共成员，则它们被 TypeScript 标记为兼容。然而，对于私有和受保护的成员，情况并非如此。对于私有成员和受保护成员，如果两个类被认为是相等的，那么这两个类必须具有来自同一来源的相同私有成员和受保护成员，它们才能被认为是同一类型。例如，如果我们有以下代码:

```
class Person {
  private name: string;
  constructor(name: string) {
    this.name = name;
  }
}class Human {
  private name: string;
  constructor(name: string) {
    this.name = name;
  }
}const human: Human = new Person('Jane');
```

那么我们将得到错误“类型‘Person’不可赋给类型‘Human’。类型有单独的私有属性“name”声明。(2322)".这意味着因为`Person`和`Human`有相同的私有成员`name`，所以它们不能被认为是相同的类型，所以我们不能将`Person`的实例赋给类型为`Human`的变量。这对于受保护的成员也是一样的，所以如果我们有下面的代码:

```
class Person {
  protected name: string;
  constructor(name: string) {
    this.name = name;
  }
}class Human {
  protected name: string;
  constructor(name: string) {
    this.name = name;
  }
}const human: Human = new Person('Jane');
```

我们会得到同样的错误。但是如果我们像下面的代码那样将`protected`改为`public`，那么它将会工作:

```
class Person {
  public name: string;
  constructor(name: string) {
    this.name = name;
  }
}class Human {
  public name: string;
  constructor(name: string) {
    this.name = name;
  }
}const human: Human = new Person('Jane');
console.log(human.name);
```

如果我们运行上面的代码，我们会在最后一行看到从`console.log`语句记录的‘Jane’。

如果我们的类中有私有成员，那么它们必须在两个类的超类中才能被认为是平等的。例如，我们可以编写以下代码，使`Person`和`Human`类被认为是相同的，同时每个类都有一个公共私有成员`name`:

```
class Animal {
  private name: string;
  constructor(name: string) {
    this.name = name;      
  }
}class Person extends Animal{
  constructor(name: string) {
    super(name);    
  }
}class Human extends Animal{
  constructor(name: string) {
    super(name);    
  }
}const human: Human = new Person('Jane');
console.log(human);
```

在上面的代码中，我们有私有`name`成员的`Animal`类，并且`Person`和`Human`类都扩展了`Animal`类，因此`Human`和`Person`将被认为是相等的，因为它们没有私有成员`name`的单独实现，而是在`Animal`类中有一个公共的`name`成员，它们都继承自这个成员。当我们在最后一行的`human`上运行`console.log`时，我们会看到`Person`对象被记录。

同样，对于受保护的成员，我们可以在下面的代码中做类似的事情:

```
class Animal {
  protected name: string;
  constructor(name: string) {
    this.name = name;      
  }
}class Person extends Animal{
  constructor(name: string) {
    super(name);    
  } getName() {
    return this.name;
  }
}class Human extends Animal{
  constructor(name: string) {
    super(name);    
  } getName() {
    return this.name;
  }
}const human: Human = new Person('Jane');
console.log(human.getName());
```

在上面的代码中，我们有受保护的成员`name`，它可以被它的子类`Human`和`Person`访问，所以我们可以用每个类上的`getName`方法返回`name`成员的值和`Animal`类中`name`成员的值。我们需要这个方法，因为受保护的成员只能在拥有受保护成员的类和拥有该成员的类的子类中使用。如果我们运行上面的代码，我们将从上面代码最后一行的`console.log`输出中得到‘Jane’。

在 TypeScript 中，类成员可以应用访问修饰符。如果未指定任何内容，Public 是成员的默认访问修饰符。如果两个类都具有相同的公共成员，或者它们从相同的源继承受保护成员和私有成员并具有相同的公共成员，则认为它们是相等的。