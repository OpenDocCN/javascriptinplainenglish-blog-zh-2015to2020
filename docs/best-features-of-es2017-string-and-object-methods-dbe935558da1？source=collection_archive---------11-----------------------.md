# ES2017 的最佳功能—字符串和对象方法

> 原文：<https://javascript.plainenglish.io/best-features-of-es2017-string-and-object-methods-dbe935558da1?source=collection_archive---------11----------------------->

![](img/a4ace83821e442a087e260fad3d1cd01.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2017 的最佳特性。

# `Object.entries()`

`Object.entries()`是 ES2017 中引入的对象静态方法。

它返回一个对象的键值对数组，每个键值对都在一个数组中。

例如，如果我们有:

```
const obj = {
  foo: 1,
  bar: 2
};for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

`Object.entries`接受一个对象并返回键值对。

然后，我们通过析构键值对数组来遍历键值对。

# 通过`Object.entries()`地图

因为`Object.entries`返回一个数组中的键值对数组，并且`Map`构造函数接受该数组，所以我们可以用它将一个对象转换成地图。

例如，我们可以写:

```
const map = new Map(Object.entries({
  foo: 1,
  bar: 2,
}));
console.log(JSON.stringify([...map]));
```

然后我们得到:

```
[["foo",1],["bar",2]]
```

# `Object.values()`

`Object.values()`从对象中返回一个值数组。

它也接受一个对象作为它的参数。

例如，我们可以写:

```
const obj = {
  foo: 1,
  bar: 2
};Object.values(obj)
```

然后我们得到返回的`[1, 2]`。

# `Object.getOwnPropertyDescriptors()`

`Object.getOwnPropertyDescriptors`方法让我们获得对象的属性描述符。

它只返回非继承属性的描述符。

例如，我们可以写:

```
const obj = {
  [Symbol('foo')]: 100,
  get bar() {
    return 'abc'
  },
};
console.log(Object.getOwnPropertyDescriptors(obj));
```

然后我们得到:

```
{
  bar: {
    configurable: true
    enumerable: true
  },
  Symbol(foo): {
    configurable: true
    enumerable: true
    value: 100
    writable: true
  }
}
```

`configurable`表示我们是否可以更改属性描述符。

`enumerable`指示属性是否与 for-in 循环一起列出。

`value`是属性值。

`writable`表示属性值是否可以改变。

我们可以用它将属性复制到一个对象中。

如果我们只是复制值，那么我们就会错过所有其他的属性描述符。

相反，我们可以调用`Object.getOwnPropertyDescriptors`来获取对象。

所以我们可以写:

```
const source = {
  foo: 1
};
const target = {};Object.defineProperties(target, Object.getOwnPropertyDescriptors(source));
console.log(Object.getOwnPropertyDescriptor(target, 'foo'));
```

然后我们将`source`的所有属性描述符复制到`target`。

我们可以设置`target.foo`的属性描述符来检查是否所有内容都被复制了。

# 字符串方法

ES2017 引入了新的弦乐方法。

新方法包括`padStart`和`padEnd`方法。

`padStart`让我们通过在字符串的开头添加一个或多个重复的字符，将字符串填充到给定的名称中。

例如，如果我们有:

```
'x'.padStart(5, 'yz')
```

然后我们得到:

```
"yzyzx"
```

同样，还有一个`padEnd`方法，它将字符串填充到给定的长度，并在字符串的末尾重复一个或多个字符。

例如，我们可以写:

```
'x'.padEnd(5, 'yz')
```

然后我们得到:

```
"xyzyz"
```

已退回。

在每个方法中，第一个参数是长度，第二个参数是我们要用来填充原始字符串的字符串。

![](img/f083cd85869aab49958ae72b490194df.png)

Photo by [Maarten van den Heuvel](https://unsplash.com/@mvdheuvel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES2017 有一些我们可以使用的方便的对象和字符串方法。

## **简明英语 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**