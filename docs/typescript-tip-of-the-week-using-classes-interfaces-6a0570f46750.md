# 本周打字技巧—使用类和接口

> 原文：<https://javascript.plainenglish.io/typescript-tip-of-the-week-using-classes-interfaces-6a0570f46750?source=collection_archive---------5----------------------->

![](img/5fcc37e303fb9b1d585e1b7a12241579.png)

嘿，伙计们，很抱歉我已经有一段时间没有发帖了，过去几个月我一直特别忙！我很高兴能够带着另一个打字技巧回来。这周我想谈谈类和接口。如果你不熟悉接口，它们为一个对象或类创建了一种结构。它们强制执行这种结构，这样，如果您的类没有正确实现它，您将会得到一个错误。

当一个接口强制一个对象结构应该是什么样子时，它也创建了一个与你的代码和外部代码的契约。为了进一步说明这一点，我们来看一个例子。

```
// person-library.tsexport interface IPerson {
  firstName: string;
  lastName: string;
  age: number;
}export class Person implements IPerson {
  firstName: string;
  lastName: string;
  age: number; constructor(data: IPerson) {
    this.firstName = data.firstName;
    this.lastName = data.lastName;
    this.age = data.age;
  } // Class Functions}
```

*如果你想知道为什么这个接口被命名为* `*IPerson*` *而不仅仅是* `*Person*` *，那么* `*I*` *通常作为通用约定用于接口，但是也可以用于被实现的接口。*

在这个例子中，`Person`类实现了`IPerson`接口。这意味着 Person 类必须具有`IPerson`的所有属性。让我们假设这是其他人正在使用的`person`库的一部分。我提到过，这可以在外部代码中使用，以在库中的接口和外部代码之间创建一个契约。

```
// someone other codeimport { IPerson, Person } from 'person-library';interface IEmployee extends IPerson {
  jobTitle: string;
  salary: number;
}class Employee extends Person implements IEmployee {
  firstName: string;
  lastName: string;
  age: number;
  jobTitle: string;
  salary: number constructor(data: IEmployee) {
    super(data); this.jobTitle = data.jobTitle;
    this.salary = data.salary;
  } // Class Functions
}const me = new Employee({firstName: 'Sam', lastName: 'Redmond', age: 100, jobTitle: 'Software Engineer', salary: 1000000});
```

这是一个简单的例子，展示了如何用额外的属性扩展`IPerson`接口。这种模式提供了一个通用的结构，并允许代码重用。我提到过接口在你的代码中创建契约。这意味着当我们使用来自一些外部代码的接口时，保证它将具有某些属性。您可以使用该保证来假设某些属性将会存在。

让我们稍微分解一下这段代码。当我们用`IPerson`接口扩展`IEmployee`接口时，我们不需要在`IPerson`中定义属性。这是 TypeScript 的一个有用功能，这样您可以声明一次接口属性，然后在以后扩展它们。我们可以用`Employee`类做同样的事情。当我们扩展一个类时，我们还必须使用`super`函数，以便将所需的信息传递给扩展类的构造函数。

你会注意到我们将一个`IEmployee`传递给了`super`函数，但它应该接受一个`IPerson`。因为`IEmployee`扩展了`IPerson`，所以它必须具有`IPerson`的所有属性，并且在这种情况下可以用在它的位置上。这是使用接口在代码中创建契约的一个很好的例子。由于`IPerson`的所有属性都已经通过使用`super`函数进行了初始化，我们只需要初始化`IEmployee`特有的属性。

遵循这种模式使您能够轻松地重用代码块。这使你的代码整洁简洁，这将有助于你长期维护它。我希望你喜欢这个打字技巧。由于假期已经结束，我应该可以继续定期发帖。我将在每周三晚上发布一个新的打字提示。

感谢阅读！

如果你喜欢这篇文章，我希望你能[加入我的邮件列表](https://email.sam-redmond.com/)，在那里我会发送额外的提示&技巧！

如果你觉得这篇文章有帮助、有趣或娱乐性[请给我买杯咖啡](https://email.sam-redmond.com/products/throw-me-a-bit)帮助我继续发布内容！