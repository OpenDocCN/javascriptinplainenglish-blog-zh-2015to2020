# 静态工厂方法:JavaScript 中构造函数重载的替代方法

> 原文：<https://javascript.plainenglish.io/js-using-static-factory-method-as-an-alternative-of-constructor-overloading-473b67de6133?source=collection_archive---------6----------------------->

构造函数重载是为一个类创建多个构造函数并在不同的上下文中使用不同的构造函数的能力。您可能已经知道，由于 JavaScript 的设计，它不支持真正的重载。我们唯一能做的就是在唯一的一个构造函数内部添加一些条件，在不同的构造逻辑之间切换。

```
class Transaction {
  paymentType;
  amount;
  settleDate; constructor(*paymentType*, *paymentObject*) {
    this.paymentType = paymentType; if (paymentType === 'CASH') {
      this.amount = paymentObject.amount;
      this.settleDate = new Date();
    }
    else if (paymentType === 'CREDIT_CARD') {
      this.amount = paymentObject.creditAmount;
      this.settleDate = paymentObject.settleDate;
    }
}
```

然而，这种方法在某些情况下并不理想。

![](img/27b9989c6eca3443b863b406fa068f37.png)

Image from [builderintruro.com](http://www.builderintruro.com/)

# 问题

将多个构造逻辑放在构造函数内部主要有两个问题。首先，当有许多案件要处理时，管理会变得困难。这是因为参数实例化不清楚。就像上面的例子一样，`paymentObject`依赖于`paymentType`，如果不查看代码的其余部分，我们是不会知道的。

另一个问题是灵活性。当你希望基于不同的上下文有不同数量的参数时，就会变得很麻烦。当你想在不同的上下文中实例化对象时，有时你需要在参数中填充`null`。当你需要更新你的构造函数的参数时，这是一个噩梦，因为它会影响所有现有的用法。因此，我想介绍静态工厂方法，这是一个更好的选择。

# 什么是静态工厂方法？

它指的是使用静态方法实例化对象而不是直接使用构造函数的技术。这里我想用一个例子来说明:

```
class TestClass {
  testValA;
  testValB; constructor(valA, valB) {
    this.testValA = valA;
    this.testValB = valB;
  }

  static testFactory(val) {
    return new TestClass(val, val);
  }
}
```

在这个例子中，`testFactory`方法是一个静态工厂方法，它接受一个值并使用它来构造和返回一个`TestClass`对象。

# 运用这种方法

下面是我们的`Transaction`类在应用静态工厂方法后的样子:

```
class Transaction {
  paymentType;
  amount;
  settleDate; static withCash(cashObject) {
    const transaction = new Transaction();
    transaction.paymentType = 'CASH';
    transaction.amount = cashObject.amount;
    transaction.settleDate = new Date();
    return transaction;
  } static withCreditCard(creditCardObject) {
    const transaction = new Transaction();
    transaction.paymentType = 'CREDIT_CARD';
    transaction.amount = creditCardObject.creditAmount;
    transaction.settleDate = creditCardObject.settleDate;
    return transaction;
  }
}
```

对于实例化:

```
//Previously
console.log(new Transaction('CASH', cashObject));
console.log(new Transaction('CREDIT_CARD', creditCardObject));//Now
console.log(Transaction.withCash(cashObject));
console.log(Transaction.withCreditCard(creditCardObject));
```

仅此而已！干净多了，不是吗？

感谢阅读！请随意留下评论，我将不胜感激。

*更多内容请看*[*plain English . io*](http://plainenglish.io/)