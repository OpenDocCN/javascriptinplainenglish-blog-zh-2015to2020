# TypeScript 类简介—更多访问修饰符

> 原文：<https://javascript.plainenglish.io/introduction-to-typescript-classes-more-access-modifiers-91c816311105?source=collection_archive---------9----------------------->

![](img/e6368c73d3ea70daa207191f6de390fd.png)

Photo by [Stefan Steinbauer](https://unsplash.com/@usinglight?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

与 JavaScript 一样，TypeScript 中的类是其原型继承模型的特殊语法，这是基于类的面向对象语言中的类似继承。类只是添加到 ES6 中的特殊函数，用来模仿其他语言中的关键字`class`。在 JavaScript 中，我们可以有`class`声明和`class`表达式，因为它们只是函数。所以像所有其他函数一样，有函数声明和函数表达式。TypeScript 也是如此。类充当创建新对象的模板。TypeScript 扩展了 JavaScript 类的语法，然后添加了自己的变体。在本文中，我们将看到更多的 TypeScript 中类成员的访问修饰符。

# 只读修饰符

使用 TypeScript，我们可以用`readonly`关键字将类成员标记为只读。这可以防止成员在初始化后被修改。此外，它们必须在声明时或在构造函数中初始化。例如，我们可以在下面的代码中使用它:

```
class Person {
  readonly name: string;
  constructor(name: string) {
    this.name = name;      
  }
}const person = new Person('Jane');
```

如果我们试图在初始化后将一个值设置为`name`后再给它赋值，那么我们会得到一个错误。例如，如果我们写:

```
class Person {
  readonly name: string;
  constructor(name: string) {
    this.name = name;      
  }
}
const person = new Person('Jane');
person.name = 'Joe';
```

那么 TypeScript 编译器不会编译代码，我们会得到错误消息“无法赋值给‘name ’,因为它是只读属性。(2540)"

我们可以通过使用参数属性来缩短上面的代码。有了参数属性，我们既可以声明一个`readonly`成员，也可以通过将成员声明放在构造函数的签名中为其赋值。一旦我们把它放在括号里，我们就可以声明它，同时给它赋值。例如，不要写:

```
class Person {
  readonly name: string;
  constructor(name: string) {
    this.name = name;      
  }
}const person = new Person('Jane');
```

我们可以改为写:

```
class Person {  
  constructor(readonly name: string) {    
  }
}const person = new Person('Jane');
```

然后，当我们在`person`上运行`console.log`时，我们可以看到成员`name`已经被赋值为‘Jane ’,而不必显式地编写代码来为其赋值。我们也可以用`public`、`private`或`protected`代替`readonly`；或者将`readonly`与`public`、`private`或`protected`组合。例如，我们可以编写以下代码:

```
class Person {  
  constructor(private readonly name: string) {    
  } getName() {
    return this.name;
  }
}
const person = new Person('Jane');
console.log(person.getName());
```

获取名为`name`的`private`和`readonly`成员，在`getName`方法中检索它的值。当我们在最后一行运行`console.log`行时，我们应该得到‘简’。同样，我们可以对`public`或`protected`做同样的事情，如下面的代码所示:

```
class Person {  
  constructor(protected readonly name: string) {    
  }
}class Employee extends Person {  
  constructor(
    public name: string, 
    private employeeCode: number
  ){    
    super(name);
  } getEmployeeCode() {
    return this.employeeCode;
  }
}
const person = new Employee('Jane', 123);
console.log(person.name);
console.log(person.getEmployeeCode());
```

正如我们所看到的，在`Person`和`Employee`类中，我们有各种访问修饰符和`readonly`关键字的参数属性。然后，我们可以使用`Employee`构造函数将所有值分配给具有`Employee`构造函数的所有成员。然后，当我们直接获取成员的值，或者通过私有成员`employeeCode`的方法获取成员的值时，我们通过`getEmployeeCode`方法获取成员的值，然后我们可以看到我们期望的值被记录。我们看到`person.name`是‘简’，而`person.getEmployeeCode()`得到我们 123。

![](img/7485982d689a4d6bef9c0cf9195c93b1.png)

Photo by [Richard Brutyo](https://unsplash.com/@richardbrutyo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 访问者

像在 JavaScript 中一样，我们可以将 getter 和 setter 方法添加到 TypeScript 类中。这可以防止我们意外地直接修改公共成员值，并让我们对如何检索和设置成员值有更多的控制。为了给类添加 getters 和 setters，我们可以分别使用`get`和`set`关键字。我们将它们放在类的方法签名前面，以将一个方法指定为 getter 或 setter。例如，我们可以在下面的代码中使用它:

```
class Person {
  private _name: string = ''; get name(): string {
    return this._name;        
  } set name(newName: string) {
    if (newName && newName.length < 5) {
      throw new Error('Name is too short');      
    }
    this._name = newName;
  }
}let person = new Person();
person.name = 'Jane Smith';
console.log(person.name);
person.name = 'Joe';
```

在上面的例子中，我们添加了一个带有`get`关键字的 getter 方法。`name`方法用于获取`_name`字段，该字段是私有的，因此如果没有 getter 方法，我们无法获取它的值。为了通过我们的 getter `name`方法检索`this._name`的值，我们只需使用`person.name`属性来获取它。然后我们设置`this._name`的值，我们添加一个带有`set`关键字和方法名`name`的 setter 方法。在`name` setter 方法中，我们传入参数，让我们用赋值操作符给它赋值，就像我们在上面代码的倒数第三行中做的那样。

正如我们所见，我们可以将验证代码放在`set name`方法中。这是使用 getter 和 setter 方法的一个很好的理由，因为我们可以控制如何为单个类成员设置值。在上面的例子中，如果我们分配的值少于 5 个字符，那么我们抛出一个错误，消息是“名称太短”。这使我们无法在字符串少于 5 个字符的情况下赋值。如果我们运行代码，第一个赋值表达式:

```
person.name = 'Jane Smith';
```

然后我们让“简·史密斯”登录。当我们试图给它赋一个少于 5 个字符的值时，就像我们对:

```
person.name = 'Joe';
```

然后我们得到一个错误，就像我们在代码中指出的那样。

注意，要使用访问器，我们必须将输出编译到 ES5 或更高版本。不支持编译到 ES3，但这应该是现代浏览器的问题。同样，有`get`但没有`set`的访问器被自动推断为`readonly`。

在 TypeScript 中，我们为类成员提供了`readonly`修饰符，这样它们在初始化后就不能被设置为新值。此外，TypeScript 具有参数属性特性，因此我们不必显式地编写代码来通过构造函数给变量赋值。如果我们在构造函数中添加参数，那么当我们用`new`关键字实例化这个类时，它们会被自动设置。访问器方法对于控制我们如何获取和设置类成员的值很有用。我们可以用关键字`get`和`set`指定 getter 和 setter 方法。