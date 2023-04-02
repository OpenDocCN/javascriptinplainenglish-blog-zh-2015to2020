# JavaScript 重构——更多的类重构

> 原文：<https://javascript.plainenglish.io/javascript-refactoring-more-class-refactoring-bd269c22220f?source=collection_archive---------10----------------------->

![](img/8ecbbcc44d5933e96235d6bda4b9ad02.png)

Photo by [Kimberly Farmer](https://unsplash.com/@kimberlyfarmer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以清理我们的 JavaScript 代码，这样我们可以更容易地使用它们。

在本文中，我们将研究一些与清理 JavaScript 条件相关的重构思想。

# 用委派替换继承

如果我们有不需要从超类继承任何东西的子类，那么这个子类不一定是超类的子类。

例如，如果我们有一个子类，除了从它调用一些方法之外，不需要从超类中得到任何东西:

```
class Animal {
  //...
  speak() {
    //...
  }
}class Cat extends Animal {
  //...
  talk() {
    super.speak()
  }
}
```

我们可以删除`extends`并调用`Animal`类，如下所示:

```
class Animal {
  //...
  speak() {
    //...
  }
}class Cat {
  constructor() {
    this.animal = new Animal();
  } talk() {
    this.animal.speak()
  }
}
```

# 减少继承层次

我们可以减少继承层次，只保留必要的部分。

我们不想要一个复杂的难以导航的继承树。

例如，如果我们有以下代码:

```
class Animal {
  //...
}class Cat extends Animal {}class Dog extends Animal {}class BlackCat extends Cat {}class Labrador extends Dog {}
```

然后，我们可能需要重新考虑是否要为每种类型的猫和狗创建子类。

我们可以通过删除这些类来减小树的大小。例如，我们可以通过编写以下代码来删除上面代码中的最后两个类:

```
class Animal {
  //...
}class Cat extends Animal {}class Dog extends Animal {}
```

那么继承树就简单多了。

# 将程序设计转化为功能

如果我们有很多过程在做类似的事情，我们可以把这些过程转换成函数，这样我们就可以在很多地方使用它们。

例如，如果我们有以下代码:

```
const subtotal = 100
const provincialSalesTax = subtotal * 0.05;
const federalSalesTax = subtotal * 0.1;
const total = subtotal + provincialSalesTax + federalSalesTax;
```

我们可以将计税代码移入自己的函数中，如下所示:

```
const calculateTax = (amount, taxRate) => amount * taxRate;
const subtotal = 100
const provincialSalesTax = calculateTax(subtotal, 0.05);
const federalSalesTax = calculateTax(subtotal, 0.1);
const total = subtotal + provincialSalesTax + federalSalesTax;
```

现在，如果我们必须再次计算税额，我们可以使用`calculateTax`函数，而不是每次都编写代码来计算这些金额。

# 提取层次结构

如果我们的类因为方法中的大量条件语句而做了太多的事情，我们可以将这些代码提取到子类中。

例如，不要写以下内容:

```
class Invoice {
  getInvoice(type) {
    if (type === 'residental') {
      //...
    } else if (type === 'commercial') {
      //...
    } else if (type === 'concession') {
      //...
    }
  }
}
```

我们可以将`getInvoice`逻辑分解成它们自己的子类，如下所示:

```
class Invoice {}class ResidentialInvoice extends Invoice {
  getInvoice() {
    // residential logic
  }
}class CommercialInvoice extends Invoice {
  getInvoice() {
    // commercial logic
  }
}class ConcessionInvoice extends Invoice {
  getInvoice() {
    // concession logic
  }
}
```

假设`getInvoice`方法体在每个子类中是不同的，那么`getInvoice`方法可以留在那里，因为它们除了名字之外都是不同的。

![](img/73f804365b2d1de708753c0d89d71174.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

通过将复杂的逻辑分解成子类，我们可以使代码更加清晰。

此外，如果一个类不需要继承它，也可能不需要继承。

减少继承层次也是一个好主意，因为复杂性是不好的。

如果我们在不同的地方有相似的逻辑，我们应该为它们提取一个函数。

## **简明英语笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**