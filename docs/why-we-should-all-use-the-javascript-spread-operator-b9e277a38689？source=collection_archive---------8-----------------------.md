# 为什么我们都应该使用 JavaScript Spread 运算符？

> 原文：<https://javascript.plainenglish.io/why-we-should-all-use-the-javascript-spread-operator-b9e277a38689?source=collection_archive---------8----------------------->

![](img/4c323f081b7be29d09d9c11185b720eb.png)

Photo by [Vernon Raineil Cenzon](https://unsplash.com/@thevernon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript spread 操作符对于许多我们可能不知道它能做的事情非常有用。

在本文中，我们将看看它为什么有用。

# 传播函数调用

spread 运算符可用于将一组对象和值扩展到参数中。

例如，我们可以写:

```
const min = Math.min(...[1, 2, 2, 3, 5]);
```

从数组中找出最小的数。

这比使用`apply`做同样的事情要好得多，因为我们不必将`this`的值作为第一个参数传入。

# 复制数组

我们可以使用 spread 操作符将一个数组浅层复制到另一个数组中。

例如，如果我们有:

```
const arr = [1, 2, 3];
const arrCopy = [...arr];
```

当我们记录两者的值时，我们会看到它们具有相同的值，但是当我们记录以下值时:

```
arr === arrCopy
```

我们看到它是`false`。因此，我们知道它们引用的不是同一个数组。

# 串联数组

我们可以通过将 spread 运算符应用于多个数组来将多个数组连接起来。

例如，我们可以写:

```
const arr1 = [0, 1, 2];
const arr2 = [3, 4, 5];
const arr = [...arr1, ...arr2];
```

那么我们得到`arr`的值是:

```
[0, 1, 2, 3, 4, 5]
```

这比使用`concat`方法将两个数组连接在一起要容易得多。

# 复制对象

我们也可以使用 spread 操作符来复制对象。例如，我们可以写:

```
const obj = {
  a: 1,
  b: 2
};
const objCopy = {
  ...obj
};
```

然后当我们记录两者的值时，我们会得到相同的值。

当我们记录以下值时:

```
obj === objCopy
```

我们会看到`false`日志。这意味着`obj`和`objCopy`没有引用同一个对象，但是有相同的内容。

# 合并对象

我们可以使用 spread 运算符将对象合并为一个。如果一个属性在多个对象中具有相同的名称，则最后一个对象的值将覆盖之前的值。

例如，如果我们写下如下，那么我们得到:

```
const obj = {
  a: 1,
  b: 2
};const obj2 = {
  a: 3,
  c: 2
};
const mergedObj = {
  ...obj,
  ...obj2
};
```

我们得到的`mergedObj`的值是:

```
{a: 3, b: 2, c: 2}
```

![](img/71dc8a17c3e94d03bccecbcc55dc1826.png)

Photo by [Louis Hansel](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将类似数组的对象转换为数组

我们可以用 spread 操作符将类似数组的对象转换成数组。

类似数组的对象包括像`Set`、`Map`、`arguments`对象、节点列表和其他任何有`Symbol.iterator`方法的对象。

例如，我们可以使用它将一个节点列表转换成 DOM nodes 对象的数组，如下所示。

假设我们有下面的 HTML:

```
<div>
  foo
</div>
<div>
  bar
</div>
```

我们可以找到内部带有文本“bar”的`div`,如下所示:

```
const divs = [...document.querySelectorAll('div')];
const barDiv = divs.find(div => div.innerText === 'bar');
```

一旦我们用 spread 操作符转换了由`querySelectorAll`返回的节点列表，我们就可以使用任何像`find`这样的数组方法来做我们想做的事情。

节点列表没有任何数组方法，但是它们可以通过循环来循环，有一个`length`属性，每个条目有一个索引。

同样，我们可以将`Set`转换成数组，如下所示:

```
const arr = [...new Set([1, 2, 3, 3])];
```

那么我们得到`arr`的值是:

```
[1, 2, 3]
```

我们可以对其他类型的类似数组的对象做同样的事情。

# 集合操作

由于数组有许多过滤数据的方法，我们可以使用这些方法来做普通的集合操作，这些操作不能用`Set`实例本身来做，因为它们没有方法来做这些事情。

我们可以通过将集合转换为数组来做集合并和交集这样的事情，然后用数组方法做运算，然后我们可以将数组结果转换回集合。

例如，如果我们想找到两个集合的交集，我们可以写:

```
const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);
const intersection = [...setA, ...setB].filter(el => [...setA].includes(el) && [...setB].includes(el));
console.log(new Set(intersection));
```

然后我们得到`intsection`中只有元素 3，因为 3 在`setA`和`setB`中都存在。

我们所做的是将`setA`和`setB`转换成数组，然后在合并后的数组上调用`filter`，将两个集合扩展成一个大数组。

然后我们用谓词调用数组上的`filter`:

```
[...setA].includes(el) && [...setB].includes(el)
```

找出两个集合中的元素。

最后，我们通过在`Set`构造函数中传递将结果数组转换回集合。

# 结论

spread 运算符对于浅层复制数组和对象很有用。

它还可以用于合并数组和对象。

我们还可以用它很容易地将类似数组的对象转换成数组，这样我们就可以在它上面调用类似数组的对象上没有的数组方法。

这使得在其中查找内容和使用集合进行操作变得更加容易。

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。