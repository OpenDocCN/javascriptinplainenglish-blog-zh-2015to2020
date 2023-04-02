# 现代 JavaScript 的精华—代理和对象操作

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-proxies-and-object-operations-4c1adc657757?source=collection_archive---------15----------------------->

![](img/b224de06637bdd2a3de32f2b2a37fea7.png)

Photo by [K. Mitch Hodge](https://unsplash.com/@kmitchhodge?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究使用 JavaScript 代理的元编程。

# 包装内置构造函数的实例

我们可以使用代理来包装内置构造函数的实例。

例如，我们可以写:

```
const target = new Date();
const handler = {};
const proxy = new Proxy(target, handler);
```

但是如果我们写:

```
console.log(proxy.getDate());
```

我们得到“未捕获的类型错误:这不是一个日期对象。”因为我们在代理上调用了`getDate`，而不是原来的`Date`实例。

然而，代理可以透明地用数组包装。

例如，我们可以写:

```
const p = new Proxy(new Array(), {});
p.push('foo');
console.log(p.length);
```

我们用`Array`构造函数创建了一个代理。

处理程序对象是一个空对象。

我们调用`push` with 向数组中插入一个条目。

它与代理一起工作。

而我们得到的`length`跟我们预料的一样。

对于不能用代理透明包装的对象，我们可以修改处理程序，让它做我们期望的事情。

例如，我们可以写:

```
const handler = {
  get(target, propKey, receiver) {
    if (typeof target[propKey] === 'function') {
      return target[propKey].bind(target);
    }
    return target[propKey]
  },
};const target = new Date();
const proxy = new Proxy(target, handler);
```

然后我们调用`target`的`propKey`方法。

在`get`方法中，我们检查`target[propKey]`的类型是否是`'function'`。

如果是，那么我们用`bind`改变`this`的值，这样函数将具有正确的值`this`。

否则，我们只是原样返回值。

现在我们可以在对象上调用`getDate`方法，没有问题:

```
const handler = {
  get(target, propKey, receiver) {
    if (typeof target[propKey] === 'function') {
      return target[propKey].bind(target);
    }
    return target[propKey]
  },
};const target = new Date();
const proxy = new Proxy(target, handler);console.log(proxy.getDate());
```

# 代理的用例

我们可以使用代理来记录对象的操作。

例如，我们可以写:

```
const handler = {
  get(target, propKey, receiver) {
    console.log(target[propKey])
    if (typeof target[propKey] === 'function') {
      return target[propKey].bind(target);
    }
    return target[propKey]
  },
};class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
  toString() {
    return `Person(${this.firstName}, ${this.lastName})`;
  }
}const target = new Person('jane', 'smith');
const proxy = new Proxy(target, handler);console.log(proxy.toString());
```

跟踪对象操作。

我们得到了`toString`方法的结构。

然后我们得到返回的结果。

我们还可以使用代理来警告未知的属性。

例如，我们可以写:

```
const target = {};
const handler = {
  get(target, propKey, receiver) {
    if (!(propKey in target)) {
      throw new Error('property does not exist');
    }
    return target[propKey];
  }
};const proxy = new Proxy(target, handler);
console.log(proxy.foo);
```

我们有一个拥有`get`方法的`handler`对象。

在其中，我们使用`in`操作符来检查是否有名为`propKey`的属性。

由于在`target`中没有`foo`属性，我们得到‘未捕获的错误:属性不存在’。

这是一个方便的特性，JavaScript 对象本身没有这个特性。

![](img/e6edb5ac657528117750fbe8e7f39a88.png)

Photo by [Stephanie Harvey](https://unsplash.com/@stephanieharvey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用代理以各种方式记录对象操作和拦截对象操作。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**