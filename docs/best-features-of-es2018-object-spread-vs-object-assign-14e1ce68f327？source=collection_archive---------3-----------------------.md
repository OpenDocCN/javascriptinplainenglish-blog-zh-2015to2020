# ES2018 的最佳功能—对象扩散与对象分配

> 原文：<https://javascript.plainenglish.io/best-features-of-es2018-object-spread-vs-object-assign-14e1ce68f327?source=collection_archive---------3----------------------->

![](img/93f5ea57da0cf98d5601694819aad53f.png)

Photo by [Bimata Prathama](https://unsplash.com/@bedeviere?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2018 的最佳特性。

# 克隆具有属性的对象原型

我们可以用属性克隆对象的原型。

为此，我们可以将`__proto__`属性设置为原始对象的原型:

```
const obj = {
  a: 1,
  b: 2
};const clone = {
  __proto__: Object.getPrototypeOf(obj),
  ...obj
};
```

我们设置了原型，并扩展了其他属性以获得更全面的克隆。

这与以下内容相同:

```
const clone = Object.assign(
  Object.create(Object.getPrototypeOf(obj)), obj);
```

# 合并两个对象

对象扩展操作符对于将多个对象合并成一个对象也很方便。

例如，我们可以写:

```
const merged = {
  ...obj1,
  ...obj2
};
```

这与以下内容相同:

```
const merged = Object.assign({}, obj1, obj2);
```

合并对象有助于将选项与默认选项合并:

```
const data = {
  ...DEFAULTS,
  ...options
};
```

# 散布物体与`Object.assign()`

传播算子和`Object.assign`非常相似。

不同之处在于 spread 定义新属性，而`Object.assign`设置它们。

例如，如果我们有:

```
Object.assign(target, obj1, obj2);
```

然后用`obj1`和`obj2`的属性就地修改`target`。

如果我们想用所有 3 个对象的属性创建一个新的合并对象，我们可以写:

```
const merged = Object.assign({}, target, obj1, obj2);
```

它从`target`、`obj1`和`obj2`返回一个新的对象。

扩展运算符与第二种使用`Object.assign`的方式相同。

Spread 和`Object.assign`都通过`get`操作读取数值。

例如，如果我们有:

```
const obj = {
  get a() {
    return 1
  }
}const clone = Object.assign({}, obj);
```

然后在`clone`中克隆`a` getter。

如果我们有:

```
const obj = {
  get a() {
    return 1
  }
}const clone = {
  ...obj
};
```

然后我们得到同样的结果。

扩展操作符在目标对象中定义新的属性。

但是`Object.assign`使用普通的设置操作来创建它们。

如果我们向`Object.prototype`添加一个 setter，创建对象，并在其上运行`Object.assign`:

```
Object.defineProperty(Object.prototype, 'foo', {
  set(value) {
    console.log('set', value);
  },
});const obj = {
  foo: 1
};const clone = Object.assign({}, obj);
```

然后，我们可以从控制台日志中看到 setter 正在运行。

另一方面，如果我们有:

```
Object.defineProperty(Object.prototype, 'foo', {
  set(value) {
    console.log('set', value);
  },
});
const obj = {
  foo: 1
};const clone = {
  ...obj
}
```

那么 setter 没有运行。

使用`Object.assign`，我们可以通过继承的只读属性来阻止它创建非继承的属性。

例如，我们可以写:

```
Object.defineProperty(Object.prototype, 'foo', {
    writable: false,
    value: 1,
});const obj = {
  foo: 1
};const clone = Object.assign({}, obj);
```

然后我们得到“未捕获的类型错误:无法分配给对象“# ”的只读属性“foo”。

另一方面，如果我们有:

```
Object.defineProperty(Object.prototype, 'foo', {
  writable: false,
  value: 1,
});const obj = {
  foo: 1
};
const clone = {
  ...obj
};
```

然后，我们没有得到任何错误，所以资产评级成功。

这是因为 spread 运算符定义属性，而不是设置属性。

![](img/1c786e15e66982bc9dc56c3d6c2023a0.png)

Photo by [Joe Green](https://unsplash.com/@jg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`Object.assign`做着相似的事情，却有着我们无法忽视的小差异。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)