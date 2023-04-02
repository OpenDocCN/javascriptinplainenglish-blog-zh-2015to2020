# TypeScript 最佳实践—成员访问、循环和函数类型

> 原文：<https://javascript.plainenglish.io/typescript-best-practices-member-access-loops-and-function-types-36dce7173b76?source=collection_archive---------5----------------------->

![](img/741e9be150254e02768fc05a6d87fb30.png)

Photo by [Whitney Wright](https://unsplash.com/@whitney_wright?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是一个简单易学的 JavaScript 扩展。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的类型脚本代码。

在本文中，我们将研究使用 TypeScript 编写代码时应遵循的最佳实践，包括禁止成员访问任何类型化变量。

此外，我们看看为什么我们要使用`const`断言。

我们也看看为什么在导入模块时应该使用`import`而不是`require`。

我们发现如何使用接口来指定函数的类型。

我们来看看为什么 for-of 循环比普通的 for 循环更好。

# 任何类型化变量都没有成员访问权限

`any`可能会泄漏到我们嵌套实体的代码库中。

例如，我们可能有具有`any`类型的类型声明。

因此，如果我们允许访问那些嵌套成员，我们可能会在运行时遇到类型错误。

所以与其写:

```
declare const anyObj: { prop: any };
anyObj.prop.a.b;
```

我们应该写:

```
declare const anyObj: { prop: string };
anyObj.prop;
```

现在我们知道`anyObj.prop`一定是一个字符串，我们可以安全地访问它。

# 不要从函数中返回任何值

我们不应该在函数中返回任何带有`any`类型的东西。

相反，我们应该添加一个返回类型注释，这样我们就知道它返回的是什么。

例如，不写:

```
function foo() {
  return 1 as any;
}
```

或者:

```
function arr() {
  return [] as any[];
}
```

我们应该写:

```
function arr(): number[] {
  return [1, 2];
}
```

或者:

```
function foo(): number {
  return 1;
}
```

现在我们知道函数返回什么了。

# 没有未使用的变量和参数

未使用的变量和参数是无用的，所以我们可能不应该在代码中使用它们。

我们可以删除不使用的变量声明，如`var`、`const`或`let`变量。

同样，不用的函数和类也可以删除。

不使用的枚举、接口和类型声明也可以删除。

像方法、实例变量、参数这样的类成员如果不用的话都可以删除。

这也适用于 import 语句。

# 除非在 Import 语句中，否则不需要语句

我们不应该再使用`require`语句，因为 ES6 模块已经成为标准。

因此，与其写:

```
const foo = require('foo');
```

我们写道:

```
import foo = require('foo');
```

或者:

```
import foo from 'foo';
```

# 在有限类型上用作常量

`const`断言比文字类型更好，因为它们使值保持不变。

如果我们使用`const`断言，文字类型不能被加宽，对象和数组条目变成`readonly`。

例如，不写:

```
let bar: 2 = 2;
```

或者:

```
let bar = { bar: 'baz' as 'baz' };
```

我们可以写:

```
let foo = 'bar' as const;
```

或者:

```
let foo = { bar: 'baz' };
```

# 使用 for-of 循环代替 for 循环

for-of 循环让我们用一个简单的循环来遍历条目，而不是必须设置循环条件和索引变量。

它还适用于任何类型的 iterable 对象，而不是具有 index 和`length`属性的对象。

例如，不写:

```
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

我们写道:

```
for (const x of arr) {
  console.log(x);
}
```

正如我们所看到的，如果我们只想遍历一个 iterable 对象中的所有项，for-of 循环要简单得多。

![](img/9683b3f8290327f915bafe02ffcbe4e5.png)

Photo by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用函数类型而不是带有调用签名的接口

我们可以使用函数类型来代替接口或对象类型文字，只使用一个调用签名。

例如，不写:

```
function foo(bar: { (): number }): number {
  return bar();
}
```

我们写道:

```
interface Foo {
  (): void;
  bar: number;
}const foo: Foo = () => {};
foo.bar = 1;
foo();
```

我们有一个不返回任何内容的函数，它有一个数字属性。

# 结论

我们可能想在文字类型上使用`as const`来防止变量的类型变宽，并使它们成为只读的。

此外，我们想使用`import`而不是`require`来导入模块。

我们还希望使用接口而不是函数来指定函数的类型。

最后，在大多数情况下，for-of 循环比 for 循环更好。