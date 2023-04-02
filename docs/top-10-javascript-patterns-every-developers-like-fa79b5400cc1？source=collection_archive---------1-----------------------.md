# 每个开发人员都喜欢的 10 大 JavaScript 模式

> 原文：<https://javascript.plainenglish.io/top-10-javascript-patterns-every-developers-like-fa79b5400cc1?source=collection_archive---------1----------------------->

![](img/f90698c3c7a7d201e3b72cffe898e0d6.png)

# 1.构造器模式

在经典的面向对象编程语言中，构造函数是一种特殊的方法，用于在为新创建的对象分配内存后初始化该对象。在 JavaScript 中，几乎所有东西都是对象，我们最感兴趣的是对象构造函数。因为对象构造函数习惯于创建特定类型的对象，例如，在第一次创建对象时，既准备对象以供使用，又接受构造函数可以用来设置成员属性和方法的值的参数。

![](img/11e219c5765b4bc6e58a6c075c86269e.png)

正如我们所知，JavaScript 不支持类的概念，因此在构造函数中，关键字 this 引用了正在创建的新对象重新查看对象创建，一个基本的构造函数可能如下所示:

```
function Car(model, year, miles) {
  this.model = model;
  this.year = year;
  this.miles = miles;
}// Usage:
var bmw = new Car('M4', '2019', '1000');
```

# 2.桃木鸟

模块是任何健壮的应用程序架构不可或缺的一部分，通常有助于保持项目的代码单元清晰地分离和组织。其中包括:

*   目标文字符号
*   模块模式
*   AMD 模块
*   CommonJS 模块
*   ECMAScript 和声模块

对象文字:

```
var newObject = {
  variableKey: variableValue,
  functionKey: function() {
    //…
  }
};
```

模块模式:

![](img/dc0aa4148308b60a651bfbb8dffe7715.png)

让我们通过创建一个自包含的模块来开始研究模块模式的实现。

```
var testModule = (function() {
  var counter = 0;
  return {
    incrementCounter: function() {
      return ++counter;
    },
    resetCounter: function() {
      counter = 0;
    }
  };
})();// Usage:
testModule.incrementCounter();
testModule.resetCounter();
```

# 3.启示模块模式

揭示模块可以做的一件事是，当我们想从一个公共方法调用另一个公共方法或访问公共变量时，避免重复主对象的名称。

```
var myRevealingModule = (function() {
  var privateVariable = 'not okay',
    publicVariable = 'okay';
  function privateFun() {
    return privateVariable;
  }function publicSetName(strName) {
    privateVariable = strName;
  }function publicGetName() {
    privateFun();
  }return {
    setName: publicSetName,
    message: publicVariable,
    getName: publicGetName
  };
})();//Usage:myRevealingModule.setName('Marvin King');
```

# 4.单一模式

单例模式之所以为人所知，是因为它将一个类的实例化限制为一个对象。单例不同于静态类，因为我们可以延迟它们的初始化。通常，因为它们需要一些在初始化时可能不可用的信息。对于不知道以前对它们的引用的代码，它们不提供容易检索的方法。让我们来看看 singleton 的结构:

```
var singletonPattern = (function() {
  var instance;
  function init() {
    // Singleton
    function privateMethod() {
      console.log('privateMethod');
    }
    var privateVariable = 'this is private variable';
    var privateRandomNumber = Math.random();
    return {
      publicMethod: function() {
        console.log('publicMethod');
      },
      publicProperty: 'this is public property',
      getRandomNumber: function() {
        return privateRandomNumber;
      }
    };
  }return {
    // Get the singleton instance if one exists
    // or create if it doesn't
    getInstance: function() {
      if (!instance) {
        instance = init();
      }
      return instance;
    }
  };
})();// Usage:
var single = singletonPattern.getInstance();
```

# 5.观察者模式

观察者是一种设计模式，在这种模式中，一个对象维护一个依赖于它的观察者的对象列表，自动通知他们状态的任何变化。

![](img/35fc626dcf4ea66835081521a5cb7a5e.png)

*   科目
*   维护观察员列表，添加或删除观察员的工具
*   观察者
*   为需要被通知主题状态更改的对象提供更新的接口
*   具体主体
*   向观察者广播状态变化的通知存储具体观察者的状态
*   具体观察者
*   存储对 ConcreteSubject 的引用，为观察者实现更新的接口，以确保状态与主题一致

```
function ObserverList() {
  this.observerList = [];
}ObserverList.prototype.Add = function(obj) {
  return this.observerList.push(obj);
};ObserverList.prototype.Empty = function() {
  this.observerList = [];
};ObserverList.prototype.Count = function() {
  return this.observerList.length;
};ObserverList.prototype.Get = function(index) {
  if (index > -1 && index < this.observerList.length) {
    return this.observerList[index];
  }
};//...
```

当一个主题需要通知观察者一些有趣的事情发生时，它向观察者广播一个通知(包括与通知主题相关的特定数据)

当我们不再希望某个特定的观察者被其注册的主体通知变化时，该主体可以将其从观察者列表中删除。将来，我会更多地讨论如何在 JavaScript 中广泛使用 observer 的特性。

# 6.中介模式

![](img/bcf54fe95fec5888f45589dcd855d7f6.png)

如果一个系统的组件之间有太多的直接关系。现在可能是时候建立一个控制中心点来代替组件之间的通信了。中介模式通过确保组件不显式地相互引用来促进松散耦合。

```
var mediator = (function() {
  var topics = {};
  var subscribe = function(topic, fn) {
    if (!topics[topic]) {
      topics[topic] = [];
    }
    topics[topic].push({ context: this, callback: fn });
    return this;
  };// publish/broadcast an event to the rest of the application
  var publish = function(topic) {
    var args;
    if (!topics[topic]) {
      return false;
    }
    args = Array.prototype.slice.call(arguments, 1);
    for (var i = 0, l = topics[topic].length; i < l; i++) {
      var subscription = topics[topic][i];
      subscription.callback.apply(subscription.content, args);
    }
    return this;
  };
  return {
    publish: publish,
    subscribe: subscribe,
    installTo: function(obj) {
      obj.subscribe = subscribe;
      obj.publish = publish;
    }
  };
})();
```

# 7.原型模式

使用原型模式的一个好处是，我们利用了 JavaScript 固有的原型优势，而不是试图模仿其他语言的特性。让我们看看模式示例。

```
var myCar = {
  name: 'bmw',
  drive: function() {
    console.log('I am driving!');
  },
  panic: function() {
    console.log('wait, how do you stop this thing?');
  }
};//Usages:var yourCar = Object.create(myCar);console.log(yourCar.name); //'bmw'
```

# 8.工厂模式

工厂可以为创建对象提供一个通用接口，在这里我们可以指定我们想要创建的工厂对象的类型。见下图。

![](img/13669a95a2eabd787901254096b7c660.png)

```
function Car(options) {
  this.doors = options.doors || 4;
  this.state = options.state || 'brand new';
  this.color = options.color || 'silver';
}
```

# 9.混合模式

Mixins 是提供功能的类，这些功能很容易被一个子类或一组子类继承，以实现功能重用。

```
var Person = function(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.gender = 'male';
};var clark = new Person('Clark', 'kent');var Superhero = function(firstName, lastName, powers) {
  Person.call(this.firstName, this.lastName);
  this.powers = powers;
};SuperHero.prototype = Object.create(Person.prototype);
var superman = new Superhero('Clark', 'Kent', ['flight', 'heat-vision']);console.log(superman); //output personal attributes as well as power
```

在这种情况下，超级英雄能够用特定于其对象的值来覆盖任何继承的值。

![](img/1f8728e03e1c3003fb8c0960a280664a.png)

# 10.装饰图案

装饰者是一种旨在促进代码重用的结构化设计模式。类似于 Mixins，它们可以被认为是对象子类化的另一种可行的替代方案。传统上，Decorators 提供了向系统中的现有类动态添加行为的能力。这个想法是装饰本身对于类的基本功能并不重要。让我们看看装饰器是如何在 JavaScript 中工作的

```
function MacBook() {
  this.cost = function() {
    return 997;
  };
  this.screenSize = function() {
    return 11.6;
  };
}// Decorator 1function Memory(macbook) {
  var v = macbook.cost();
  macbook.cost = function() {
    return v + 75;
  };
}// Decorator 2function Engraving(macbook) {
  var v = macbook.cost();
  macbook.cost = function() {
    return v + 200;
  };
}// Decorator 3function Insurance(macbook) {
  var v = macbook.cost();
  macbook.cost = function() {
    return v + 250;
  };
}var mb = new MacBook();Memory(mb);
Engraving(mb);
Insurance(mb);mb.cost(); // 1522
```

并非所有的模式都适用于一个项目，有些项目可能会受益于观察者模式提供的解耦优势。也就是说，一旦我们牢牢掌握了设计模式和它们最适合的特定问题。因此，集成到我们的应用程序架构中变得更加容易。