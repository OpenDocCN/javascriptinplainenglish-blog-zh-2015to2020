# 使用 TypeScript 泛型集合和索引类型

> 原文：<https://javascript.plainenglish.io/using-typescript-generic-collections-and-index-types-48aef82c9162?source=collection_archive---------2----------------------->

![](img/c9cc0f6901d3e635838aed7bf5f7b228.png)

Photo by [Evelyn](https://unsplash.com/@evelynnnnn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将研究如何在 TypeScript 中扩展泛型集合和索引类型。

# 使用泛型集合

TypeScript 提供了各种 JavaScript 集合的通用版本。

它们采用类型参数来限制可以用它们填充的数据类型。

`Map<K, V>`是一个 JavaScript 映射，键被限制为类型`K`，值被限制为类型`V`。

`ReadonlyMap<K, V>`不能修改的地图。

`Set<T>`是值类型为`T`的集合。

`ReadonlySet<T>`是不能修改的集合。

例如，我们可以通过编写以下内容来定义地图:

```
const map: Map<string, number> = new Map([["foo", 1], ["bar", 2]]);
```

我们传入字符串和值，如果我们不这样做，将从 TypeScript 编译器得到错误。

# 通用迭代器

TypeScript 还为我们提供了迭代器的通用版本。

`Iterator<T>`是一个描述迭代器的接口，迭代器的`next`方法返回`IteratorResult<T>`对象。

`IteratorResult<T>`描述了由具有`done`和`value`属性的迭代器产生的结果。

`Iterable<T>`定义一个具有`Symbol.iterator`属性并支持迭代的对象，

`IterableIterator<T>`结合`Iterator<T>`和`Iterable<T>`接口描述具有`Symbol.iterator`属性的对象的接口，具有`next`和`result`属性。

例如，我们可以写:

```
function* gen() {
  yield 1;
  yield 2;
}const iterator: Iterator<number> = gen();
```

我们限制迭代器只能顺序返回数字。

此外，我们可以创建一个 iterable 对象，如下所示:

```
const obj = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
  }
};const iteratable: Iterable<number> = obj;
```

然后，我们可以像其他任何可迭代对象一样，将它与 spread 操作符或 for-of 循环一起使用。

# 创建可迭代类

我们可以像创建 iterable 对象一样创建一个 iterable 类。

它有`Symbol.iterator`方法，就像我们之前看到的 iterable 对象。

例如，我们可以写:

```
class GenericIterable<T> implements Iterable<T> {
  items: T[] = [];
  constructor(...items: T[]) {
    this.items = items;
  } *[Symbol.iterator]() {
    for (const i of this.items) {
      yield i;
    }
  }
}
```

我们用自己的 iterable 类实现了`Iterable<T>`接口。

只要它有`Symbo.iterator`方法并且是一个生成器函数，那么它就正确地实现了接口。

然后我们可以通过写来使用它:

```
const itr: GenericIterable<number> = new GenericIterable<number>(1, 2, 3);
```

# 索引类型

TypeScript 有索引类型，我们可以用它来限制另一个对象的键的值。

例如，我们可以写:

```
function getProp<T, K extends keyof T>(item: T, keyname: K) {
  console.log(item[keyname]);
}
```

然后`K`被限制为任何具有`T`类型和`keyof`关键字的键。

然后我们可以通过写来使用它:

```
interface Person {
  name: string;
}const person: Person = { name: "joe" };
getProp(person, "name");
```

我们有带`name`属性的`Person`接口，然后我们可以用用`Person`接口和字符串`'name'`创建的对象调用`getProp`。

如果我们将第二个参数中的字符串替换为不在接口中的任何内容，我们将得到一个错误。

我们可以添加类型参数。

例如，我们可以写:

```
getProp<Person, "name">(person, "name");
```

这和我们没有它的时候是一样的。

然而，我们可以对类型进行更多的限制。

![](img/1e14ebe9e650a5a62743b9e63c5cb1c5.png)

Photo by [Alysa Bajenaru](https://unsplash.com/@alysa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 索引存取运算符

我们可以在索引中使用`keyof`操作符。

然后，我们可以获取所有属性的类型，并将其作为一个联合放入一个类型别名中。

例如，如果我们有接口`Person`:

```
interface Person {
  name: string;
  age: number;
}
```

那么如果我们写:

```
type allTypes = Person[keyof Person];
```

那么`allTypes`就是`string | number`。

然后我们可以写:

```
const foo: allTypes = 1;
const bar: allTypes = "foo";
```

正如我们看到的，我们可以指定一个字符串或一个数字。

# 结论

泛型集合类型内置于 TypeScript 中。

有可迭代的类型，迭代器，映射，集合等等。

还有一些类型可以获取其他类型成员的键。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**