# 使用流进行 JavaScript 类型检查—更多实用程序类型

> 原文：<https://javascript.plainenglish.io/javascript-type-checking-with-flow-more-utility-types-28ab1d326772?source=collection_archive---------6----------------------->

![](img/1ac5d2f41df583685a8254e18a7a2677.png)

Photo by [Sergey Pesterev](https://unsplash.com/@sickle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Flow 是一个由脸书开发的类型检查器，用于检查 JavaScript 数据类型。它有许多内置的数据类型，我们可以用它们来注释变量和函数参数的类型。

在本文中，我们将研究 Flow 内置的更多实用程序类型。

# $NonMaybeType

`$NonMaybeType<T>`将类型`T`转换为非可能类型。这意味着我们不能将`null`或`undefined`赋给这个实用程序类型返回的类型的任何属性。

我们可以如下使用它:

```
type MaybeAge = ?number;
type Age = $NonMaybeType<MaybeAge>;
let age: Age = 1;
```

我们不能将`null`或`undefined`分配给任何类型为`Age`的对象:

```
let age2: Age = null;
```

上面的代码会给出一个错误。

# `$ObjMap<T, F>`

`$ObjMap<T, F>`返回一个通过函数类型`F`映射对象类型`T`的类型。

例如，如果我们有一个映射函数的类型:

```
type ExtractReturnType = <V>(() => V) => V;
```

然后我们有下面的运行函数的函数:

```
function run<O: Object>(o: O): $ObjMapi<O, ExtractReturnType>{
  return Object.keys(o).map(key => o[key]());
}
```

那么假设我们有以下对象:

```
const o = {
  a: () => 1,
  b: () => 'foo'
};
```

那么我们可以得到`o`对象的方法的返回类型如下:

```
(run(o).a: number);
(run(o).b: string);
```

# $ObjMapi

`$ObjMapi<T, F>`与`$ObjMap<T, F>`相似，但`F`将被对象类型`T`的元素的键和值类型调用。

例如，如果我们有:

```
type ExtractReturnType = <V>(() => V) => V;
function run<O: Object>(o: O): $ObjMapi<O, ExtractReturnType>{
  return Object.keys(o).map(key => o[key]());
}
const o = {
  a: () => 1,
  b: () => 'foo'
};
```

然后我们得到为`a`和`b`返回的以下类型:

```
(run(o).a: { k: 'a', v: number });
(run(o).b: { k: 'b', v: string });
```

在上面的代码中，`k`是`o`的键值，`v`是`o`键值的对应值。

# $TupleMap

`$TupleMap<T, F>`采用类似元组或数组的迭代类型`T`和函数类型`F`，并返回通过将迭代中的每个值映射为类型`F`的函数而获得的迭代类型。

这和 JavaScript 中在数组中调用`map`是一样的。

例如，我们可以如下使用它:

```
type ExtractReturnType = <V>(() => V) => Vfunction run<A, I: Array<() => A>>(iter: I): $TupleMap<I, ExtractReturnType> {
  return iter.map(fn => fn());
}const arr = [() => 1, () => 2];
(run(arr)[0]: number);
(run(arr)[1]: number);
```

请注意，`arr`数组中每个函数的返回类型必须相同。否则，我们会得到一个错误。

![](img/045ef44af0ecedc04e9750f124b22db4.png)

Photo by [Fezbot2000](https://unsplash.com/@fezbot2000?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `$Call<F, T...>`

`$Call<F, T…>`是一种类型，它导致调用带有 0 个或更多参数`T...`的类型为`F`的函数。这类似于在运行时调用一个函数，但返回的是类型。

例如，我们可以如下使用它:

```
type Add = (number, number) => string;
type Sum = $Call<Add, number, number>;
let x: Sum = '1';
```

在上面的代码中，假设我们有`Add`类型，这是一个接受 2 个数字并返回一个字符串的函数类型。我们用它创造了一种新的类型:

```
type Sum = $Call<Add, number, number>;
```

然后我们得到`Sum`类型是一个字符串。

我们也可以写:

```
const add = (a: number, b: number) => (a + b).toString();
type Add = (number, number) => string;
type Sum = $Call<typeof add, number, number>;
let x: Sum = '1';
```

正如我们所见，这对于在不实际调用函数的情况下获取函数的返回类型很有用。

# `Class<T>`

`Class<T>`用于将类型传入一个类。它让我们可以创建一个可以接受多种类型的泛型类。

例如，给定以下类:

```
class Foo<T>{
  foo: T;
  constructor(foo: T){
    this.foo = foo;
  } getFoo(): T {
    return this.foo;
  }
}
```

我们可以用它来创建多个类:

```
type NumFoo = Foo<number>;
type StringFoo = Foo<string>;
```

然后我们可以实例化这些类，如下所示:

```
let numFoo: NumFoo = new Foo<number>(1);
let stringFoo: StringFoo = new Foo<string>('abc');
```

# $Shape

`$Shape<T>`是包含`T`中属性子集的类型。

例如，给定`Person`类:

```
type Person = {
  name: string,
  age: number
}
```

那么我们可以如下使用`$Shape<T>`类型:

```
const age: $Shape<Person> = { age: 10 };
```

注意，我们没有在分配的对象中包含`name`属性。

`$Shape<T>`与`T`不同，它的所有字段都标记为可选。`$Shape<T>`可以铸入`T`。例如，我们之前定义的`age`常量可以转换如下:

```
(age: Person);
```

# 美元出口

`$Exports<T>`让我们从另一个文件导入类型。例如，以下内容是相同的:

```
import typeof * as T from './math';
type T = $Exports<'./math'>;
```

在 Flow 中，我们为具有方法的对象提供了特定的实用程序类型，以返回方法类型。此外，我们有一个类型，用于将具有相同返回类型的函数的可迭代对象映射到每个函数的返回类型。

此外，还有用于定义泛型类的`Class<T>`实用类型，用于获取类型`T`的属性子集作为其自身类型的`$Shape<T>`类型。

还有用于在不调用的情况下检索`F`的返回类型的`$Call<F, T,...>`。

最后，我们有从另一个文件获取类型的`$Exports<T>`类型。