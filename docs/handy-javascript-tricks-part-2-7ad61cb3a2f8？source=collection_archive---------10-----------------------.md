# 方便的 JavaScript 技巧—第 2 部分

> 原文：<https://javascript.plainenglish.io/handy-javascript-tricks-part-2-7ad61cb3a2f8?source=collection_archive---------10----------------------->

![](img/18a326d03ff3914ff80eeaf13df97e9b.png)

Photo by [Anthony Mapp](https://unsplash.com/@tonito?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 和其他编程语言一样，有许多方便的技巧，让我们可以更容易地编写程序。在本文中，我们将看看如何将对象属性和数组元素分解成单独的变量，将多个对象合并成一个，并用`URL`对象操纵 URL。

# 分解对象属性和数组元素

使用 ES6，我们可以使用快捷方式将一个对象的值赋给它自己的变量，也可以将单个数组条目赋给它们自己的变量。多亏了析构赋值语法，我们可以做到这一点，而不需要通过索引显式地检索对象键值对或数组条目。

在对象上使用它的最简单的方法是编写如下代码:

```
const {
  a,
  b
} = {
  a: 1,
  b: 2
};
```

使用上面的代码，JavaScript 解释器将右边的键名与右边的变量名匹配。这样，它可以将 1 分配给`a`，将 2 分配给`b`。我们也可以把右边的值赋给左边一个不同名字的变量。为此，我们可以编写以下代码:

```
const {
  a: foo,
  b: bar
} = {
  a: 1,
  b: 2
};
```

上面的代码首先将右边的键名与左边的键名进行匹配，然后将匹配这些键名的值传递给左边冒号右边的变量。

这意味着右边的`a`键将与左边的`a`键匹配。这意味着右边的值为 1 的`a`将被赋给变量名，即`a`键的值，即`foo`。

同样，右边的`b`键将与左边的`b`键匹配，右边的`b`键的值将被分配给与左边的`b`键对应的变量名。所以最后我们得到的是变量`foo`为 1，变量`bar`为 2。

我们可以把默认值赋给左边的变量，这样就不用担心它们在析构赋值操作后被`undefined`了。

为了做到这一点，我们编写下面的代码，像典型的赋值操作一样，用`=`操作符设置左侧变量的默认值。例如，我们可以为左边的变量设置默认值，如下面的代码所示:

```
const {
  a = 0,
  b = 0
} = {
  a: 1
};
console.log(a, b);
```

如果我们像上面那样记录`a`和`b`的值，我们应该为`a`得到 1，为`b`得到 0，因为我们没有为左侧的`b`分配任何值，所以我们指定的默认值 0 会像我们指定的那样自动分配给`b`的值。

同样，我们可以对数组使用析构赋值语法。我们可以在下面的代码中使用它:

```
const [a, b] = [1, 2];
```

对于数组，JavaScript 解释器会将变量的位置与变量名称所在位置的数组条目相匹配。因此，右边的第一个数组条目将被赋予左边的第一个变量名，右边的第二个数组条目将被赋予左边的第二个变量名，依此类推。我们还可以使用它来交换变量值，如下面的代码所示:

```
let a = 1,
  b = 2;
[a, b] = [b, a];
```

如果我们在析构赋值后对`a`和`b`运行`console.log`，我们得到`a`是 2 而`b`是 1。这非常方便，因为我们不必将变量赋给临时变量来交换变量的值。

当我们在析构语法中使用变量时，我们也可以给数组中的变量赋值默认值，这样我们就不必担心在用析构语法给变量赋值后变量会变成`undefined`了。例如，我们可以写:

```
let a,b;
([a=1,b=2] = [0])
```

这是一个有效的语法。在上面的代码中，我们得到`a`是 0，因为我们给它赋值了 0。`b`是 2，因为我们没有给它赋值。

# 将多个对象合并成一个

使用 spread 操作符，我们可以用它将多个对象合并成一个。在使用 spread 操作符之前，我们必须遍历每个对象的键，然后用我们自己的代码手动将每个对象的键值对放入一个新对象中，并且我们必须对我们想要合并在一起的所有对象都这样做。

这真的很痛苦。但是现在，有了 spread 操作符语法，我们可以在新对象的每个对象中应用 spread 操作符，然后我们得到一个包含新对象所有键的新对象。例如，如果我们有这些对象:

```
const obj1 = {
  a: 1,
  b: 2
};
const obj2 = {
  c: 3,
  d: 4
};
const obj3 = {
  e: 5,
  f: 6
};
const obj4 = {
  g: 7,
  h: 8
};
const obj5 = {
  i: 9,
  j: 10
};
```

然后，我们可以使用 spread 运算符将它们合并在一起，如下面的代码所示:

```
const obj1 = {
  a: 1,
  b: 2
};
const obj2 = {
  c: 3,
  d: 4
};
const obj3 = {
  e: 5,
  f: 6
};
const obj4 = {
  g: 7,
  h: 8
};
const obj5 = {
  i: 9,
  j: 10
};
const mergedObj = {
  ...obj1,
  ...obj2,
  ...obj3,
  ...obj4,
  ...obj5
};
```

然后，当我们记录`mergedObj`的值时，我们得到:

```
{
  "a": 1,
  "b": 2,
  "c": 3,
  "d": 4,
  "e": 5,
  "f": 6,
  "g": 7,
  "h": 8,
  "i": 9,
  "j": 10
}
```

如果我们的对象有一些或所有的键彼此相同，那么后来合并的重叠键的值将会覆盖先前合并的值。例如，如果我们有:

```
const obj1 = {
  a: 1,
  b: 2
};
const obj2 = {
  a: 3,
  d: 4
};
const obj3 = {
  a: 5,
  f: 6
};
const obj4 = {
  g: 7,
  h: 8
};
const obj5 = {
  i: 9,
  j: 10
};
const mergedObj = {
  ...obj1,
  ...obj2,
  ...obj3,
  ...obj4,
  ...obj5
};
```

然后，当我们记录`mergedObj`的值时，我们得到:

```
{
  "a": 5,
  "b": 2,
  "d": 4,
  "f": 6,
  "g": 7,
  "h": 8,
  "i": 9,
  "j": 10
}
```

正如我们所看到的，属性`a`的值是 5。这是因为我们首先在`obj1`中合并`a`的值为 1，然后在`obj2`中合并`a`的值为 3，覆盖了原来的值 1，然后在`obj3`中合并`a`的值为 5，覆盖了之前合并的值 3。因此，我们得到`a`的最终值 5。

![](img/62f25f76257c580b7a0047d9f6d7b50c.png)

Photo by [Jacob Hawk](https://unsplash.com/@jhawk00?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 操纵 URL

使用 URL 对象，我们可以传入一个 URL 字符串，提取并设置 URL 的各个部分，然后获得一个新的 URL。我们可以使用构造函数创建一个 URL 对象。

构造函数最多接受 2 个参数。要么我们有一个参数作为完整的 URL 字符串，要么我们可以传入一个相对 URL 字符串，它是完整 URL 的一部分，作为第一个参数，完整 URL 字符串的第一部分，或者主机名，作为第二个参数。例如，我们可以写:

```
new URL('http://medium.com');
```

或者

```
new URL('/@hohanga', 'http://medium.com');
```

使用 URL 对象，我们可以获取和设置各种属性来获取 URL 的一部分，还可以设置 URL 的一部分来创建新的 URL。利用`hash`属性，我们可以设置 URL 的 hash 部分，也就是 URL 在井号(`#`)之后的部分。例如，我们可以编写类似下面的代码:

```
const url = new URL('[http://example.com/#hash'](http://example.com/#hash'));
console.log(url.hash);
url.hash = 'newHash';
console.log(url.toString());
```

如果我们运行代码，我们可以看到第一个`console.log`语句记录了`'#hash'`。然后我们将值`'newHash'`赋给`url`的`hash`属性。然后，当我们对`url`对象运行`toString()`方法，并对`toString()`返回的值运行`console.log`方法时，我们得到的`'[http://example.com/#newHash](http://example.com/#newHash)'`是带有新散列的 URL 的新值。

同样，我们可以通过设置`host`属性来更改主机名，主机名是 URL 的第一部分。像`hash`属性一样，`host`属性也有一个获取 URL 主机名的 getter 函数。例如，我们可以编写类似下面的代码:

```
const url = new URL('[http://example.com/#hash'](http://example.com/#hash'));
console.log(url.host);
url.host = 'newExample.com';
console.log(url.toString());
```

如果我们运行代码，我们可以看到第一个`console.log`语句记录了`'#hash'`。然后我们将值`'example.com'`赋给`url`的`hash`属性。然后，当我们对`url`对象运行`toString()`方法，并对`toString()`返回的值运行`console.log`方法时，我们会得到`‘[http://newexample.com/#hash](http://newexample.com/#hash)’`，这是带有新散列的 URL 的新值。

URL 对象中有更多属性。请继续关注下一部分，我们将探索 URL 对象的更多部分。

JavaScript 和其他编程语言一样，有许多方便的技巧，让我们更容易编写程序。在本文中，我们研究了如何将对象属性和数组元素分解成单独的变量，将多个对象合并成一个，并使用`URL`对象操作 URL。有了这些技巧，我们减少了编写代码的工作量，使我们的生活更加轻松。