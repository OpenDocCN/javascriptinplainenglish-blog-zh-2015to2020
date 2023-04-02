# JavaScript 最佳实践—导入、符号和构造函数

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-imports-symbols-and-constructors-e78ce6ab8e3d?source=collection_archive---------6----------------------->

![](img/ca643a669cb23df56c7d3a340e7c9b49.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在这篇文章中，我们将看看导入模块的正确方法，使用`Symbol` 构造函数，在子类的构造函数中引用`this`之前调用`super`。

# 没有重复的导入

绝对不应该添加重复的导入。如果我们从同一个模块中导入多个项目，那么我们应该把它们都放在一个语句中。

例如，如果我们想要导入多个项目，我们不应该编写如下内容:

```
import { foo } from "./module";
import { bar } from "./module";
```

相反，我们可以通过编写以下代码来节省一些空间:

```
import { foo, bar } from "./module";
```

如果我们从同一个模块导入多个成员，我们导入的越多，通过将所有的导入合并到一个语句中节省的空间就越多。

# 不要将符号用作构造函数

在 JavaScript 中，符号被用作对象和类中方法的唯一标识符。

每个符号都是独一无二的，即使它们有相同的名字。例如，如果我们有以下代码:

```
const foo1 = Symbol('foo');
const foo2 = Symbol('foo');
console.log(foo1 === foo2);
```

然后控制台批量输出记录`false`，因为由`Symbol`函数返回的每个符号都是它们自己的实例。

`Symbol`函数不是构造函数。因此，我们不应该使用`new`操作符。

例如，我们不应该编写类似下面这样的代码:

```
const a = new Symbol("a");
```

如果我们运行它，那么我们将得到错误消息‘未捕获的类型错误:符号不是构造函数’。

相反，我们应该直接调用它，如下所示:

```
const a = Symbol("a");
```

`Symbol`函数应该作为函数直接调用。

# 在构造函数中调用`super()`之前不要使用`this or super`

在 JavaScript 中，如果我们创建一个使用`extends`关键字扩展另一个类的类，那么我们必须调用`super`来调用子类中父类的构造函数。

如果我们在调用`super`之前引用`this`，那么我们会得到一个 ReferenceError。

例如，如果我们有以下类:

```
class Animal {
  constructor(name) {
    this.name = name;
  }
}class Cat extends Animal {
  constructor(name, breed) {
    this.breed = breed;
    super(name);
  }
}const cat = new Cat('jane', 'black')
```

在上面的代码中，我们将得到错误消息“未捕获的 ReferenceError:在访问“this”或从派生的构造函数返回之前，必须调用派生类中的`super`构造函数”。

这是因为我们必须调用`super`来定义`this`。在子类开始配置`this`之前，必须首先调用父构造函数来完成`this`的配置。

超类不知道子类，所以超类必须先实例化。

因此，我们需要在运行任何引用`this`的代码之前调用`super`，这样我们就不会出现这个错误。

必须修改`Cat`类，这样任何引用`this`的代码都必须移到`super`函数调用的下面。

我们应该改为写:

```
class Animal {
  constructor(name) {
    this.name = name;
  }
}class Cat extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
}const cat = new Cat('jane', 'black')
```

在上面的代码中，我们有在`constructor`方法中的 `this.breed = breed;`之前的`super`调用。

现在，当我们运行代码时，不会出现任何错误。

然而，在任何其他方法中引用`this`的代码可以在任何地方。例如，如果我们有以下代码:

```
class Animal {
  constructor(name) {
    this.name = name;
  } speak() {}
}class Cat extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  } meow() {
    console.log(this.name);
    super.speak();
  }
}
const cat = new Cat('jane', 'black')
cat.meow();
```

在上面的代码中，我们在`Animal`类中有`speak`方法。在`Cat`类中，我们有在从父`Animal`类调用方法之前引用`this.name`的`meow`方法。

![](img/c96d152904d716264170a857eb3a783d.png)

Photo by [Alexander Schimmeck](https://unsplash.com/@alschim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

如果我们从同一个模块中导入多个成员，那么我们应该把它们都放在一个语句中以节省空间。

`Symbol`函数不是构造函数。要创建一个新的符号，我们应该通过调用`Symbol`函数来创建它。

为了确保我们的父类实例配置正确，我们应该首先调用子类的构造函数中的`super`，然后运行任何引用`this`的代码。

# **简明英语笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**