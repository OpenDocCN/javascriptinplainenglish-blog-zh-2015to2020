# 现代 JavaScript 的精华——解决和拒绝承诺

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-resolve-and-reject-promises-b1167f50e78?source=collection_archive---------11----------------------->

![](img/d9db4e4c4bebbdcf53d27bbf23e4ccbc.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 承诺。

# 创造承诺的其他方式

除了使用`Promise`构造函数，我们还可以用其他方式创建承诺。

一种方法是`Promise.resolve`方法。

它将承诺的解析值作为其参数。

它会回报一个承诺。

例如，我们可以写:

```
Promise.resolve(1)
  .then(x => console.log(x));
```

`x`是来自`Promise.resolve`的解析值。

如果`x`是一个承诺，它的结构是接受者，那么这个承诺是不变的。

例如，如果我们有:

```
const p = new Promise(() => null);
console.log(Promise.resolve(p) === p);
```

然后我们从控制台日志中得到`true`。

如果参数是一个 thenable，那么参数中的`then`方法是一个函数，那么用`Promise.resolve`解析的值就是我们用来调用`then`参数的参数。

例如，如果我们有:

```
const thenableObj = {
  then(reaction) {
    reaction('foo');
  }
};
const promise = Promise.resolve(thenableObj);
console.log(promise instanceof Promise); 
promise.then(x => console.log(x));
```

我们有一个`thenableObj`，它有`then`方法。

它采用了一个`reaction`函数，我们用`'foo'`调用它。

然后我们向`Promise.resolve`方法传递一个对象，该方法返回一个承诺。

如果我们检查`promise`是否是`Promise`的实例，那么它将返回`true`。

我们还可以用回调来调用它的`then`以获得`'foo'`值，该值被赋给`x`。

# `Promise.reject()`

我们可以调用`Promise.reject()`来返回一个被有值拒绝的承诺。

例如，我们可以写:

```
const error = new Error('error');
Promise.reject(error)
  .catch(err => console.log(err === error));
```

我们通过将一个`error`对象传递给`Promise.reject`方法来传递这个对象。

然后我们通过向其传递回调来调用`catch`。

`err`有我们称之为`Promise.reject`的 error 对象，应该和`error`一样。

# 连锁承诺

我们可以用`then`连锁承诺，只要我们还一个承诺。

无论我们在`then`回调中返回什么，都将是承诺的解析值。

例如，如果我们有:

```
Promise.resolve('bar')
  .then(function(value1) {
    return 'foo';
  })
  .then(function(value2) {
    console.log(value2);
  });
```

那么`value2`就是`'foo'`。

这让我们展平承诺的锁链。

而不是写:

```
Promise.resolve('bar')
  .then(function(value1) {
    Promise.resolve('foo')
      .then(function(value2) {
        //...
      });
  })
```

我们写道:

```
Promise.resolve('bar')
  .then(function(value1) {
    return 'foo';
  })
  .then(function(value2) {
    //...
  });
```

# 捕捉错误

我们可以用`catch`方法捕捉错误。

例如，我们可以写:

```
Promise.reject(new Error('error'))
  .catch(function() {
    return 'error occurred';
  })
  .then(function(value) {
    //...
  });
```

我们调用`catch`来捕捉被拒绝的承诺中的错误，并运行我们自己的代码。

这让我们为下一个承诺设置一些值。

# 抛出异常

我们可以在`then`回调中抛出一个异常，那么返回的承诺将被拒绝。

例如，我们可以写:

```
Promise.resolve()
  .then(function(value) {
    throw new Error();
  })
  .catch(function(reason) {
    // ...
  });
```

我们在`then`回调中抛出了一个错误，所以`catch`回调将会运行。

![](img/b0d85c2f3fbdcb9f5dc1a79e14ec8c88.png)

Photo by [www.raubfisch24.de](https://unsplash.com/@raubfisch24de?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一个值来解析一个承诺，这样就可以调用下一个。

同样，我们可以拒绝承诺，用`catch`抓住错误。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**