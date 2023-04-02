# 面向对象的 JavaScript——原型、调用和应用

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-prototypes-call-and-apply-af9d96b6df95?source=collection_archive---------11----------------------->

![](img/6ecb7ea6ebb1f610be48f1e9fb23e4b1.png)

Photo by [Rita Morais](https://unsplash.com/@moraisr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看原型、`call`和`apply`。

# 功能原型

函数有一个特殊的`prototype`属性，该属性包含在构造函数的所有实例之间共享的项。

例如，我们可以写:

```
function F() {}F.prototype = {
  name: 'james',
  say() {
    return `I am ${this.nam}`;
  }
};
```

然后，如果我们创建 2 个`F`实例:

```
const foo = new F();
const bar = new F();
console.log(foo, bar);
```

我们看到`foo`和`bar`具有相同的属性。

`say`方法也做同样的事情。

# 函数对象的方法

函数对象有自己的方法。

例如，函数使用`toString`方法返回包含函数代码的字符串。

所以如果我们有:

```
function add(a, b, c) {
  return a + b + c;
}console.log(add.toString());
```

我们得到:

```
"function add(a, b, c) {
  return a + b + c;
}"
```

记录在案。

# 打电话申请

函数有`call`和`apply`方法让我们运行函数，设置`this`的值并传递参数给它。

例如，如果我们有:

```
const obj = {
  name: 'james',
  say(who) {
    return `${this.name} is ${who}`;
  }
};
```

然后我们可以用`call`来调用`say`通过写:

```
const a = obj.say.call({
  name: 'mary'
}, 'female');
```

那么`a`就是`'mary is female’`。

我们将`this`设置为:

```
{
  name: 'mary'
}
```

所以`this.name`就是`'mary'`。

第二个参数是用于`say`的参数，因此`who`是`'female'`。

我们也可以通过做同样的事情来调用`apply`，除了函数的参数在数组中。

例如，我们可以写:

```
const obj = {
  name: 'james',
  say(who) {
    return `${this.name} is ${who}`;
  }
};const a = obj.say.apply({
  name: 'mary'
}, ['female']);
```

我们得到了同样的结果。

# 箭头函数中的词法 this

`this`是根据上下文变化的动态对象。

箭头函数不绑定到自己的值`this`。

所以如果我们有:

```
const obj = {
  prefix: "Hello",
  greet(names) {
    names.forEach(function(name) {
      console.log(`${this.prefix} ${name}`);
    })
  }
}
```

因为`this`是回调函数，所以它没有`prefix`属性，所以我们会得到一个关于`greet`方法的错误。

但是如果我们使用一个箭头函数:

```
const obj = {
  prefix: "Hello",
  greet(names) {
    names.forEach((name) => {
      console.log(`${this.prefix} ${name}`);
    })
  }
}
```

然后，该函数正常工作，因为它没有绑定到自己的值`this`。

# 推断对象类型

`Object.prototype`的`toString`方法给了我们用来创建对象的类名。

例如，我们可以写:

```
Object.prototype.toString.call({});
```

并获得:

```
"[object Object]"
```

如果我们写下:

```
Object.prototype.toString.call([]);
```

我们得到:

```
"[object Array]"
```

我们可以通过编写以下代码将`call`与`toString`方法一起使用:

```
Array.prototype.toString.call([1, 2, 3]);
```

我们得到:

```
"1,2,3"
```

这与以下内容相同:

```
[1, 2, 3].toString()
```

![](img/5e198f12d6383f623dc5150f714eb1c2.png)

Photo by [Julian Hochgesang](https://unsplash.com/@julianhochgesang?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`call`和`apply`方法让我们用不同的`this`值和参数调用函数。

此外，`prototype`拥有所有构造函数实例共享的属性。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**