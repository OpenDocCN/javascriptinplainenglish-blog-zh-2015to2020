# 每个开发人员都应该知道的 7 个 JavaScript 设计模式

> 原文：<https://javascript.plainenglish.io/7-javascript-design-patterns-every-developer-should-know-df9c40e7debf?source=collection_archive---------4----------------------->

## 如果您可以像构建一个漂亮的模板一样构建您的源代码，并将其应用于所有同类项目，那会怎么样？

![](img/a9549e96e1234230dfa1a3500d1540f7.png)

Photo by [Edgar Chaparro](https://unsplash.com/@echaparro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

每个项目一个模板？听起来很神奇，不是吗？

这也是我努力追求的编码水平。

实现它的方法之一是使用设计模式，这有助于编写结构良好、美观且有组织的代码。

在这个故事中，我们将发现 JavaScript 中使用的一些常见设计模式。让我们开始吧。

# 1.构造器模式

您熟悉构造函数，它是用特定的属性和方法初始化对象的函数。

构造器模式类似于这个定义。我们使用这种模式来创建同一个对象的多个实例。

在 JavaScript 中创建新对象有很多方法。看看下面的例子:

```
// Using **{}** to create empty objects:
let person = {};// Using **Object()** to create empty objects:
let person = new Object();// Using **function constructor**:
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.showName = () => console.log(this.name);
}let person = new Person(‘Amy’, 28);
person.showName();
```

# 2.原型模式

原型模式是一种基于对象的创造性设计模式。它通过从原型克隆对象来创建对象的新实例。原型模式的主要焦点是创建一个对象，作为每个对象构造器的蓝图。

如果您发现直接构造一个新对象是复杂且低效的，那么原型模式就非常适合这种情况。

您可以用一种经典方式实现原型模式，如下所示:

```
function Book(title, price) {
  this.title = title;
  this.price = price;
  this.printTitle = () => console.log(this.title);
}function BookPrototype(prototype) {
  this.prototype = prototype; this.clone = () => {
    let book = new Book();
    book.title = prototype.title;
    book.price = prototype.price;

    return book;
  };
}let sampleBook = new Book(‘JavaScript’, 15);
let prototype = new BookPrototype(sampleBook);
let book = prototype.clone();
book.printTitle();
```

由于 JavaScript 有自己内置的原型工具，您可以更有效地使用 it 模式，如下所示:

```
let book = {
  title: ‘JavaScript’,
  price: 15
}let anotherBook = Object.assign({}, book);
```

我们有第一本书作为原型。然后，我们使用这个原型创建一个新的 book 实例，这样新的 book 就有了用原型值初始化的所有属性值。

# 3.命令模式

这种模式的主要目的是将动作或操作封装成对象。

假设您需要为一家电子商务商店构建一个支付系统。根据所选择的付款方式，您将需要处理特定的流程。

示例:

```
if (selectedPayment == ‘creditcard’) {
  // handle payment by creditcard
}
```

有些付款方式只有一个步骤，但其他方式可能不止一个步骤。通过使用上面的示例代码，您提供了一个实现，而不是一个接口，这会导致紧耦合。

命令模式是提供松散耦合的一个很好的解决方案。系统不应该知道太多关于每个特定支付方法处理的信息。为了实现这一点，该模式将把请求操作的代码与执行实际实现的代码分开。

示例:

```
function Command(operation) {
  this.operation = operation;
}Command.prototype.execute = function () {
  this.operation.execute();
}function ProcessCreditCardPayment() {
  return {
    execute: function() {
      console.log(‘Credit Card’)
    }
  };
}function ProcessPayPalPayment() {
  return {
    execute: function() {
      console.log(‘PayPal’)
    }
  };
}function ProcessStripePayment() {
  return {
    execute: function() {
      console.log(‘Stripe’)
    }
  };}function CreditCardCommand() {
  return new Command(new ProcessCreditCardPayment());
}function PayPalCommand() {
  return new Command(new ProcessPayPalPayment());
}function StripeCommand() {
  return new Command(new ProcessStripePayment());
}function PaymentSystem() {
  let paymentCommand;

  return {
    setPaymentCommand: function(command) {
      paymentCommand = command;
    },
    executeCommand: function() {
      paymentCommand.execute();
    }
  };
}function run() {
  let paymentSystem = new PaymentSystem();
  paymentSystem.setPaymentCommand(new CreditCardCommand());
  paymentSystem.executeCommand();
  paymentSystem.setPaymentCommand(new PayPalCommand());
  paymentSystem.executeCommand();
  paymentSystem.setPaymentCommand(new StripeCommand());
  paymentSystem.executeCommand();
}run();
```

# 4.观察者模式

你订阅过时事通讯吗？

如果是这样的话，无论主人什么时候发邮件，你都会收到通知。

观察者模式以同样的机制工作。它为您提供了一个订阅模型，您可以订阅一个事件，当该事件发生时，您会收到通知。

示例:

```
function Newsletter() {
  this.observers = [];
}Newsletter.prototype = {
  subscribe: function (observer) {
    this.observers.push(observer);
  },
  unsubscribe: function(observer) {
    this.observers = this.observers.filter(ob => ob !== observer);
  }, notify: function() {
    this.observers.forEach(observer => console.log(‘Hello ‘ + observer.toString()));
  }
}let subscriber1 = ‘Subscriber 1’;
let subscriber2 = ‘Subscriber 2’;
let newsletter = new Newsletter();newsletter.subscribe(subscriber1);newsletter.subscribe(subscriber2);
newsletter.notify(); // Hello Subscriber 1 Hello Subscriber 2newsletter.unsubscribe(subscriber2);
newsletter.notify(); // Hello Subscriber 1
```

# 5.单一模式

这是最广为人知的模式。我们使用这种模式来限制一个类只有一个实例，并且它可以被全局访问。当你需要一些东西来处理整个应用程序中任何地方的特定任务时，这是非常有用的。

示例:

```
const utils = (function () {
  let instance;

  function initialize() {
    return {
      sum: function (a, b) {
        return a + b;
      }
    };
  } return {
    getInstance: function () {
      if (!instance) {
        instance = initialize();
      } return instance;
    }
  };
})();let sum = utils.getInstance().sum(3, 5); // 8
```

虽然这很方便，但您应该明智地使用这种模式，因为它有一个缺点，使您的代码难以测试。

# 6.模块模式

该模式是用于实现软件模块概念的设计模式。模块模式是一种强大的模式，通常在 JavaScript 中使用。

当您查看 JavaScript 库时，您会看到这种模式通常被用作单例对象。它使您能够封装您的代码来编写最健壮的应用程序。

您可以将模块中的函数、变量和属性作为公共或私有成员。

示例:

```
const bookModule = (function() {
  // private
  let title = ‘JavaScript’;
  let price = 15; // public
  return {
    printTitle: function () {
      console.log(title);
    }
  }
})();bookModule.printTitle(); // JavaScript
```

# 7.工厂模式

你可能在不知不觉中使用了它的模式。工厂模式就像一个工厂。在 JavaScript 中，它将对象的创建与代码的其余部分分开。您包装创建代码，然后公开 API 来生成不同的对象。

示例:

```
const Vehicle = function() {};const Car = function() {
  this.say = function() {
    console.log(‘I am a car’);
  }
};const Truck = function() {
  this.say = function() {
    console.log(‘I am a truck’);
  }
};const Bike = function() {
  this.say = function() {
    console.log(‘I am a bike’);
  }
};const VehicleFactory = function() {
  this.createVehicle = (vehicleType) => {
    let vehicle; switch (vehicleType) {
      case ‘car’:
        vehicle = new Car();
        break;
      case ‘truck’:
        vehicle = new Truck();
        break;
      case ‘bike’:
        vehicle = new Bike();
        break;
      default:
        vehicle = new Vehicle();
    }

    return vehicle;
  }
};const vehicleFactory = new VehicleFactory();let car = vehicleFactory.createVehicle(‘car’);
let truck = vehicleFactory.createVehicle(‘truck’);
let bike = vehicleFactory.createVehicle(‘bike’);car.say(); // I am a car
truck.say(); // I am a truck
bike.say(); // I am a bike
```

# 结论

根据我的经验，很难将设计模式应用到你的项目中。你必须多次尝试和失败，这样你才能掌握它。然而，努力是有回报的。一旦你完全理解了设计模式，并且知道什么时候应该实现什么模式，你就可以把你的代码提升到最健壮的水平。

以上只是一些设计模式的简单例子。如果你觉得这个话题有趣，你应该更深入地研究它。相信我，你永远不会后悔。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/15-simple-coding-techniques-to-get-your-tasks-done-with-shorter-code-in-javascript-59d46801db0) [## 15 种简单的编码技术，用更短的 JavaScript 代码完成任务

### 不要浪费时间写长代码，而你可以把它写得更短，更清晰，更易读。

medium.com](https://medium.com/javascript-in-plain-english/15-simple-coding-techniques-to-get-your-tasks-done-with-shorter-code-in-javascript-59d46801db0)