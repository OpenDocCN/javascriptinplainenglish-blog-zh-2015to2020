# JavaScript 中的类、工厂和对象原型

> 原文：<https://javascript.plainenglish.io/class-factory-and-object-prototypes-b4a7fff7dba8?source=collection_archive---------4----------------------->

![](img/9e9f3cdd1fb7ea374e4e5823054f674b.png)

Photo by [Miguel Á. Padriñán](https://www.pexels.com/@padrinan?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/photo-of-golden-cogwheel-on-black-background-3785927/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

对象原型继承是 JavaScript 的核心。基于我们在[上一篇关于函数](https://medium.com/javascript-in-plain-english/whats-function-got-to-do-with-this-7ab2633fb4a5)的文章中所学到的，这篇文章更深入地探讨了函数和原型继承。具体来说，我们就来看看**类函数和工厂函数。**

[](https://medium.com/javascript-in-plain-english/whats-function-got-to-do-with-this-7ab2633fb4a5) [## “this”关键字在不同的上下文中如何变化

### 并不是所有的 JavaScript 函数都是一样的。

medium.com](https://medium.com/javascript-in-plain-english/whats-function-got-to-do-with-this-7ab2633fb4a5) 

## **关于 JavaScript 中的**`**Class**`

**JavaScript 是基于原型的语言，而不是基于类的语言。由于 ES6 在语言中添加了一个`class`声明，JavaScript 开发人员可以更容易地创建类。然而，这种添加只是 JavaScript 原型继承的语法上的糖衣。**

**简而言之，在基于类的语言中，JavaScript 类不会像您所期望的那样运行。一个`new()`类不会创建一个要实例化的副本。它创建另一个对象，并链接到另一个对象的原型。**

**理解 ES6 类掩盖了引擎盖下的原型机制是至关重要的。需要记住的两个关键区别是:**

**1) JavaScript 类属性继承遵循原型链，而不是类链**

**2) JavaScript 类属性*可以在运行时动态添加或删除***

****有关这方面的更多阅读，请查看“** [**对象模型的详细信息。**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model)**

# ******类功能******

****在 ES6 之前，我们将结合显式委托原型使用[构造函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)。****

****对于 ES6，我们可以简单地使用`class`关键字。我们可以用两种不同的方式创建一个类，作为一个声明或者作为一个表达式。比如`class MyClass{}`和`const MyClass = class{}`是等价的。无论哪种方式，类函数都不会被提升，这意味着我们不能在它声明之前引用它。****

****同样，一个类函数，关于`this`关键字上下文，表现得像一个常规函数。所以我们在[上一篇文章](https://medium.com/javascript-in-plain-english/whats-function-got-to-do-with-this-7ab2633fb4a5)中探索的警告仍然适用于一个类实例。****

## ******基本用法******

****我们创建一个类函数，然后在其中定义一个`constructor`:****

```
**class Employee {
  constructor(name, hoursWorked) {
    this.wagePerHour = 15;
    this.hours = hoursWorked;
    this.employeeName = name;
  }// class method
  getWageReport() {
   const totalWage = this.hours * this.wagePerHour;
   return `${this.employeeName} total wages: ${totalWage}`
  }// getter
 get totalWage() {
  return this.getWageReport();
 }
}**
```

****我们还在类中使用了 getter 和 setter。使用`new`关键字创建该类的一个实例。****

```
**var employeeOne = new Employee(“Amy”, 20);
employeeOne.totalWage; // output: “Amy total wages: 300”**
```

## ******扩展创建子类******

****我们可以用关键字`extend`创建一个子类:****

```
**class Contractor extends Employee {
  constructor(name, hours) {
   super(name, hours);
   this.wagePerHour = 8;
  }
}var employeeTwo = new Contractor(“Tom”, 20);
employeeTwo.totalWage; // output: “Tom total wages: 160”**
```

****在子构造函数中，我们用`super()`调用父类。然后将`wagePerHour`赋值为新值。****

****在子类中，我们必须在访问关键字`this`之前调用`super()`。****

## ******静态方法******

****类也可以有静态方法，这些方法只能在类本身上调用，而不能在类实例上调用。静态方法作为实用方法很有用。****

****例如，如果我们想添加一个实用程序类，通过传入任意工资和工时来计算工资总额:****

```
**class Employee {
  ...omitted...
  static calculateWage(wage, hours) {
    return wage * hours
  }
}**
```

****我们只能通过直接引用类而不是实例来调用它:****

```
**Employee.calculateWage(100, 5); // output: 500employeeOne.calculateWage(100, 5); // TypeError: employeeOne.calculateWage is not a function**
```

****而如果我们想从一个类内部引用静态方法，就必须通过类名(*显式引用*)或者调用它的构造函数(*动态引用*)。****

****例如，如果我们重写`getWageReport`类方法以使用静态`calculateWage`方法:****

*****明确引用-*****

```
**getWageReport() {
  const totalWage = 
   **Employee.calculateWage**(this.wagePerHour, this.hours)

  return `${this.employeeName} total wages: ${totalWage}`
}**
```

*****动态参考-*****

```
**getWageReport() {const totalWage = 
   **this.constructor.calculateWage**(this.wagePerHour, this.hours)return `${this.employeeName} total wages: ${totalWage}`
}**
```

****如果你不想改变静态方法，那么使用显式引用。****

****静态方法的`this`关键字不会自动绑定到执行上下文或词法范围(自动装箱)。我们需要明确地定义或传递`this`上下文给这个方法。****

****这就是为什么我认为与其在父类中动态引用一个静态方法，不如使用一个简单的类方法要干净得多。那么该方法的可用性是显而易见的。我们也没有丢失`this`上下文的问题。****

## ******私有字段和方法(ES2019)******

****默认情况下，类字段和方法是公共的。ES2019 增加了通过在私有成员前面加上#来定义私有成员的功能。****

******对私有字段的支持还不是 100%。私有方法处于** [**提议阶段**](https://github.com/tc39/proposal-private-methods) **并且只能用于 Babel 和其他一些框架。******

****假设您已经添加了支持，使用**雇员**示例，让我们添加一个私有字段和方法:****

```
**class Employee {#minimumWage;constructor(name, hoursWorked) {
    this.#minimumWage = 7;    
    this.wagePerHour = this.#calculateBaseWage();
    this.hours = hoursWorked;
    this.employeeName = name;
  }// class method
  getWageReport() {
    const totalWage = this.hours * this.wagePerHour;
    return `${this.employeeName} total wages: ${totalWage}`
  }// getter
 get totalWage() {
   return this.getWageReport();
 }//private static method
 static #assignBaseWage(min) {
   return min * 2;
 }

 // private method
 #calculateBaseWage() {
   return Employee.#assignBaseWage(this.#minimumWage);
 }

}const employeeThree = new Employee("Bobby", 25);
employeeThree.#minimumWage; // error: Syntax error
employeeThree.totalWage; // output: "Bobby total wages: 350"**
```

****上面的例子有点做作，在根据最低工资计算基本工资时多了一层。****

****这只是为了说明静态私有方法与上一节中解释的常规静态方法具有相同的非实例访问限制。****

## ******类捕获:可变性******

****由类创建的对象不是不可变的。因为类对象只是另一个 JavaScript 对象。考虑:****

```
**employeeTwo.employeeName = ‘Bob’;employeeTwo.totalWage; // output: “Bob total wages: 160”**
```

****我们会使用一个不可变的库或者在类函数中使用`Object.freeze`,但是一旦我们开始创建子类/子类，就会有很多警告。****

******要深入了解不变性和类，请查看 Everything Frontend 上 JavaScript** **中的** [**不可变类。值得一读！**](https://www.everythingfrontend.com/posts/immutable-classes-in-javascript.html)****

## ******类问题:实践中的对象原型链******

****为了说明原型继承或委托的含义，请使用上面的代码示例来考虑这些场景。****

****这些情况不太可能通过设计发生。因为它们不是好的编码实践，可以被认为是反模式，所以不要这样做！****

****但是，随着代码库变得越来越复杂，不变性没有得到很好的实施，意外的操作可能会发生。****

## ****类的方法或属性可以在声明后修改吗？是****

****继续上面的静态方法示例，如果我在类声明之后重新定义了`calculateWage`方法，并且我们使用了显式引用`Employee.calculateWage`:****

```
**// redefine the method in parent class
Employee.calculateWage = function() { return ‘what?’ }// redefine the method in child class
Contractor.calculateWage = function() { return ‘what now?’ }// get totalWage again, parent class used explicit reference
employeeOne.totalWage; //output: “Amy total wages: what?”
employeeTwo.totalWage; //output: “Tom total wages: what?”**
```

****父类和子类的实例现在都从父类 Employee 中新定义的`calculateWage`方法返回值——“什么？”。我们不应该期望子类的实例返回“现在怎么办？”因为声明的引用对*雇员*是显式的。****

****如果我们已经声明使用动态引用，`this.constructor.calculateWage`，我们应该看到每个类的实例尊重它的边界:****

```
**// get totalWage again, parent class used dynamic referenceemployeeOne.totalWage; //output: “Amy total wages: what?”
employeeTwo.totalWage; //output: “Tom total wages: what now?”**
```

## ******我可以在声明后添加新的父方法吗？是******

****如果我通过添加一个新的`getShift`方法来修改父 ***雇员*** 类 aka 对象原型:****

```
**// randomly generate and return a shift number with employee nameEmployee.prototype.getShift = function () {
  const shift = lastShiftNumber + 1;
  return `${this.employeeName} is on shift number ${shift}.`;
};**
```

****然后我们对之前创建的两个对象调用这个方法:****

```
**employeeOne.getShift(1); //output: “Amy is on shift number 2.”
employeeTwo.getShift(2); //output: “Tom is on shift number 3.”**
```

****我们将得到相应的结果，打印出员工姓名和正确的班次编号。****

****这是因为原型委托的缘故。****

****当`employeeTwo`在原型中找不到方法时，它会沿着委托链向上遍历到父原型，并从那里执行`getShift`方法。****

## ******关于类的结论******

****原型委托是 JavaScript 中的一个强大特性。类语法使得创建具有继承行为的对象更加容易。****

****然而，将面向类的范例应用到基于原型的语言中会导致混乱。我们需要在原型继承的上下文中理解类，以便在设计 OOP 系统时充分利用对象原型。****

# ******工厂功能******

****ES6 类或构造函数不是在 JavaScript 中创建可重用或可组合对象的唯一方法。另一种广泛使用的方法是工厂函数。****

****类和工厂函数的心理模型并不相似。一种是从面向对象的模式开始，而另一种更接近于函数式编程。****

******工厂函数是返回对象的(非构造函数)函数。对象可以是对象文字或对象原型。******

****以上一节的`**Employee**`为例，说明使用**工厂函数创建一个对象文字(无继承)**:****

```
**const RegularEmployeeModel = { wagePerHour: 15 };
const ContractorModel = { wagePerHour: 8 };// utility function
const calculateWage = (wage, hours) => wage * hours;// factory functionconst CreateEmployee = (name, hoursWorked, model) => {
  const hours = hoursWorked;
  const employeeName = name;
  const wagePerHour = model.wagePerHour;

  const getWageReport = () => {
    const totalWage = calculateWage(wagePerHour, hours);
    return `${employeeName} total wages: ${totalWage}`;
  };return Object.freeze({
     totalWage: getWageReport(),
     name: employeeName,
     hours
  });
}**
```

****我们可以使用常规或箭头函数。这里我们使用一个箭头函数`CreateEmployee`，它接受`name`和`hoursWorked`参数(就像类的例子一样)。最后，它接受`model`作为参数，它定义了我们想要创建的对象的结构。****

****在那里，我们调用函数为雇员创建一个对象:****

```
**const employeeOne = CreateEmployee(“Amy”, 20, RegularEmployeeModel);
const employeeTwo = CreateEmployee(“Tom”, 20, ContractorModel);employeeOne.totalWage; // output: “Amy total wages: 300”employeeTwo.totalWage; // output: “Tom total wages: 160”**
```

****这很简单——给定输入，我们有一个预期的输出。这种模式提供了很大的灵活性。****

****美妙的是，我们可以轻松地将 return 语句包装在一个`Object.freeze`或其他不可变的实用程序中，以确保创建的任何对象都是不可变的。****

****由于该函数每次都返回一个新对象，因此没有委托或继承。****

****我们还可以有一个**工厂函数来创建一个具有原型继承的对象。为此，我们将在函数中使用`Object.create`:******

```
**const RegularEmployee = {
  wagePerHour: 15,
  getWageReport: function () {
    const totalWage = calculateWage(this.wagePerHour, this.hours);
    return `${this.name} total wages: ${totalWage}`;
  }
};const Employee = (name, hoursWorked, model) => {
   let newEmployee = Object.create(model);
   newEmployee.name = name;
   newEmployee.hours = hoursWorked;
   newEmployee.totalWage = newEmployee.getWageReport(); return Object.freeze(newEmployee);
}const employeeThree = Employee("Amy", 20, RegularEmployeeModel);employeeThree.totalWage; // output: "Amy total wages: 300"**
```

****我们可以检查原型委托:****

```
**Object.getPrototypeOf(employeeThree) === RegularEmployeeModel;
// output: true// don't really do this mutation, anti-pattern!
RegularEmployeeModel.say = 'hello';
employeeThree.say; // output: 'hello';**
```

****在上面的例子中，我们看到当我们用一个新方法修改委托原型`RegularEmployeeModel`时，一个冻结的/不可变的`employeeThree`的行为发生了变化。不变性仅限于通过调用工厂函数创建的对象属性。****

## ******私有数据和封装******

****从前面两个例子可以看出，除非一个变量或方法被故意暴露(返回)，那么它就不能在函数之外被访问。因此，默认情况下，工厂函数中的所有变量和方法都是私有的。当然，这不适用于对象原型上的方法。****

## ******关于工厂的结论******

******工厂功能灵活。它们很容易理解，执行上下文总是很清楚。你没有模糊不清的** `***this***` **关键词问题。******

******我们可以修改它以包含不同的实现模式，特别是使用** `***Object.create***` **的 OOLO(对象链接到其他对象)模式。******

******它可以支持创建简单的对象文字。如果我们将工厂功能分解成小的功能块，它们可以成为组合模式的基础。******

# ******类/构造函数与对象.用工厂创建******

****我们看到工厂函数和类/构造函数都可以通过继承创建对象原型。工厂函数并不局限于此，它在生成对象文字或简单数据对象时非常有用。****

****Class 的 ES6 语法使得使用构造函数创建新对象更容易编写。与 Object.create 相比，类/构造函数的一个优势在于性能。****

****JavaScript 引擎通过使用`new`关键字和类在`Object.create`上创建新的对象原型来优化性能。****

****同样，性能差异主要在于系统是如何构建的。是这样吗，还是需要用面向对象的方法来设计？代码的清晰性和可维护性也不容忽视。****

****通常，在使用**类/构造函数**和使用**对象的工厂之间的选择。创造归结于个人喜好以及它如何适应你的大格局。******