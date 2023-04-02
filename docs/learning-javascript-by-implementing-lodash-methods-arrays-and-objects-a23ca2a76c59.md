# 通过实现 Lodash 方法学习 JavaScript 数组和对象

> 原文：<https://javascript.plainenglish.io/learning-javascript-by-implementing-lodash-methods-arrays-and-objects-a23ca2a76c59?source=collection_archive---------4----------------------->

![](img/fa7793256ff9713aacf0605bbfa9eaff.png)

Photo by [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是一个非常有用的工具库，让我们可以轻松地处理对象和数组。

然而，现在 JavaScript 标准库正在赶上诸如 Lodash 之类的库，我们可以用简单的方法实现许多功能。

在本文中，我们将研究 Lodash 方法，这些方法将数组压缩成对象属性和值，并遍历数组和对象。

# `zipObject`

Lodash `zipObject`方法将 2 个数组放入对象属性和值中。第一个数组包含属性名，第二个包含属性值。

我们可以如下实现我们自己的`zipObject`方法:

```
const zipObject = (props, vals) => {
  let obj = {};
  for (let i = 0; i < props.length; i++) {
    obj[props[i]] = vals[i];
  }
  return obj;
}
```

在上面的代码中，我们创建了一个空对象。然后我们遍历`props`数组来获取属性名和索引。

然后，我们通过使用索引访问`vals`数组中的条目，用我们自己的属性和相应的值填充`obj`对象。

那么当我们这样称呼它的时候:

```
const result = zipObject(['a', 'b'], [1, 2]);
```

然后我们得到`result`的值:

```
{
  "a": 1,
  "b": 2
}
```

# `countBy`

`countBy`方法从一个数组中创建一个对象，该数组将数组条目作为键，键通过`iteratee`函数处理。每个键的值是通过`iteratee`映射后键的数量。

`iteratee`接受一个参数并将该参数映射到其他东西。

我们可以如下实现它:

```
const countBy = (arr, iteratee) => {
  let obj = {};
  for (const a of arr) {
    if (typeof obj[iteratee(a)] === 'undefined') {
      obj[iteratee(a)] = 0;
    } else {
      obj[iteratee(a)]++;
    }
  }
  return obj;
}
```

在上面的代码中，我们通过用键调用`iteratee`来映射键，键是数组条目。

然后，当属性尚未定义时，我们将相应的值设置为 0。

否则，我们将该值增加 1。最后，我们返回`obj`。

那么当我们这样称呼它的时候:

```
const result = countBy([6.1, 4.2, 6.3], Math.floor);
```

我们得到的`result`是:

```
{
  "4": 0,
  "6": 1
}
```

![](img/07d8e2a1b38343ce8f5acb60a6ffdabe.png)

Photo by [Stavrialena Gontzou](https://unsplash.com/@stavrialena?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `forEach`

Lodash `forEach`方法遍历集合中的一个元素，并对每个元素调用`iteratee`。

`iteratee`函数可以通过显式返回`false`来停止循环的执行。

它可以将数组或对象作为第一个参数。

我们可以如下实现它:

```
const forEach = (collection, iteratee) => {
  if (Array.isArray(collection)) {
    for (const a of collection) {
      const result = iteratee(a)
      if (result === false) {
        break;
      }
    }
  } else {
    for (const a of Object.keys(collection)) {
      const result = iteratee(collection[a], a)
      if (result === false) {
        break;
      }
    }
  }
}
```

在上面的代码中，我们首先检查`collection`是否是一个数组。如果是，那么我们使用一个`for...of`循环遍历每个条目，并在每个条目上调用`iteratee`。

如果它返回`false`，那么我们停止循环。

同样，如果`collection`不是一个数组，那么我们通过用`Object.keys`获取键来遍历对象的键，然后对每个键-值对运行`iteratee`。

对象情况下的参数`iteratee`是值，然后是键，而数组情况下的`iteratee`只是数组条目。

如果`iteratee`返回`false`，那么我们停止循环。

那么当我们这样称呼它的时候:

```
forEach([1, 2], (value) => {
  if (value === 2) {
    return false;
  }
  console.log(value);
});forEach({
  'a': 1,
  'b': 2
}, (value, key) => {
  console.log(key);
});
```

我们得到第一个`forEach`日志 1 和第二个呼叫日志‘a’和‘b’。

# 结论

使用`for`循环，将键和值填充到一个对象中并返回它，可以很容易地实现`zipObject`方法。

`countBy`方法类似，但是值是每个项目的计数，这是通过首先调用一个`iteratee`函数来填充键并计数在调用`iteratee`函数后出现的项目数来确定的。

最后，当`iteratee`函数返回`false`时，可以通过`for...of`循环和中断循环来实现`forEach`函数。

## 来自简明英语团队的说明

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会与您联系！****