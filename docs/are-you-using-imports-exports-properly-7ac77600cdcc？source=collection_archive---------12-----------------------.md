# 你是否正确使用进出口？

> 原文：<https://javascript.plainenglish.io/are-you-using-imports-exports-properly-7ac77600cdcc?source=collection_archive---------12----------------------->

![](img/42c13de60cb12016edbc72fc339bd937.png)

Photo by [Andy Li](https://unsplash.com/@andasta?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 模块由文件分隔并异步加载。使用关键字`export`定义导出，使用关键字`import`定义导入。

虽然导入和导出的基础知识很容易理解，但是使用 ES 模块还有许多其他方法。在本文中，我们将讨论在模块中导出和导入的所有方法。

# 出口

我们将研究三种类型的出口:

## 1.默认导出

每个模块都有一个单独的**默认**导出，它表示从该模块导出的主值。一个模块中不能有多个默认导出。

```
const foo = () => console.log('foo');export default foo;
```

默认情况下，我们也可以导出函数声明或类声明。

```
export default function foo() {
  console.log('foo');
}
```

我们还可以将值导出为默认导出。

```
export default 555;
```

## 2.命名出口

任何变量声明都可以在创建时导出，方法是在声明前添加`export`关键字。这基本上创建了一个使用变量名作为导出名的命名导出。

```
export const foo = () => console.log('foo');
```

我们还可以立即导出函数和类声明。

```
export function foo() {
 console.log('foo')
}
```

如果我们想导出一个已经定义好的变量，我们可以用花括号把变量括起来。这通常在文件末尾完成。

```
const foo = () => console.log('foo');export { foo }; 
```

为了重命名已命名的导出，使用`as`关键字。我们也可以同时导出其他变量。

```
const foo = () => console.log('foo');
const bar = 123;export { foo as printFoo, bar };
```

## 3.出口总额

有些情况下，我们必须从另一个文件导入模块，然后再导出它们。这种情况通常出现在从几个文件中导入模块，然后从一个文件中导出所有模块的地方。当您同时导入和导出大量内容时，这会变得很乏味。ES 模块允许我们同时导入和导出多个值。

```
export * from "./foo.js";
```

这将获取`./foo.js`的所有 ***名为*** 的导出，并重新导出它们。但是它不会重新导出默认导出，因为一个模块只能有一个默认导出。我们还可以专门从其他文件中导出默认模块，或者在重新导出时命名默认导出。

```
export { default } from "./foo.js";// orexport { default as foo } from "./foo.js";
```

我们还可以有选择地从另一个模块中导出不同的变量，而不是重新导出所有的变量。

```
export { foo as printFoo, bar} from "./foo.js"; 
```

最后，我们可以使用`as`关键字将整个模块包装成一个单独的命名导出。假设，考虑下面的文件。

```
// funcs.js
export function foo() {console.log('foo')}
export function bar() {console.log('bar')}
```

我们现在可以将它打包到一个导出中，这个导出是一个包含所有命名和默认导出的对象。

```
export * as funcs from "./funcs.js"; 
// { foo: function foo(), bar: function bar() }
```

# 进口

我们将研究三种类型的进口:

## 1.默认导入

当我们导入一个默认值时，我们需要给它指定一个名称。因为它是默认的，我们实际上可以给它任何你选择的名字。

```
import fooFunctions from "./foo.js";
```

我们还可以同时导入所有导出，包括命名导出和默认导出。这将把所有的导出放到一个对象中，默认的导出将被赋予属性名`default`。

```
import * as foo from "./foo.js"; 
// { default: foo }
```

## 2.命名导入

我们可以通过将导出的名称放在花括号中来导入任何命名的导出。

```
import { foo, bar } from "./foo.js";
```

我们还可以在导入时使用`as`关键字重命名导入。

```
import {foo as fooFunction, bar} from './foo.js`
```

我们还可以在同一个 import 语句中混合使用命名导出和默认导出。

```
import foo, { bar } from "./foo.js";
```

最后，我们可以导入一个模块，而不需要在文件中列出任何想要使用的导出。这被称为**副作用导入**，它将执行模块中的代码，而不向我们提供任何导出的值。

```
import "./fruitBasket.js";
```

## 3.动态导入

有时，我们在导入文件之前不知道它的名称，或者在执行代码到一半时才需要导入文件。在这些情况下，我们可以使用动态导入在代码中的任何地方导入模块。

因为 ES 模块是异步的，所以模块不会立即可用。我们必须等待它被加载，然后才能对它做任何事情。如果我们的模块找不到，动态导入将抛出一个错误。

使用动态导入时，最好使用`try` / `catch` 。

```
async function printFn() {
  try {
    const fooFn = await import('./foo.js');
  } catch {
    console.error("Error getting foo module:");
  }
  return fooFn();
}
```

# 结论

需要记住的一点是，导出和静态导入只能发生在模块的顶层。另一方面，动态导入可以在函数内部完成。

我希望这篇文章对你有所帮助。如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**