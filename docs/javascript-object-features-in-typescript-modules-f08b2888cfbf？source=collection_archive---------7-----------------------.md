# TypeScript 模块中的 JavaScript 对象特性

> 原文：<https://javascript.plainenglish.io/javascript-object-features-in-typescript-modules-f08b2888cfbf?source=collection_archive---------7----------------------->

![](img/9785e355d9cc0928619c6c93214d66f7.png)

Photo by [Gavin Allanwood](https://unsplash.com/@gavla?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将看看如何定义和使用模块。

# 使用模块

几乎所有的应用程序都不能包含在一个文件中。

因此，我们需要将代码放在模块中，这样我们就可以组织代码。

这样，我们可以将代码分成易于管理的块。

JavaScript 模块可以与 TypeScript 项目一起使用。

应该使用它们，因为它们现在是 JavaScript 中的标准。

从版本 12 开始，Node.js 也支持 JavaScript 模块，因此我们也可以在 Node 项目中使用它们，而无需添加任何 transpilation。

# 创建 JavaScript 模块

要创建一个模块，我们只需要创建一个 JavaScript 文件。

然后，如果我们想让一个成员成为可导入的，那么我们将该成员放在顶层，并使用`export`关键字将其暴露给外部。

例如，我们可以写:

```
export const name = "joe";
```

这将使我们的名称变量可以从另一个模块导入。

这种导出称为命名导出，因为我们必须在导入时显式指定成员的名称。

我们还可以进行默认导出。

为此，我们可以使用`default`关键字:

```
export default {
  name: "james"
};
```

我们只能在任何模块中进行默认导出。

当我们在另一个模块中导入它时，我们可以给它起任何名字。

# 使用 JavaScript 模块

要使用模块的导出成员，我们必须导入它。

例如，如果我们想要使用下面的导出成员:

```
export const name = "joe";
```

我们可以写:

```
import { name } from "./module";console.log(name);
```

我们使用了`import`关键字和花括号中的模块成员来导入它。

然后我们可以在模块内部的任何地方引用它。

为了导入默认的导出，我们跳过花括号。

例如，如果我们有:

```
export default {
  name: "james"
};
```

然后，我们可以通过编写以下内容来导入对象:

```
import obj from "./module";console.log(obj.name);
```

我们跳过了`import`语句中的花括号，并在导入后记录值。

`./`告诉我们，我们正在搜索相对于当前模块的路径。

所以我们应该把它包括在相对进口中。

如果我们跳过`./`，那么我们表明我们正在从一个依赖项而不是本地项目中的一个模块中导入。

模块依赖关系最常见的位置是在`node_modules`文件夹中。

![](img/f321659a45f4b619c856abc8c3797cd1.png)

Photo by [Aswathy N](https://unsplash.com/@abnair?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在模块中定义多个命名成员

我们可以在一个模块中定义多个命名成员。

例如，我们可以写:

```
export const name = "joe";
export const age = 20;
```

我们从模块中导出了成员`name`和`age`。

然后我们通过写来导入 Then:

```
import { name, age } from "./module";
console.log(name, age);
```

然后，我们通过用逗号分隔来导入它们。

使用命名成员，我们可以有选择地导入我们想要的成员。

这样，我们就不必进口我们不用的东西了。

# 结论

在 TypeScript 项目中，我们必须使用模块将它们组织成小的、可管理的块。

我们可以随心所欲地导入和导出成员。

我们不需要把所有东西都暴露在外面，也不需要进口所有东西。

## **说白了**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**