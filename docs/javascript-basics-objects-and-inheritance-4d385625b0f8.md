# JavaScript 基础——对象和继承

> 原文：<https://javascript.plainenglish.io/javascript-basics-objects-and-inheritance-4d385625b0f8?source=collection_archive---------11----------------------->

![](img/0b2e96848781cfe9157e38a106c91185.png)

Photo by [nicolas reymond](https://unsplash.com/@nicolasreymond?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究 JavaScript 对象和继承。

# 静态成员

JavaScript 类可以有静态成员。

它们在一个类的所有实例之间共享。

我们用关键字`static`表示某物是静态的。

例如，我们可以写:

```
class Cat {
  static type() {
    return 'cat';
  }
}
```

我们有直接用类调用的静态`type`方法:

```
Cat.type()
```

然后我们得到`'cat'`。

此外，我们可以写:

```
class Cat {}
Cat.type = 'cat';
```

向类中添加静态属性。

因为属性不能在类内部，我们必须把它附加在类外部。

# 遗产

在 JavaScript 中，我们可以创建子类来继承共享父类的方法，同时添加子类独有的成员。

例如，我们可以写:

```
class Cat {
  speak() {
    //...
  }
}class NiceCat extends Cat {
  fly() {
    //...
  }
}
```

我们使用`extends`关键字来表示我们从`NiceCat`子类中的`Cat`类继承成员。

`speak`方法对两个类都可用，而`fly`方法只在`NiceCat`类中可用。

要调用父类的构造函数和方法，我们可以使用`super`关键字。

例如，我们可以通过编写以下代码来调用父构造函数:

```
class Cat {
  constructor(name) {
    this.name = name;
  } speak() {
    //...
  }
}class NiceCat extends Cat {
  constructor(name, color) {
    super(name);
    this.color = color;
  } fly() {
    //...
  }
}
```

`NiceCat`有`super`关键字，用`name`参数调用`Cat`构造函数。

它必须出现在修改当前类的任何内容之前。

所以我们在设置`color`属性之前有了`super`调用。

为了调用父类的方法，我们还使用了`super`关键字。

例如，我们可以如下使用它:

```
class Cat {
  constructor(name) {
    this.name = name;
  } speak(words) {
    return words;
  }
}class NiceCat extends Cat {
  constructor(name, color) {
    super(name);
    this.color = color;
  } fly() {
    //...
  } niceSpeak(words) {
    return `nice cat says ${super.speak(words)}`
  }
}
```

我们添加了通过使用`super.speak`调用`Cat`的`speak`方法的`niceSpeak`方法。

# 实例 of

我们可以使用`instanceof`操作符来找出一个对象是否来自于一个特定的类。

左操作数是我们要检查的对象，右操作数是类或构造函数。

例如，我们可以写:

```
console.log(new NiceCat('joe', 'brown') instanceof Cat);
```

那么我们将得到`true`，因为它是`Cat`的一个实例。

同样，如果我们写:

```
console.log(new NiceCat('joe', 'brown') instanceof NiceCat);
```

我们还会记录下`true`。

![](img/04bfbdda5210e518f74dd4e5b9f89f4e.png)

Photo by [TOMOKO UJI](https://unsplash.com/@ujitomo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

类可以有静态成员。

我们可以使用`static`关键字来表示静态方法。

如果我们需要静态属性，我们可以将它直接附加到类中。

我们可以使用`extends`关键字来创建一个类的子类。

`instanceof`操作符对于检查对象是否是构造函数的实例很有用。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**