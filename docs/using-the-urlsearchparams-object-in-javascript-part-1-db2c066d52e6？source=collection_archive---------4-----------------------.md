# 在 JavaScript 中使用 URLSearchParams 对象—第 1 部分

> 原文：<https://javascript.plainenglish.io/using-the-urlsearchparams-object-in-javascript-part-1-db2c066d52e6?source=collection_archive---------4----------------------->

![](img/421ac34d08a47a9758aef763719debb2.png)

Photo by [Christian Wiediger](https://unsplash.com/@christianw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果我们不得不从头开始编写代码来解析和修改查询字符串，那么处理查询字符串可能会很痛苦。幸运的是，大多数最新的浏览器都有`URLSearchParams` 对象，可以让我们轻松处理查询字符串。有了它，解析和操作查询字符串就变得容易了。我们不再需要第三方库或从头开始编写代码来处理查询字符串。在本指南的第 1 部分，我们将看看如何创建`URLSearchParams`对象，并看看一些方便的方法，让我们获得和设置查询字符串的键值对。

# 创建 URLSearchParam 对象

我们可以用`URLSearchParams`构造函数创建一个 URLSearchParams 对象。`URLSearchParams`构造函数接受一个可选参数，这是一个包含查询字符串的`USVString` 参数。`USVString`对象对应于 Unicode 标量值的所有可能序列的集合。在我们的代码中，我们可以像对待常规字符串一样对待它们。它可以是一系列的`USVString`或包含`USVString`的记录。在我们的代码中，我们不必关心`USVString` s。实际上，它们被视为字符串。我们可以像下面的代码一样构造一个`URLSearchParams`对象:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
console.log(params.get('key1'));
console.log(params.get('key2'));
```

使用上面的代码，我们创建了一个`URL` 对象，然后我们用`URL` 对象的`search`属性获取查询字符串，然后我们将它传递给`URLSearchParams`构造函数。然后我们可以使用`URLSearchParams`对象，通过使用`URLSearchParams` 对象的`get`方法来获取键值。上面的`console.log`语句应该分别得到值 1 和 2。

我们还可以使用一个数组构造一个`URLSearchParams`对象，该数组的条目是以键作为第一个元素，以值作为第二个元素的数组。例如，我们可以用如下代码中的数组构造一个`URLSearchParams`对象:

```
const params = new URLSearchParams([
  ["key1", 1],
  ["key2", 2]
]);
console.log(params.get('key1'));
console.log(params.get('key2'));
console.log(params.toString());
```

使用上面的代码，我们传入一个数组，数组中的键和值都在条目中。使用`toString()`实例方法，我们可以很容易地获得完整的查询字符串。上面的`console.log`语句应该通过前两个语句分别得到值 1 和 2，最后一个语句应该得到值`key1=1&key2=2`。使用该对象创建查询字符串从未如此简单。

构造`URLSearchParams` 对象的第三种方法是将普通的 JavaScript 对象传递给构造函数。键在对象的键中，值在对象的值中。例如，我们可以像下面的代码一样构造一个`URLSearchParams`对象:

```
const params = new URLSearchParams({key1: 1, key2: 2});
console.log(params.get('key1'));
console.log(params.get('key2'));
console.log(params.toString());
```

上面的`console.log`语句应该通过前两个语句分别得到值 1 和 2，最后一个语句应该得到值`key1=1&key2=2`。使用该对象创建查询字符串从未如此简单。

我们还可以将映射传入构造函数，并获得一个查询字符串。对于地图，我们可以像下面的代码一样传递它:

```
const params = new URLSearchParams(new Map([['key1', 1], ['key2', 2]]));
console.log(params.get('key1'));
console.log(params.get('key2'));
console.log(params.toString());
```

使用上面的`console.log`语句，我们应该得到与前面的例子相同的输出。

在构造了一个`URLSearchParams`对象后，我们可以遍历它，因为它是一个类似数组的对象。类似数组的对象是包含迭代器方法的对象。该方法用`Symbol.iterator`符号标识。我们可以用`for...of`循环遍历像 objects 这样的数组条目。例如，如果我们有下面的`URLSearchParams`对象:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
for (const [key, value] of params) {
  console.log(key, value);
}
```

然后我们得到以下`console.log`输出:

```
key1 1
key2 2
```

正如我们所看到的，我们用`for...of`循环获得了所有的查询字符串键和值。这是使用`URLSearchParams` 对象解析查询字符串的另一个原因。它是一个 iterable 对象，让我们可以很容易地获得键和值。如果我们有一个很长的查询字符串键值对列表，这个特性就更有用了。或者，我们可以使用`entries`方法，这是一个`URLSearchParams`对象的实例方法。获取查询字符串的键值对，就像我们在下面的代码中所做的那样:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
for (const [key, value] of params.entries()) {
  console.log(key, value);
}
```

我们应该得到与第一个例子相同的输出，我们直接用`for...of`循环遍历`params`对象。

![](img/209ca516b74658e781a67aa24209c898.png)

Photo by [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 方法

每个`URLSearchParams`实例都有多种方法来获取键和值，并通过添加、更改和删除键或值来操作它们。使用这些方法，我们可以轻松地构造查询字符串，而不必直接进行字符串操作。如果我们想在操作后得到完整的查询字符串，我们可以用`toString()`方法得到它。

## 附加

`append`方法让我们向`URLSearchParams`对象添加一个键值对作为新的搜索参数。该方法有两个参数。一个用于键名，一个用于值。当我们传入两个参数时，它们都将被转换成字符串。我们可以多次追加相同的键，可以有也可以没有相同的对应值。例如，我们可以在下面的代码中使用`append`方法:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 1);
params.append('key1', 2);
console.log(params.toString());
```

使用上面的代码，我们应该从上面代码中的`console.log`语句中返回`key1=1&key2=2&key1=1&key1=2`。正如我们所看到的，我们可以任意多次追加相同的键-值对，并且`append`方法不会试图将它们合并在一起。

删除

`URLSearchParams`对象的`delete`方法将让我们删除带有给定键的键-值对。如果有多个相同的密钥，那么它们都会被删除。If 接受一个参数，即字符串形式的键名。例如，我们可以编写以下代码:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key1', 1);
params.append('key1', 2);
params.delete('key1');
console.log(params.toString());
```

如果我们像上面的最后一段代码那样运行上面的`console.log`，我们将返回`key2=2`。这意味着键名为`key1`的所有键值对都从`URLSearchParams`对象和相应的查询字符串中删除了。

## 进入

`entries`方法从`URLSearchParams`对象获取键值对。它不带参数，返回一个迭代器，让我们遍历键值对。键-值对将采用数组的形式，键作为第一个元素，对应的值作为第二个元素。例如，我们可以在下面的代码中使用`entries`方法:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
for (const [key, value] of params.entries()) {
  console.log(key, value);
}
```

在上面的代码中，我们使用了析构赋值语法将键值数组分解成它们自己的变量。这是一种访问键和值的好方法，因为我们不必使用索引或者用自己的代码将它们赋给变量。

## 为每一个

`forEach`方法让我们直接迭代这些值，而不使用`entries`方法。它采用一个回调函数，其中 can 可以访问每个`URLSearchParams`条目的键和值。回调函数将值作为第一个参数，将键作为第二个参数。例如，如果我们有以下代码:

```
const url = new URL('[https://example.com?key1=1&key2=2'](https://example.com?key1=1&key2=2'));
const params = new URLSearchParams(url.search);
params.append('key3', 3);
params.append('key4', 4);
params.append('key5', 5);
params.forEach((value, key) => {
  console.log(key, value);
})
```

当我们运行上面的代码时，我们应该从`console.log`语句中获得以下输出:

```
key1 1
key2 2
key3 3
key4 4
key5 5
```

重要的是要注意 value 参数在 key 之前，所以我们不像在大多数其他地方那样颠倒它们。

在`URLSearchParams` 对象中有更多的方法，我们将继续方法列表，并在本指南的第 2 部分中查看使用`URLSearchParams`对象的注意事项。

大多数最新的浏览器都有`URLSearchParams` 对象，可以让我们轻松处理查询字符串。现在，处理查询字符串不再是一件痛苦的事情，因为我们不必从头开始编写代码来解析和修改它。有了`URLSearchParams`对象，解析和操作查询字符串就变得容易了。我们不再需要第三方库或从头开始编写代码来处理查询字符串。在`URLSearchParams` 对象中有更多的方法和一些使用时的注意事项，所以请继续关注第 2 部分。