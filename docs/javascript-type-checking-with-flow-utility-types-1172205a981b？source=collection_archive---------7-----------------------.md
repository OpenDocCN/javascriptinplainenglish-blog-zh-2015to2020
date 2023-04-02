# 使用流检查 JavaScript 类型—实用程序类型

> 原文：<https://javascript.plainenglish.io/javascript-type-checking-with-flow-utility-types-1172205a981b?source=collection_archive---------7----------------------->

![](img/4ebd3bb97ff8f4414cf7c203de313b6d.png)

Photo by [The Framed Bear](https://unsplash.com/@theframedbear?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Flow 是一个由脸书开发的类型检查器，用于检查 JavaScript 数据类型。它有许多内置的数据类型，我们可以用它们来注释变量和函数参数的类型。

在本文中，我们将看看 Flow 自带的一些内置的实用程序类型。

# $Keys

`$Keys<T>`类型从一个类型中获取属性，并将其作为自己的类型。它对于创建枚举也很有用。

例如，我们可以写:

```
const fruits = {
  apple: 'Apple',
  orange: 'Orange',
  banana: 'Banana'
};type Fruit = $Keys<typeof fruits>;
let a: Fruit = 'apple';
```

上面的代码将使用`fruits`对象的键的联合定义一个新的类型`Fruit`。这意味着对于一个类型为`Fruit`的变量，我们可以给它赋值`'apple'`、`'orange'`或`'banana'`。

分配其他值会给我们带来一个错误:

```
let b: Fruit = 'grape';
```

# $Values

类型让我们获得一个对象类型的类型，并从它们的联合中创建一个类型。

例如，我们可以写:

```
type Person = {
  name: string,
  age: number,
};type PersonValues = $Values<Person>;
let a: PersonValues = 1;
let b: PersonValues = 'Joe';
```

在代码中，我们定义了带有一些属性的`Person`类型，然后`$Values<Person>`接受`name`和`age`的类型并从中创建一个联合。所以`PeronValues`的类型是`string | number`。

# $ReadOnly

`$ReadOnly<T>`获取类型`T`的只读版本。

以下内容:

```
type ReadOnlyObj = {
  +foo: string
};
```

并且:

```
type ReadOnlyObj = $ReadOnly<{
  foo: string
}>;
```

是等价的。

当我们想将一个对象类型的所有属性重定义为只读时，这很方便。

例如，我们可以编写以下代码来定义只读类型和对象:

```
type Person  = {
  name: string,
  age: number,
};type ReadOnlyPerson = $ReadOnly<Person>;let person: ReadOnlyPerson = {
  name: 'Joe',
  age: 10
}
```

然后，当我们试图在`person`对象中重新分配一个属性时，如下所示:

```
person.name = 'Bob';
```

我们会得到一个错误。

# $Exact

`$Exact<T>`让我们从一个对象类型创建一个精确的类型。

这意味着:

```
type ExactPerson = $Exact<{name: string}>;
```

与以下内容相同:

```
type ExactPerson = {| name: string |};
```

# $Diff

`$Diff<A, B>`创建一个类型，其属性在`A`中，但不在`B`中，它们都是对象类型。和`A \ B`一样，是`A`和`B`的设定区别。

例如，如果我们有两种类型:

```
type Person  = {
  name: string,
  age: number,
};type Age = { age: number };
```

然后我们可以创建一个接受`name`属性的对象类型:

```
type Name = $Diff<Person, Age>;
```

当我们创建一个类型为`Name`的对象时，我们需要`name`属性:

```
let name: Name = { name: 'Mary' };
```

如果`name`属性不包含在对象中:

```
let foo: Name = { age: 10 };
```

然后我们会得到一个错误。

# $Rest

`$Rest<A, B>`类似于`$Diff<A, B>`，但是属性检查是在运行时完成的。属性是 rest 操作符的一部分。

导致`A`的自有属性不是`B`的自有属性。在 Flow 中，精确的对象类型被视为具有自己的属性。

例如，我们可以通过运行以下命令用`$Rest<A, B>`定义一个新的对象类型:

```
type Person = { name: string, age: number };
const person: Person = {name: 'Jon', age: 42};
const {age, ...rest} = person;
(name: $Rest<Person, {|age: number|}>);
```

然后，`name`对象不能再有属性`age`，因为我们把它转换成了`$Rest<Person, {|age: number|}>`类型。`$Rest<Person, {|age: number|}>`返回从`Person`类型中移除了`age`属性的新类型。

![](img/c3f8367f1db7734a5561744d07a4fbf6.png)

Photo by [Nicolas J Leclercq](https://unsplash.com/@nicolasjleclercq?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# $PropertyType

`$PropertyType<T, k>`从类型`T`中获取键`k`的类型。键`k`是一个字符串。例如，如果我们如下创建`Person`类型:

```
type Person = { name: string, age: number };
```

然后`$PropertyType<Person, ‘name’>`将为我们获取`string`类型:

```
let foo: $PropertyType<Person, 'name'> = 'name';
```

嵌套查找也有效。例如，我们可以写:

```
type Person = { 
  name: {
    firstName: string,
    lastName: string
  }, 
  age: number 
};let foo: <$PropertyType<$PropertyType<Person, 'name'>, 'firstName'> = 'name';
```

# $ElementType

`$ElementType<T, K>`是表示数组、元组或对象类型`T`中与键名`K`匹配的每个元素的类型。

例如，如果我们有以下类型:

```
type Person = { 
  name: string, 
  age: number 
};
```

然后，我们可以用它来获取`name`属性的类型，方法是:

```
$ElementType<Person, 'name'>
```

然后我们可以用它来注释其他变量的类型:

```
let foo: $ElementType<Person, 'name'> = 'name';
```

我们可以将它与元组一起使用，如下所示:

```
type Tuple = [boolean, string];
let foo: $ElementType<Tuple, 0> = true;
```

对于动态对象类型，我们可以将键的类型直接传递给第二个参数。例如，如果我们有以下类型:

```
type DynamicObj = { [key: string]: number };
```

然后，我们可以通过编写以下内容来获得带有`$ElementType`的属性键的类型:

```
let x: $ElementType<DynamicObj, string> = 1;
```

对于数组，我们可以写如下:

```
type StrArrayObj = {
  strings: Array<string>,
};let x: $ElementType<$ElementType<StrArrayObj, 'strings'>, number> = 'abc';
```

上面的代码工作如下。`$ElementType<StrArrayObj, ‘strings’>`获取`StrArrayObj`的`strings`属性的类型。然后我们将`$ElementType`与`$ElementType<StrArrayObj, ‘strings’>`和`number`相结合，得到`strings`的数组元素的类型。

`number`用于检索`strings`数组的索引。

Flow 有许多有用的实用程序类型，这使得创建新类型更加容易。我们可以获取一个对象的键来创建一个新的联合类型。此外，我们可以获取值的类型，并根据它们创建一个新的联合类型。

此外，还有一种将所有属性都改为只读的类型。我们还可以获得对象键、元组或数组条目的类型。