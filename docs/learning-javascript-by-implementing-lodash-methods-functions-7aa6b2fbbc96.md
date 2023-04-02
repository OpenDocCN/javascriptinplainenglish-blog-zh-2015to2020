# 通过实现 Lodash 方法—函数来学习 JavaScript

> 原文：<https://javascript.plainenglish.io/learning-javascript-by-implementing-lodash-methods-functions-7aa6b2fbbc96?source=collection_archive---------8----------------------->

![](img/4b79471ae9135355829221ee682ee51f.png)

Photo by [Angel Luciano](https://unsplash.com/@roaming_angel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将研究如何实现一些 Lodash 方法来将函数转换成不同的形式。

# `bind`

Lodash `bind`函数返回一个我们设置的值为`this`的函数，并且部分应用了一些参数。

我们可以通过返回一个调用传入函数的`bind`方法的函数来实现这一点，并返回部分应用了参数的函数。

例如，我们可以编写以下代码:

```
const bind = (fn, thisArg, ...partials)=> { 
  return (...args) => fn.apply(thisArg, [...partials, ...args])
}
```

在上面的代码中，我们采用了`fn`函数、`thisArg`作为`this`的值，以及`partials`，这是一个要用`fn`调用的参数数组。

然后我们使用 JavaScript 的函数的`apply`方法调用`fn`并将`thisArg`作为第一个参数来设置`fn`中`this`的值和一个参数数组，我们使用 spread 操作符对其进行扩展。

我们首先展开`partials`来首先应用它们，然后我们展开在返回的函数中接收的`args`。

那么当我们这样称呼它的时候:

```
function greet(greeting, punctuation) {
  return `${greeting} ${this.user}${punctuation}`;
}const object = {
  'user': 'joe'
};
const bound = bind(greet, object, 'hi');
console.log(bound('!'))
```

我们看到控制台日志应该记录“嗨，乔！”因为我们将`greet`中`this`的值设置为`object`。然后我们首先应用`'hi'`的部分参数，然后用感叹号调用`bound`。

# `bindKey`

`bindKey`类似于`bind`,除了我们可以用它对一个对象的方法应用与`bind`相同的操作。

它也不允许我们改变`this`的值，并保持该值作为该方法所在的原始对象。

我们可以如下实现`bindKey`:

```
const bindKey = (object, key, ...partials) => {
  return (...args) => object[key].apply(object, [...partials, ...args])
}
```

在上面的代码中，我们将`object`、`key`和`partials`作为参数。其中`object`是带有我们想要调用`bindKey`的方法的对象。

`key`是我们想要应用部分参数的方法的关键，而`partials`有要应用的参数。

然后，我们可以如下运行它:

```
const object = {
  user: 'joe',
  greet(greeting, punctuation) {
    return `${greeting} ${this.user} ${punctuation}`;
  }
};
const bound1 = bindKey(object, 'greet', 'hi');
console.log(bound1('!'))object.greet = function(greeting, punctuation) {
  return `${greeting} ya ${this.user} ${punctuation}`;
};const bound2 = bindKey(object, 'greet', 'hi');
console.log(bound2('!'))
```

在上面的代码中，我们有带有`greet`方法的`object`，它返回一个引用`this.user`的字符串，应该是`'joe'`。

然后我们用`object`调用我们的`bindKey`函数，以及我们想要用来调用返回函数的参数，这些参数将被设置为`bound1`。

在此阶段，`bound1`将`'hi'`设置为`greeting`的值。然后当我们调用`bound1`时，我们将`'!'`传递给它，它用`punctuation`调用`bound1`。

然后我们从控制台日志中得到`'hi joe !’`。

之后我们把`object.greet`换了一个不同的功能。现在，当我们像在倒数第二行那样调用`bindKey`时，我们得到了`bound2`函数，该函数将`'hi'`字符串作为值 off `greeting`应用于`object.greet`方法。

然后当我们调用`bound2`时，我们将`'!'`应用为`punctuation`的值。因此，我们在控制台日志输出中记录了字符串`'hi ya joe !’`。

![](img/aa58c7f33d49b534ca370fdc771d2664.png)

Photo by [Olya Kuzovkina](https://unsplash.com/@o_l_l_a?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `delay`

Lodash `delay`方法在给定的等待时间(毫秒)后运行一个函数。它还带有可选参数，我们可以用这些参数调用我们的函数。

我们可以用`setTimeout`函数实现我们自己的函数，如下所示:

```
const delay = (fn, wait, ...args) => {
  setTimeout((...args) => fn(...args), wait, ...args)
}
```

在上面的代码中，我们只调用了`setTimeout`作为第一个参数。第二个参数有`wait`时间，而`args`是将被传递到回调的参数，这是所有后续的参数。

回调采用`args`方法，然后应用传入函数`fn`。

当我们这样称呼它时:

```
delay((text) => {
  console.log(text);
}, 1000, 'foo');
```

我们得到`'foo'`是由我们在 1 秒钟后传递到`delay`的函数记录的。

# 结论

为了实现`bind`和`bindKey` Lodash 方法，我们可以使用 JavaScript 函数的`apply`方法来更改`this`的值，并将参数应用于传入的函数。

我们可以用`setTimeout`函数创建自己的 Lodash `delay`方法，延迟函数调用和调用回调的参数需要几毫秒。

## **简明英语团队备注**

你知道我们有四种出版物吗？给他们一句如下的话来表达爱意: [**JavaScript 简单明了**](https://medium.com/javascript-in-plain-english) 、[T21【AI】简单明了](https://medium.com/ai-in-plain-english) 、[**UX**](https://medium.com/ux-in-plain-english)、[、 **Python 简单明了**、](https://medium.com/python-in-plain-english)、**——谢谢你们，继续学习吧！我们还推出了一个 YouTube，希望您通过订阅我们的简单英语频道** 来支持我们

像往常一样，简单英语想要帮助推广好的内容。如果您有任何想提交给我们的出版物的文章，请发邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的中位用户名和您感兴趣的写作内容，我们将回复您！**