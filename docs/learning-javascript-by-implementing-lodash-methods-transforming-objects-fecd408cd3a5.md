# 通过实现 Lodash 方法学习 JavaScript 转换对象

> 原文：<https://javascript.plainenglish.io/learning-javascript-by-implementing-lodash-methods-transforming-objects-fecd408cd3a5?source=collection_archive---------18----------------------->

![](img/b27caedd0ee19e7b80cd338c59ea9372.png)

Photo by [Ricardo Rocha](https://unsplash.com/@rcrazy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将研究如何用普通 JavaScript 实现一些对象方法。

# `toPlainObject`

Lodash 的`toPlainObject`方法通过将继承的可枚举属性合并到对象本身来将对象转换为普通对象。

我们可以通过使用`Object.create`方法来实现它

然后我们使用`Object.getPrototypeOf`方法，然后将它们合并到对象自己的属性中。

例如，我们可以如下实现它:

```
const toPlainPObject = (obj) => {
  let plainObj = Object.create(null);
  plainObj = {
    ...obj
  };
  let prototype = Object.getPrototypeOf(obj)
  while (prototype) {
    plainObj = {
      ...plainObj,
      ...prototype
    }
    prototype = Object.getPrototypeOf(prototype)
  }
  return plainObj;
}
```

在上面的代码中，我们有我们的`toPlainObject`函数，它接受包含我们想要转换成普通对象的对象的`obj`参数。

然后，我们用带有参数`null`的`Object.create`创建一个新对象，它通过创建一个没有原型的对象来创建一个普通对象。

然后我们使用 spread 运算符将`obj`自己的属性合并到`plainObj`中。

接下来，我们调用传入了`obj`的`Object.getPrototypeOf`来获取`obj`的原型。然后我们使用一个`while`循环将原型的属性合并到`plainObj`中，并沿着原型链向上:

```
prototype = Object.getPrototypeOf(prototype)
```

最后，我们返回`plainObj`。

现在当我们这样称呼它时:

```
function Foo() {
  this.a = 1;
}Foo.prototype.b = 2;
console.log(toPlainPObject(new Foo()));
```

我们得到的控制台日志输出是`{a: 1, b: 2}`。

# `toSafeInteger`

Lodash 的`toSafeInteger`方法将一个值转换成一个安全的整数。它通过检查`Infinity`和`-Infinity`来做到这一点，然后分别返回最大安全整数和乘以 1 的最大安全整数。

我们可以如下实现它:

```
const toSafeInteger = val => {
  if (val === Infinity) {
    return Number.MAX_SAFE_INTEGER
  } else if (val === -Infinity) {
    return -1 * Number.MAX_SAFE_INTEGER
  }
  return isNaN(+val) ? 0 : Math.round(+val);
}
```

在上面的代码中，我们检查了`val`是`Infinity`还是`-Infinity`。如果是`Infinity`，那么我们返回`Number.MAX_SAFE_INTEGER`。如果`val`是`-Infinity`，那么我们返回`-Number.MAX_SAFE_INTEGER`。

否则，我们通过调用`isNaN`来检查`val`是否可以转换成数字。如果它返回`true`,那么我们返回 0，否则，我们在将`+val`转换成数字后用`Math.round`调用它。

那么我们可以这样称呼它:

```
console.log(toSafeInteger(3.2));
console.log(toSafeInteger(Number.MIN_VALUE));
console.log(toSafeInteger(Infinity));
console.log(toSafeInteger('3.2'));
```

然后，我们分别从每个中获得以下输出:

```
3
0
9007199254740991
3
```

![](img/2c72cbb1070765f9348984f8f2869734.png)

Photo by [freestocks](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `toString`

Lodash 的`toString`方法将给定的值转换成字符串。为`null`和`undefined`值返回一个空字符串。

例如，我们可以如下实现它:

```
const toString = val => {
  if (Object.is(val, -0)) {
    return '-0'
  } else if (Object.is(val, +0)) {
    return '+0'
  } else if (Object.is(val, null) || Object.is(val, undefined)) {
    return ''
  } else if (Array.isArray(val)) {
    return val.join(',');
  }
  return JSON.stringify(val);
}
```

在上面的代码中，我们使用了`Object.is`来检查`val`是否是列出的值之一。我们使用`Object.is`而不是`===`，因为`-0`和`+0`被认为与`Object.is`不同。

如果`val`不是`-0`、`+0`、`null`或`undefined`，那么我们使用`JSON.stringify`将对象转换为字符串。

那么我们可以这样称呼它:

```
console.log(toString(null));
console.log(toString(-0));
console.log(toString([1, 2, 3]));
```

然后我们从第一个控制台日志中得到一个空字符串，从第二个控制台日志中得到`'-0'`，从最后一个控制台日志中得到`1,2,3`。

# `invert`

方法翻转一个对象的键和值。如果两个属性具有相同的值，则后一个属性会覆盖前一个属性。

我们可以自己用`Object.keys`方法来获取键，用键和值填充一个新对象，如下所示:

```
const invert = (obj) => {
  let invertedObj = {};
  for (const key of Object.keys(obj)) {
    invertedObj[obj[key]] = key;
  }
  return invertedObj;
}
```

在上面的代码中，我们创建了`invertedObj`对象。然后，我们通过用`Object.keys`方法获取这些键，然后用`for...of`循环遍历它们，从而循环遍历这些键。

然后我们将`obj`的值填充为`invertedobj`的键，将`obj`的键填充为`invertedObj`的值。

最后，当我们这样称呼它时:

```
const object = {
  'a': 1,
  'b': 2,
  'c': 1
};console.log(invert(object));
```

我们从控制台日志输出中获取`{1: “c”, 2: “b”}`日志。

# 结论

为了实现`toPlainObject`方法，我们可以使用`Object.getPrototype`方法来遍历一个对象的原型链。

`toString`和`toSafeInteger`方法的相似之处在于，它们都对传入的参数进行一系列检查，然后相应地返回一些值。

最后，我们可以使用`Object.keys`来获取一个对象的键来实现`invert`。

## **来自简明英语团队的通知**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****