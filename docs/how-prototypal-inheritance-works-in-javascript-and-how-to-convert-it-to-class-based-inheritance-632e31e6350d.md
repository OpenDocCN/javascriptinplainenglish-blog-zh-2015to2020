# JavaScript 中的原型继承是如何工作的

> 原文：<https://javascript.plainenglish.io/how-prototypal-inheritance-works-in-javascript-and-how-to-convert-it-to-class-based-inheritance-632e31e6350d?source=collection_archive---------3----------------------->

## 以及如何将其转换为基于类的继承

![](img/c80bf674b2745b7cfda2db93c49ac034.png)

Photo by [Bharat Patil](https://unsplash.com/@bharat_patil_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在从`prototypal inheritance`开始之前，让我们先了解一下`prototype`是什么。

> JavaScript 中的所有对象，如`Array`、`Boolean`、`Date`等，都继承了它们原型的属性和方法。

`Object`在原型链的顶端意味着所有其他对象从`Object.prototype`继承它们的属性和方法

假设我们有`Person`对象构造函数

```
function Person(name, age) {
 this.name = name;
 this.age = age;
}
```

然后我们如下创建`Person`的对象

```
const person1 = new Person("David", 30);
const person2 = new Person("John", 35);console.log(person1); // {name: "David", age: 30}
console.log(person2); // {name: "John", age: 35}
```

如果我们想给`Person`对象添加另一个属性，我们可以像这样给一个单独的对象添加:

```
person1.gender = 'Male';
```

但这只会将该属性添加到`person1`对象中。

```
console.log(person1.gender); // Male
console.log(person2.gender); // undefined
```

要将属性添加到`Person`对象本身，我们必须在创建对象之前将其添加到其原型，这样它将对其所有对象可用。

```
Person.prototype.gender = 'Male';const person1 = new Person("David", 30);
const person2 = new Person("John", 35);console.log(person1.gender); // Male
console.log(person2.gender); // Male
```

我们还可以向`Person`原型添加方法

```
Person.prototype.display = function() {
 console.log(this.name, this.age);
};const person1 = new Person("David", 30);
const person2 = new Person("John", 35);person1.display(); // David 30
person2.display(); // John 35
```

因此，要向对象添加任何属性或方法，我们需要将其添加到其原型中。

如果我们比较`person1`和`person2`显示方法，我们会看到它返回 true

```
console.log(person1.display === person2.display) // true
```

*它返回 true，因为在内存中只有显示函数的一个副本，因为我们将该方法添加到了它的原型中，所以它由所有对象共享。*

让我们看看将它直接添加到`Person`构造函数中会发生什么

```
function Person(name, age) {
 this.name = name;
 this.age = age;
 this.display = function() {
  console.log(this.name, this.age);
 };
}const person1 = new Person("David", 30);
const person2 = new Person("John", 35);person1.display(); // David 30
person2.display() // John 35
```

现在，让我们比较显示方法

```
console.log(person1.display === person2.display) // false
```

现在它显示 false，因为我们已经将该方法直接添加到构造函数中，从`Person`创建的每个对象都有自己的函数副本，所以内存中有两个显示函数副本，随着我们创建更多的对象，内存中显示函数副本的数量也会增加。

所以这是一个不好的做法，不推荐直接在构造函数中创建方法，我们应该把它们添加到原型中。

现在，我们有了一些原型的基本知识，让我们现在来理解原型继承。

**原型遗传**

让我们从上面同样的`Person`例子开始

```
function Person(name, age) {
 this.name = name;
 this.age = age;
}Person.prototype.display = function() {
 console.log(this.name, this.age);
};
```

现在，让我们创建`Employee`构造函数，它将继承 Person 的属性和方法

```
function Employee(name, age, salary) {
 Person.call(this, name, age);
 this.salary = salary;
}const emp = new Employee('Mike', 20, 4000);
```

这里，我们使用`Person.call`函数来调用`Person`的构造函数，并将`name`和`age`传递给它

现在，让我们看看`emp`包含了什么

```
console.log(emp); // {name: "Mike", age: 20, salary: 4000}
```

如你所见，它包含了`Person`的所有属性加上它自己的属性

现在，让我们使用`emp`对象调用显示方法。

```
emp.display(); //  Error: emp.display is not a function
```

为什么我们会得到这个错误？

这是因为我们只提到了使用`Person.call(this, name, age)`继承`Person`的属性

为了继承方法，我们需要链接原型，然后创建一个对象

```
Employee.prototype = Object.create(Person.prototype);const emp = new Employee('Mike', 20, 4000);
emp.display(); // Mike 20
```

现在，让我们检查一下`Employee`的类型，我们可以使用构造函数属性来检查它

```
console.log(emp.constructor) // Person
console.log(Employee.prototype.constructor) // Person
```

它打印出`Person`，这是错误的，因为我们知道，它应该是`Employee`。`constructor`属性应该总是返回正确的类型。

假设，我们有一个数组

```
const numbers = [1, 2, 3, 4];console.log(typeof numbers); // "object"
```

我们得到 object，因为 JavaScript 中的每个数组都是一个对象，所以要得到它的正确类型，我们可以使用返回正确类型的`constructor`属性。

```
console.log(numbers.constructor) // Array
console.log(numbers.constructor === Array) // true
```

所以要解决`Employee`构造函数的问题，我们需要改变它的构造函数类型

```
Employee.prototype.constructor = Employee;console.log(emp.constructor) // Employee
console.log(Employee.prototype.constructor) // Employee
```

现在，我们得到了正确的结果。

完整的原型继承将如下所示

如你所见，即使对于简单的原型继承，我们也必须添加许多额外的代码。

所以 ES6 增加了类语法，允许我们以一种简单的方式实现同样的功能。

如您所见，与原型继承相比，基于类的语法简短且易于理解。

**注意:**最后的类使用原型继承本身。只是，我们不需要担心它

今天到此为止。我希望你学到了新东西。

**别忘了订阅我的每周简讯，里面有惊人的技巧、诀窍和文章，直接在这里的收件箱里** [**。**](https://yogeshchavan.dev/)