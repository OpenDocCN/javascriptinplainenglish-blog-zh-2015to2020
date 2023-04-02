# JavaScript 重构—类

> 原文：<https://javascript.plainenglish.io/javascript-refactoring-classes-2fd5338a37b0?source=collection_archive---------5----------------------->

![](img/06b62853fd5e729e3a58c4cb977fc916.png)

Photo by [Sam Balye](https://unsplash.com/@sbk202?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以清理我们的 JavaScript 代码，这样我们可以更容易地使用它们。

在本文中，我们将研究一些与清理 JavaScript 类相关的重构思想。

# 内嵌类

我们从两个类开始，然后把它们合并成一个。这和之前的重构正好相反。

例如，不写:

```
class PhoneNumber {
  constructor(phoneNumber) {
    this.phoneNumber = phoneNumber;
  }
}class Person {
  constructor(name, phoneNumber) {
    this.name = name;
    this.phoneNumber = new PhoneNumber(phoneNumber);
  }
}
```

我们将`PhoneNumber`类的代码回滚到`Person`类，如下所示:

```
class Person {
  constructor(name, phoneNumber) {
    this.name = name;
    this.phoneNumber = phoneNumber;
  }
}
```

如果我们分离出来的类不是很复杂，我们可能会这么做。

在上面的代码中，我们有没有方法的`PhoneNumber`类，所以我们可以把它放到`Person`类中，因为`phoneNumber`是一个`Person`实例可以拥有的属性。

# 隐藏代理

我们可以在客户端类上创建方法，通过用直接获得我们想要的值的代码填充方法类来隐藏代码的底层实现。

这降低了我们获取想要的项目的代码的耦合性。

例如，不要写以下内容:

```
class Department {
  constructor(name, deptHead) {
    this.name = name;
    this.deptHead = deptHead;
  } getDeptHead() {
    return deptHead;
  }
}class Person {
  constructor(name, dept) {
    this.name = name;
    this.dept = dept;
  } getDept() {
    return this.dept;
  }
}const deptHead = new Person('jane');
const dept = new Department('customer service', deptHead);
const employee = new Person('joe', dept);
const manager = employee.getDept().getDeptHead();
```

这就要求我们在找到部门主管之前先找到部门，就像我们在最后一行所做的那样。

我们可以这样写:

```
class Department {
  constructor(name, deptHead) {
    this.name = name;
    this.deptHead = deptHead;
  }
}class Person {
  constructor(name, dept) {
    this.name = name;
    this.dept = dept;
  } getDept() {
    return this.dept;
  } getDeptHead() {
    return this.dept.deptHead;
  }
}const deptHead = new Person('jane');
const dept = new Department('customer service', deptHead);
const employee = new Person('joe', dept);
const manager = employee.getDeptHead();
```

我们所做的只是将`getDeptHead`方法添加到`person`中，以直接从部门获得部门主管。

然后我们不必先得到部门再得到部门负责人，这减少了`Person`和`Department`类之间的耦合。

# 引进外国方法

我们可以通过用`Object.assign`给一个不能直接修改的类添加方法来给它添加一个外来方法。

例如，我们可以编写以下内容:

```
class Foo {
  //...
}const mixin = {
  bar() {
    //...
  }
}Object.assign(Foo.prototype, mixin)
```

在上面的代码中，我们通过调用`Object.assign`向`Foo`添加了`bar`实例方法，因为`Foo`是一个我们不能修改的类。

# 引入本地扩展

除了直接向类中添加新方法之外，我们还可以通过将包含方法的类作为不能直接修改的类的子类，来创建包含我们想要调用的额外方法的新类。

例如，我们可以编写以下内容:

```
class Foo {
  //...
}class Bar extends Foo {
  bar() {
    //...
  }
}
```

在上面的代码中，我们创建了一个包含我们想要调用的方法的`Foo`的子类，其中`Foo`是一个我们不能更改的类。

现在我们不必通过向它的原型添加东西来修改`Foo`。

![](img/f60d1ec5db0a150c945f3f2241cb5060.png)

Photo by [Kimberly Farmer](https://unsplash.com/@kimberlyfarmer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 自封装字段

我们可以在直接访问的字段中添加 getters 和 setters 方法，这样我们就可以封装它。

例如，不要编写以下内容:

```
class Counter {
  inRange(arg) {
    return arg >= this._low && arg <= this._high;
  }
}
```

我们写道:

```
class Counter {
  inRange(arg) {
    return arg >= low() && arg <= high();
  } get low() {
    return this._low;
  } get high() {
    return this._high;
  }
}
```

这让我们可以封装字段，我们也可以对其应用其他操作，而不需要访问`high`和`low`字段，直接对它们应用操作。

# 结论

我们可以将一个类中不做很多事情的成员移到另一个类中，这样我们就可以删除不做很多事情的类。

为了减少类之间的耦合，我们可以用方法直接得到我们想要的，而不是用迂回的方式。

我们可以封装字段，这样我们就可以获得一个值，并在我们返回的值之前做一些事情。

## 来自简明英语团队的说明

简明英语刚刚推出了一个 YouTube 频道！我们希望你现在就订阅 [**来支持我们！**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)