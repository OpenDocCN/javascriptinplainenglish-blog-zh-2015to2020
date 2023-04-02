# JavaScript 最佳实践—填充、求幂和扩散

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-padding-exponentiation-and-spread-e326f873998?source=collection_archive---------16----------------------->

![](img/f088944bcf54a2f33ee8105954da8f77.png)

Photo by [Unda Tiltina](https://unsplash.com/@mermalade?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究语句之间的填充、取幂和扩展。

# 语句之间的填充行

我们不需要所有语句之间都有空行。我们只需要不相关的语句组之间的空行。

例如，对函数中的语句进行分组的一种方法是编写如下代码:

```
for (let i = 0; i < 10; i++) {
  console.log(i);
}for (let j = 0; j < 5; j++) {
  console.log(i);
}
```

在上面的代码中，我们在 2 个`for`循环之间有一个空行，因为每个循环都是它自己的逻辑组。

我们只需要在要分组的语句组之间有一个空行。空行告诉我们，他们应该作为一个群体来阅读。

否则，空行是对空间的浪费，我们可以删除它们以节省垂直空间。

# 使用`Math.pow`与`**`操作符

`Math.pow`是一种让我们在从第一个版本到当前版本的所有 JavaScript 版本中做幂运算的方法。

它接受两个参数，即底数和指数，并返回底数的给定指数。

例如，我们可以如下使用它:

```
const a = Math.pow(2, 3);
```

然后我们得到`a`是 8，因为 2 是 3 的幂。

它也适用于分数幂和负幂。例如，我们可以写:

```
const a = Math.pow(2, 0.5);
```

并得到`a`为 1.4142135623730951。我们也可以写:

```
const a = Math.pow(2, -1);
```

并得到`a`为 0.5。

此外，我们可以用表达式代替数字，如下所示:

```
const x = 1,
  y = 2,
  z = 3;
const a = Math.pow(x + y, z);
```

然后我们得到`a`是 27，因为`x`是 1，`y`是 2，`c`是 3，所以我们将基数 3 提升到 3 的幂。

ES2015 引入了指数运算符。用`**`表示。

我们可以用它来做幂运算，如下所示:

```
const a = 2 ** 3;
```

并得到`a`为 8。

使用`**`运算符，分数、负幂和指数都如我们所预期的那样工作。例如，我们可以给他们写信如下:

```
const a = 2 ** 0.5;
const b = 2 ** -1;
```

对于表达式，我们可以写成:

```
const x = 1,
  y = 2,
  z = 3;
const a = (x + y) ** z;
```

正如我们所看到的，使用取幂操作符更短，我们得到的结果是一样的，而且比调用一个我们不需要调用的方法更易读。

我们节省了大量的输入并做同样的事情，所以我们应该使用取幂操作符而不是调用`Math.pow`来做取幂操作。

# 调用时使用对象扩展运算符`Object.assign`

从 ES2018 开始，扩展操作符在对象上工作。它允许我们对对象进行浅层复制，或者将多个对象合并成一个新对象。

在 ES6 和 ES2017 之间，我们必须使用`Object.assign`将多个对象合并成一个，或者对其进行浅层复制。

使用`Object.assign`，我们通过编写以下代码来制作一个对象的浅层副本:

```
const foo = {
  a: 1
};
const bar = Object.assign({}, foo);
```

在上面的代码中，我们定义了`foo`对象。然后我们调用了`Object.assign`，用一个空对象作为第一个参数，用`foo`对象作为第二个参数来返回`foo`对象的一个浅层副本。

浅层复制是指只复制顶级属性。嵌套对象仍然引用原始对象。

如果我们记录表达式`foo === bar`，它返回`false`，这意味着`foo`和`bar`没有在内存中引用同一个对象。因此，我们知道我们做了对象的浅层拷贝。

`Object.assign`的第一个参数是要复制到的目标对象，其余的参数是我们想要复制到目标对象中的源对象，我们想要多少就有多少。

为了用`Object.assign`将多个对象合并在一起，我们只需将更多的对象作为参数传递给它。

例如，我们可以编写以下代码:

```
const foo = {
  a: 1
};
const baz = {
  b: 2
};
const qux = {
  c: 3
};
const bar = Object.assign({}, foo, baz, qux);
```

然后我们得到`bar`就是`{a: 1, b: 2, c: 3}`。

spread 运算符使这变得更简单。有了它，我们就不用调用函数来合并和制作对象的浅层副本了。我们所要做的就是使用 spread 运算符，用`...`表示如下:

```
const foo = {
  a: 1
};
const baz = {
  b: 2
};
const qux = {
  c: 3
};
const bar = {
  ...foo,
  ...baz,
  ...qux
};
```

上面的代码也将所有的对象合并成一个，就像我们对上面的`Object.assign`所做的一样。

因此我们得到`bar`是`{a: 1, b: 2, c: 3}`。

我们可以制作一个对象的浅层副本，如下所示:

```
const foo = {
  a: 1
};
const bar = {
  ...foo,
};
```

我们得到`bar`是`{ a: 1 }`，但是`foo`和`bar`与`===`运算符相比不相等，因为它们不引用同一个对象。

![](img/e807f9f66205baf9e743b7c6d9588865.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

应该使用扩展和取幂运算符，而不是它们的旧版本。

我们不需要在每个语句后都加一行。我们需要在一组要组合在一起的语句后添加一个新的空行。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****