# 在 JavaScript 中使用 URLSearchParams 对象—第 2 部分

> 原文：<https://javascript.plainenglish.io/using-the-urlsearchparams-object-in-javascript-part-2-b63006f605df?source=collection_archive---------7----------------------->

![](img/620cdbfc040fcf57eff012e6e6c87ad3.png)

Photo by [Rami Al-zayat](https://unsplash.com/@rami_alzayat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果我们不得不从头开始编写代码来解析和修改查询字符串，那么处理查询字符串可能会很痛苦。幸运的是，大多数最新的浏览器都有 URLSearchParams 对象，可以让我们轻松地处理查询字符串。有了它，解析和操作查询字符串就变得容易了。我们不再需要第三方库或从头开始编写代码来处理查询字符串。在本文中，我们继续本指南的第 1 部分。

我们可以用`URLSearchParams`构造函数创建一个 URLSearchParams 对象。`URLSearchParams`构造函数接受一个可选参数，这是一个包含查询字符串的`USVString` 参数。`USVString`对象对应于 Unicode 标量值的所有可能序列的集合。在我们的代码中，我们可以像对待常规字符串一样对待它们。它可以是一系列的`USVString`或包含`USVString`的记录。在我们的代码中，我们不必关心`USVString` s。实际上，它们被视为字符串。我们可以像下面的代码一样构造一个`URLSearchParams`对象:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
console.log(params.get('key1'));
console.log(params.get('key2'));
```

创建`URLSearchParams`对象还有其他方法，包括传入其他类型的对象。要查看详细信息，请参阅本指南的第 1 部分。

# 更多方法

每个`URLSearchParams`实例都有多种方法来获取键和值，并通过添加、更改和删除键或值来操作它们。使用这些方法，我们可以轻松地构造查询字符串，而不必直接进行字符串操作。如果我们想在操作后得到完整的查询字符串，我们可以用`toString()`方法得到它。

## 得到

`URLSearchParams`对象的`get`方法让我们通过给定的键获得查询字符串的值。它有一个参数，即带有键名的字符串。如果找到搜索参数，则返回值的`USVString`，否则返回`null`。例如，如果我们有下面的查询字符串和`URLSearchParams`对象，如下面的代码所示:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
console.log(params.get('key1'));
```

然后我们从上面的`console.log`语句中得到 1。如果我们有多个具有相同键的键-值对，如下例所示:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 2);
params.append('key1', 3);
console.log(params.get('key1'));
```

然后我们得到第一个值，它是从`get`方法返回的值。如果我们想获得与给定键相关的所有值，我们可以使用`getAll`方法。即使我们不使用`append` 方法向查询字符串添加更多的键值对，情况也是一样的:

```
const url = new URL('[https://example.com?key1=1&key1=2&key1=3&key2=2'](https://example.com?key1=1&key1=2&key1=3&key2=2'));
const params = new URLSearchParams(url.search);
console.log(params.get('key1'));
```

在上面的例子中，我们仍然从上面的`console.log`输出中得到 1。

如果我们试图用一个不存在的键获取一个值，那么我们得到的是返回的`null`，如下例所示:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
console.log(params.get('abc'));
```

我们从上面的`console.log`语句中得到`null`，因为我们没有带有关键字`'abc'`的搜索参数。

## getAll

使用`getAll`方法，我们可以获得所有带有相关键名的值。它接受一个参数，该参数是一个带有键名的字符串，并返回带有与键名相关联的所有值的`USVString`数组。例如，如果我们有以下代码:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 2);
params.append('key1', 3);
console.log(params.getAll('key1'));
```

然后我们从上面的`console.log`语句得到`[“1”, “2”, “3”]`。

## 有

如果具有给定键的值存在，`has`方法返回一个布尔值`true`。它有一个参数，这是一个字符串，带有我们要查找的键名。例如，我们可以在下面的代码中使用`has`方法:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 2);
params.append('key1', 3);
console.log(params.has('key1'));
console.log(params.has('abc'));
```

当我们运行上面的代码时，我们得到第一个`console.log`语句将输出`true`，而第二个语句将输出`false`。这是我们所期望的，因为我们将`‘key1’`作为一个或多个搜索参数的关键字，但是我们没有任何将`'abc'`作为关键字的搜索参数。

## 键

我们可以使用`keys`方法获得一个迭代器，让我们遍历查询字符串中的所有搜索参数键。钥匙都是`USVString` 的物件。它不带参数，返回一个迭代器，让我们遍历搜索参数键。例如，我们可以写:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);for (const key of params.keys()) {
  console.log(key);
}
```

然后，我们从上面的`console.log`语句中得出以下内容:

```
key1
key2
```

## 设置

`URLSeacrchParam`实例的`set`方法让我们用给定的搜索参数键设置值。它需要两个参数。第一个是带有搜索参数键的字符串，第二个是要设置为带有给定键的值的值。如果有几个值具有相同的键，那么它们将被删除。如果带有给定键的搜索参数不存在，那么这个方法将创建它。

例如，我们可以像下面的代码一样使用它:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 2);
params.append('key1', 3);
params.set('key1', 5);
console.log(params.getAll('key1'));
console.log(params.toString());
```

如果我们运行上面的代码，将从上面的第一个`console.log`语句中获得数组`[“5”]`。如我们所料，`key1`的所有其他值都被删除了。第二个`console.log`语句将得到`key1=5&key2=2`，它与我们从`getAll`方法中得到的相匹配。我们还可以使用它来创建新的搜索参数键值对，就像我们对以下代码所做的那样:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 2);
params.append('key1', 3);
params.set('abc', 1);
console.log(params.getAll('abc'));
console.log(params.toString());
```

在我们运行上面的代码之后，我们从第一个`console.log`语句中返回`[“1”]`，这意味着带有关键字`'abc'`和值 1 的搜索参数被创建，就像我们期望的那样。当我们使用`toString()`方法时得到的查询字符串是`key1=1&key2=2&key1=2&key1=3&abc=1`，这与我们从`getAll()`方法中得到的一致。

## 分类

我们可以用`sort()`方法按键对`URLSearchParams`对象的键值对进行排序。排序顺序由键的 Unicode 码位决定。具有相同键的键值对之间的相对顺序将被保留。这个方法不接受任何参数并返回`undefined`。例如，我们可以在下面的代码中使用它:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 2);
params.append('key1', 3);
params.set('abc', 1);
params.sort();
console.log(params.toString());
```

如果我们运行上面的代码，我们会得到:

```
abc=1&key1=1&key1=2&key1=3&key2=2
```

从`console.log`语句的输出来看，这是我们所期望的，因为`'abc'`比`'key1'`具有更低的码位，而`‘key1'`比`key2'`具有更低的码位值。

![](img/86c4b015b01f283c968053aa59c03694.png)

Photo by [Philip Estrada](https://unsplash.com/@philipestrada?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## toString

正如我们在前面的例子中看到的，`toString()`方法返回查询字符串，我们可以把它放在 URL 中。在完成对`URLSearchParams`对象的操作后，返回的查询字符串将包含结果。注意，返回的查询字符串前面不会有问号，不像从`window.location.searh`返回的那样。例如，如果我们有:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key3', 3);
params.append('key4', 4);
params.set('key5', 5);
console.log(params.toString());
```

然后我们回来了:

```
'key1=1&key2=2&key3=3&key4=4&key5=5'
```

这正是我们所期望的。我们有从 URL 对象解析的所有搜索参数，以及我们用`URLSearchParams`对象的`append()`方法添加的内容，还有我们用`set()`方法添加的内容。

## 价值观念

`values`方法返回一个迭代器，让我们遍历包含在`URLSearchParams`对象中的所有值。迭代器可以由`for...of`循环遍历，并由 spread 操作符操作。这不需要争论。例如，如果我们有以下代码:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key3', 3);
params.append('key4', 4);
params.set('key5', 5);
for (const value of params.values()) {
  console.log(value);
}
```

然后我们从循环中的`console.log`语句得到以下输出:

```
1
2
3
4
5
```

这是我们在搜索参数中设置的值。

# 使用 URLSearchParams 对象时捕获

`URLSearchParams`构造函数不能解析完整的 URL。大概是这样的:

```
const params = new URLSearchParams('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));for (const [key, value] of params.entries()) {
  console.log(key, value);
}
```

不会起作用。当我们运行上面的代码时，我们从`console.log`语句中得到以下输出:

```
[https://example.com?key1](https://example.com?key1) 1
key2 2
```

我们可以看到，第一个等号前的整个 URL 被解析为第一个搜索参数的键，这是错误的。这意味着我们必须使用 URL 对象的 search 属性来获取查询字符串并传递它，就像我们在下面的示例中所做的那样:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
for (const [key, value] of params.entries()) {
  console.log(key, value);
}
```

如果我们运行上面的代码，我们会从`console.log`语句中得到以下输出:

```
key1 1
key2 2
```

上面的输出是我们想要的，因为这些是实际的搜索参数键值对。

大多数最新的浏览器都有`URLSearchParams` 对象，可以让我们轻松处理查询字符串。现在，处理查询字符串不再是一件痛苦的事情，因为我们不必从头开始编写代码来解析和修改它。有了`URLSearchParams`对象，解析和操作查询字符串就变得容易了。我们不再需要第三方库或从头开始编写代码来处理查询字符串。唯一的问题是，我们必须向构造函数传入一个查询字符串，或者像映射和数组这样的序列，将键值对作为一个数组，key 作为第一个元素，value 作为第二个元素。关于如何构造`URLSearchParams`对象的更多细节，请参见第 1 部分。