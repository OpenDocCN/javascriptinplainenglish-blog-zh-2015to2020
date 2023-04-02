# 使用流类进行 JavaScript 类型检查

> 原文：<https://javascript.plainenglish.io/javascript-type-checking-with-flow-classes-877e3e4a85f4?source=collection_archive---------7----------------------->

![](img/2c32467c6dcc0864bd99a809957fb8ea.png)

Photo by [Van Tay Media](https://unsplash.com/@vantaymedia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Flow 是一个由脸书开发的类型检查器，用于检查 JavaScript 数据类型。它有许多内置的数据类型，我们可以用它们来注释变量和函数参数的类型。

在本文中，我们将看看如何向类中添加流类型。

# 类别定义

在 Flow 中，定义类的语法与普通 JavaScript 相同，但是我们添加了类型。

例如，我们可以写:

```
class Foo {  
  name: string;
  constructor(name: string){
    this.name= name;
  }    foo(value: string): number {  
    return +value;
  }
}
```

来定义`Foo`类。常规 JavaScript 类和具有流语法的类之间的唯一区别是在字段和参数中添加了类型注释，以及方法的返回值类型。

在上面的代码中，字段的类型注释是:

```
name: string;
```

`value`参数还添加了一个类型注释:

```
value: string
```

我们在`foo`方法的签名后有了`number`返回类型注释。

我们也可以定义没有类内容的类类型定义，如下所示:

```
class Foo {  
  name: string;
  foo: (string) => number;
  static staticField: number;
}
```

在上面的代码中，我们有一个字符串字段`name`，一个接受字符串并返回数字的方法`foo`，以及一个静态的数字`staticField`。

然后，我们可以在类定义之外为每个设置值，如下所示:

```
class Foo {  
  name: string;
  foo: (string) => number;
  static staticField: number;
  static staticFoo: (string) => number;
}const reusableFn = function(value: string): number {
  return +value;
}Foo.name = 'Joe';
Foo.prototype.foo = reusableFn
Foo.staticFoo = reusableFn
Foo.staticField = 1;
```

JavaScript 中的实例方法对应于其原型的方法。静态方法是在所有实例之间共享的方法，就像在其他语言中一样。

![](img/7d0f31303d5b93e5afcdc1ff6cc92237.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 无商标消费品

我们可以将泛型类型参数传递给类。

例如，我们可以写:

```
class Foo<A, B> {
  name: A;
  constructor(name: A) {
    this.name = name;
  }    foo(val: B): B {
    return val;
  }
}
```

然后要使用`Foo`类，我们可以写:

```
let foo: Foo<string, number> = new Foo('Joe');
```

正如我们所看到的，在 Flow 中定义类与 JavaScript 没有太大区别。唯一的区别是，我们可以向字段、参数和方法的返回类型添加类型注释。

此外，我们可以通过向字段、参数和返回类型传递泛型类型标记来使类型通用。

有了 Flow，我们还可以拥有只包含属性和方法标识符以及它们各自对应的类型和签名的类定义。

一旦设置了类型，如果我们在类之外设置这些属性的值，流将检查类型。类方法与其原型的方法相同。静态方法只是类中的一个方法，由类的所有实例共享。