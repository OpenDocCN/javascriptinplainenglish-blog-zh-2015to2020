# TypeScript 最佳实践—函数和数组类型，降低复杂性

> 原文：<https://javascript.plainenglish.io/typescript-best-practices-function-and-array-types-reducing-complexity-a5c42ee226c?source=collection_archive---------6----------------------->

![](img/86e440ba0d7860a42694d0baba1834d8.png)

Photo by [Joshua J. Cotten](https://unsplash.com/@jcotten?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 使用 isNaN 检查 NaN

`NaN`不等于它本身，所以我们不能用`===`或`!==`来检查`NaN`。

相反，我们使用`isNaN`函数来检查它。

所以与其写:

```
if (foo === NaN) {}
```

我们写道:

```
if (isNaN(foo)) {}
```

# void 的无效使用

我们应该注意`void`的无效使用。

我们不应该把`void`和其他类型混在一起，因为`void`除了是一个显式类型的值之外什么都不是。

如果我们需要`undefined`，那么我们应该使用`undefined`型。

所以输入像这样的表达式:

```
Foo | void
```

应替换为:

```
Foo | undefined
```

# 每个文件的最大类别数

每个文件不应该有太多的类。

如果我们做的话，可能太复杂了。

我们可以用 linter 设置每个文件的最大类数。

# 最大文件行数

如果一个文件有太多的行，那么它可能太复杂和难以阅读。

我们可以用 linter 设置文件中的最大文件行数。

# 默认导出

默认导出并不清楚，所以我们应该避免使用它们。

命名出口更清晰。

所以我们可能想避开它们。

# 默认导入

像默认导出一样，默认导入不像命名导入那样清晰。

# 没有重复的导入

我们可以将来自同一个模块的多个 import 语句合并成一个`import`语句，所以我们应该这样做。

例如，不写:

```
import { foo } from 'module';
import { bar } from 'module';
```

我们写道:

```
import { foo, bar } from 'module';
```

# 没有可合并的名称空间

我们不应该有两个同名的名称空间，这样我们就不必考虑如何将它们合并在一起。

相反，我们可以用不同的名字命名不同的名称空间。

例如，不写:

```
namespace Animals {
  export class Dog {}
}namespace Animals {
  export interface Cat {
    numberOfLegs: number;
  }
  export class Zebra {}
}
```

我们写道:

```
namespace Pets {
  export class Dog {}
  export interface Cat {
    numberOfLegs: number;
  }
}name WildAnimals {
  export class Zebra {}
}
```

# 不需要进口

现在 ES 模块已经广泛可用，我们可以用`import`代替`require`。

例如，不写:

```
const { bar } = require('foo');
```

我们可以使用 ES6 模块版本的模块并编写:

```
import { bar } from 'foo';
```

# 使用常量

我们应该尽可能使用`const`而不是`let`或`var`来声明变量。

这样，它们就不会意外地被赋予另一个值。

例如，不写:

```
var x = 1;
let y = 2;
```

我们写道:

```
const z = 3;
```

# 将变量标记为只读

如果变量是私有的并且从未在构造函数之外被修改过，我们应该将它们标记为`readonly`。

这样，我们就不会不小心改变它们。

例如，不写:

```
class Foo {
  private bar = 1;
}
```

我们写道:

```
class Foo {
  private readonly bar = 1;
}
```

# 箭头返回速记

如果我们有一个箭头函数，它只有一个语句并返回一些东西，我们可以把它们变得更短。

而不是写:

```
() => { return x; }
```

我们写道:

```
() => x
```

# 设置数组类型

我们应该设置数组内容的类型，这样我们就可以限制进入数组的值的类型。

我们还可以设置泛型类型来使数组通用。

例如，不写:

```
const arr = ['foo', 'bar', 1];
```

我们写道:

```
const arr: string[] = ['foo', 'bar', 'baz'];
```

# 可调用类型

如果我们有一个只有调用签名的接口或文字类型，那么我们应该把它写成函数类型。

例如，不写:

```
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```

我们写道:

```
(source: string, subString: string) => boolean
```

![](img/57f498fa71e5cae7eab8be4114a32f7b.png)

Photo by [John Duncan](https://unsplash.com/@jrduncan11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该使用函数类型，而不是只有函数签名的接口或文字类型。

数组应该有一个类型，所以我们不能把任何类型的数据放进去。

我们应该降低每个文件的复杂性。

`isNaN`应该用于检查`NaN`。