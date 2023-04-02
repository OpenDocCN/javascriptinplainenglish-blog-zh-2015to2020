# 使用流联合类型进行 JavaScript 类型检查

> 原文：<https://javascript.plainenglish.io/javascript-type-checking-with-flow-union-types-3c8aa5d1dc05?source=collection_archive---------3----------------------->

![](img/e00e80f6ddaf2d6cddbfc3c422250c2f.png)

Photo by [Andre Hunter](https://unsplash.com/@dre0316?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Flow 是一个由脸书开发的类型检查器，用于检查 JavaScript 数据类型。它有许多内置的数据类型，我们可以用它们来注释变量和函数参数的类型。

在本文中，我们将研究如何使用联合类型来创建接受几种不同类型的变量。

# 联合类型语法

定义联合类型的一般语法如下:

```
Type1 | Type2 | ... | TypeN
```

我们用竖线`|`将每种类型分开。

同样，我们可以通过使用前导`|`将它们分成多行:

```
type Bar =
  | Type1
  | Type2
  | ...
  | TypeN
```

我们可以从其他联合类型中创建联合类型:

```
type Numbers = 1 | 2;
type Fruit = 'apple' | 'orange';type Foo = Numbers | Fruit;
```

# 功能

如果我们在函数中使用联合类型，我们必须处理参数的联合类型的每一种可能类型。例如，我们必须写出这样的内容:

```
function foo(value: number | boolean | string):  number { 
  if (typeof value === 'number') {
    return 1;
  } else if (typeof value === 'boolean') {
    return 2;
  }
  return Number(value);
}
```

如果我们像下面的代码一样跳过对参数的联合类型中任何类型的处理:

```
function foo(value: number | boolean | string):  number { 
  if (typeof value === 'number') {
    return 1;
  } else if (typeof value === 'boolean') {
    return 2;
  }  
}
```

然后我们会得到一个错误。

# 联合和细化

对于联合类型参数，单独处理每种类型是很有用的。我们可以使用`typeof`操作符来检查类型，并按如下方式处理每种类型:

```
function foo(value: number | boolean | string): number { 
  if (typeof value === 'number') {
    return 1;
  } 
  else if (typeof value === 'boolean') {
    return 2;
  } 
  else if (typeof value === 'string') {
    return Number(value);
  }
  return Number(value);
}
```

![](img/1e14edc5c31f7506edd04555bb12461d.png)

Photo by [Dominic Blignaut](https://unsplash.com/@domi_blig?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 不相交并集

我们可以用不同的对象类型创建一个联合类型。例如，如果我们有以下对象:

```
{ foo: true, bar: 'bar' }
{ foo: false, bar: 'baz' }
```

我们可以通过为这两个对象分别创建一个类型别名来创建一个联合类型，然后将它们联合起来。所以我们可以写:

```
type Foo1 = { foo: true, bar: 'bar' };
type Foo2 = { foo: false, bar: 'baz' };type Foo = Foo1 | Foo2;
```

然后我们可以创建类型为`Foo`的变量，如下所示:

```
let foo1: Foo = { foo: true, bar: 'bar' };
let foo2: Foo = { foo: false, bar: 'baz' };
```

具有固定值的两个对象类型的联合将允许我们的变量采用联合中的两个类型中的任何一个。

具有不精确属性的对象类型的联合将为属性创建两种类型的联合。

例如，如果我们有:

```
type Foo1 = { foo: true, bar: string };
type Foo2 = { foo: true, bar: number };
type Foo = Foo1 | Foo2;
```

那么`Foo`将允许`bar`为字符串或数字，而`foo`将始终为`true`。所以我们可以创建如下的变量:

```
let foo1: Foo = { foo: true, bar: 'bar' };
let foo2: Foo = { foo: true, bar: 2 };
```

如果我们有两个不同属性的类型，那么两个类型的并集让我们包含对象的所有属性，如果我们有一个两个类型并集的变量。

例如，如果我们有:

```
type Foo1 = { foo: true, bar: string };
type Foo2 = { baz: false, a: number };type Foo = Foo1 | Foo2;
```

然后，我们可以定义一个具有所有属性的变量，如下所示:

```
let foo: Foo = { foo: true, bar: 'bar', baz: false, a: 1 };
```

对于联合类型，我们还可以传入组成联合的类型中不包含的额外属性。所以考虑到:

```
type Foo1 = { foo: true, bar: string };
type Foo2 = { baz: false, a: number };type Foo = Foo1 | Foo2;
```

所以我们可以写:

```
let foo: Foo = { foo: true, bar: 'bar', baz: false, a: 1, b: 2 };
```

# 精确类型的不相交并集

我们可以通过创建精确类型的联合来限制新属性的添加，如下所示:

```
type Foo1 = {| foo: true, bar: string |};
type Foo2 = {| baz: false, a: number |};type Foo = Foo1 | Foo2;
```

然后写道:

```
let foo: Foo = { foo: true, bar: 'bar', baz: false, a: 1, b: 2 };
```

会给我们一个错误。

有了精确类型的联合，我们可以从其中一个获得属性。给定这些类型:

```
type Foo1 = {| foo: true, bar: string |};
type Foo2 = {| baz: false, a: number |};type Foo = Foo1 | Foo2;
```

我们可以选择:

```
let foo: Foo = { foo: true, bar: 'bar' };
```

或者:

```
let foo: Foo = {  baz: false, a: 1 };
```

使用联合类型，我们可以创建可以采用几种类型的值的变量。形成联合的类型可以是基元或对象类型。

我们还可以形成精确类型的联合，以防止添加未在类型中列出的附加属性或包含所有类型的属性。