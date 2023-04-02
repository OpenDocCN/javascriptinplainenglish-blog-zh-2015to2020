# MobX 修改器列表

> 原文：<https://javascript.plainenglish.io/list-of-mobx-modifiers-d266b9957ec6?source=collection_archive---------10----------------------->

![](img/46f62f7e3a5da1316a9fb3433427c1d2.png)

Photo by [Qijin Xu](https://unsplash.com/@obkim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

MobX 有一组装饰器来改变可观察属性的行为。

在本文中，我们将一个一个地看它们，看看我们如何使用它们。

# 修饰语

MobX 附带了以下装饰器，它们定义了可观察属性的行为方式:

*   `observable`—`observable.deep`的别名
*   `observable.deep` —任何可观察对象使用的默认修饰符。它克隆和转换任何数组，地图或普通对象到它的可观察的对应物
*   `observable.ref` —禁用自动可观察转换，改为创建可观察参考
*   `observable.shallow` —只能用于集合。将任何指定的集合转换为可观察的集合，但是值将按原样处理
*   `observable.struct` —类似于`ref`，但忽略结构上等于当前值的新值
*   `computed` —创建派生属性
*   `computed(options)` —与`computed`相同，但设置了附加选项
*   `computed.struct` —与`computed`相同，但仅当产生的值在结构上不同于之前的值时，才通知任何 is 观察者。
*   `action` —创建一个操作
*   `action(name)` —创建操作并覆盖名称
*   `action.bound` —创建操作并将`this`绑定到实例。

装饰者可以使用 MobX 的`decorate`、`observable.object`、`extendObservable`和`observable`来指定对象成员应该如何表现。

默认情况下，`observable.deep`是任何键值对的默认行为，而`computed`是 getters 的默认行为。

例如，我们可以定义一个可观察值如下:

```
import { observable, action } from "mobx";const person = observable(
  {
    firstName: "John",
    lastName: "Smith",
    age: 42, get fullName() {
      return `${this.firstName} ${this.lastName}`;
    }, setAge(age) {
      this.age = age;
    }
  },
  {
    setAge: action
  }
);
```

在上面的代码中，除了被我们明确定义为`action`的`setAge`之外，所有成员都有默认的装饰器。

因此，`firstName`、`lastName`、`age`是`observable` s，`fullName`是`computed`字段。

我们可以如下使用`decorate`功能:

```
import { observable, action, decorate } from "mobx";class Person {
  firstName = "John";
  lastName = "Smith";
  age = 42; get fullName() {
    return `${this.firstName} ${this.lastName}`;
  } setAge(age) {
    this.age = age;
  }
}decorate(Person, {
  firstName: observable,
  lastName: observable,
  age: observable,
  setAge: action
});
```

在上面的代码中，我们用类作为第一个参数来调用`decorate`，然后用一个包含我们想要修改的字段的对象作为第二个参数。

我们将`firstName`、`lastName`和`age`设为可观察值，将`setAge`设为动作。

`fullName`是一个计算字段，因为它是 getters 的默认选项。

![](img/39797147fa92b28bc02095f194b3ecc4.png)

Photo by [Qijin Xu](https://unsplash.com/@obkim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 深度可观测性

当 MobX 使用`observable`、`observable.object`或`extendObservable`创建一个可观察对象时，它引入了默认使用`deep`修饰符的可观察属性。

深度修饰符递归地调用`observable(newValue)`任何使用`deep`修饰符的赋值，直到它到达对象的底层。

# 参考可观测性

在某些情况下，物体不需要转换成可观察的。例如，我们不想为不可变对象或由外部库管理的对象这样做。

在这种情况下，我们可以使用`ref`修改器。

例如，我们可以如下使用它:

```
class Person {
  firstName = "John";
  lastName = "Smith";
  @observable.ref age = 42;
}
```

在上面的代码中，我们给`age`添加了`observable.ref`装饰器，这样 MobX 将只跟踪它的引用，而不会试图转换它的值。

使用 ES5 语法，我们可以编写以下代码:

```
import { observable, extendObservable } from "mobx";function Person() {
  extendObservable(
    this,
    {
      name: "Joe",
      age: 42
    },
    {
      age: observable.ref
    }
  );
}
```

# 浅层可观察性

我们可以使用`observable.shallow`修改器来应用一级深度的可观察性。我们需要这个来创建一个可观察的引用集合。

它不会像`deep`那样递归地应用可观测性。

例如，我们可以如下使用它:

```
class Books {
  @observable.shallow authors = [];
}
```

`{ deep: false }`可以作为选项传递给`observable`、`observable.object`、`observable.array`、`observable.map`和`extendObservable`来创建浅层集合。

# 结论

我们可以使用修饰符来改变 MobX 观察值变化的方式。

默认情况下，它递归地查看值的变化。

还有用于计算值、浅层观察等的修改器。

## JavaScript 用简单的英语写的一个注释:

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**