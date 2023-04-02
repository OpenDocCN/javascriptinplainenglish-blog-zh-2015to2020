# JavaScript 模块—导出

> 原文：<https://javascript.plainenglish.io/javascript-modules-exports-64a512b078d4?source=collection_archive---------6----------------------->

![](img/a86996ad5c82bc2b7afb1c6af429504c.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 模块允许我们将代码分成小块。它们还允许我们保留一些私有代码，同时公开可以导入到另一个模块中的其他代码。

在本文中，我们看看如何以各种方式导出模块。

# 名为 Exports 的行内 vs 子句样式

向 JavaScript 模块添加命名导出有两种方法。例如，我们可以编写以下内容:

```
export const foo = 1;
export const bar = 2;
export const baz = () => {};
```

或者我们可以这样写:

```
const foo = 1;
const bar = 2;
const baz = () => {};
export { foo, bar, baz };
```

我们也可以用不同的名字导出成员，关键字 a 后面跟有`as`:

```
const foo = 1;
const bar = 2;
const baz = () => {};
export { foo, bar, baz as qux };
```

# 重新导出成员

我们可以导出从其他模块导入的成员。例如，我们可以这样做:

`bar.js`

```
const foo = 1;
const bar = 2;
const baz = () => {};
export { foo, bar, baz as qux };
```

`foo.js`:

```
export * from "./bar";
```

`index.js`:

```
import { foo } from "./foo";
```

在上面的代码中，我们从`foo.js`的`bar.js`模块中重新导出了所有成员。然后我们从`foo.js`导入`foo`而不是`bar.js`。

我们不必导出所有成员。例如，我们可以从`bar.js`中导出选定的成员，如下所示:

`bar.js`

```
const foo = 1;
const bar = 2;
const baz = () => {};
export { foo, bar, baz };
```

`foo.js`:

```
export { foo, bar } from "./bar";
```

`index.js`:

```
import { foo } from "./foo";
```

# 使重新导出成为默认导出

我们可以将重新导出标记为默认。例如，我们可以将`foo.js`中的重新导出更改为默认导出，如下所示:

`bar.js`

```
const foo = 1;
const bar = 2;
const baz = () => {};
export { foo, bar, baz };
```

`foo.js`:

```
export { foo as default } from "./bar";
```

`index.js`:

```
import foo from "./foo";
```

在上面的代码中，我们从`foo.js`中的`bar`导出`foo`作为默认导出，因此我们可以将其作为默认导入导入到`index.js`中。

我们还可以导出默认导出，如下所示:

`bar.js`:

```
let foo = 1;
export default foo;
```

`foo.js`:

```
export { default } from "./bar";
```

`index.js`:

```
import foo from "./foo";
```

那么`index.js`中的`foo`就是 1。

# 在一个模块中混合命名导出和默认导出

我们可以在一个模块中同时拥有命名导出和默认导出。例如，我们可以编写以下代码来实现这一点:

`bar.js`

```
export const bar = 3;
export const baz = "abc";
let foo = 1;
export default foo;
```

`index.js`:

```
import foo, { bar, baz } from "./bar";
```

在上面的代码中，`bar.js`既有命名导出，也有默认导出。然后我们可以像我们在`index.js`中期望的那样导入每个成员。

![](img/c7c47dc7120876f0b7a4c45b985ff1ec.png)

Photo by [Lutz Baumann](https://unsplash.com/@luba1304?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 默认导出只是另一个命名导出

默认导出只是另一个命名导出，只不过我们只能拥有其中的一个。例如，给定以下模块:

`bar.js`:

```
let foo = 1;
export default foo;
```

然后我们可以用`default as`关键字导入它，如下所示:

`index.js`:

```
import { default as foo } from "./bar";
```

现在`index.js`有了一个指定的默认导出。

我们还可以将命名导出转换为默认导出，如下所示:

`bar.js`:

```
let foo = 1;
export { foo as default };
```

然后我们可以按如下方式导入:

```
import foo from "./bar";
```

# 使用导入加载模块

我们可以使用 Webpack 模块包附带的`import`函数来动态导入模块。

例如，我们可以编写以下代码来实现这一点:

`bar.js`:

```
let foo = 1;
export { foo as default };
```

`index.js`:

```
(async () => {
  const foo = await import("./bar");
  console.log(foo.default);
})();
```

然后我们从`index.js`中的`bar.js`导入一个默认导出。这是一个基于承诺的 API，所以我们使用`async`和`await`来导入它。

然后我们用导入模块的`default`属性访问默认导出。

对于命名导出，我们可以编写以下代码来导入成员。给定以下模块:

`bar.js`:

```
export let foo = 1;
```

我们可以在`index.js`中编写以下内容来导入它:

```
(async () => {
  const foo = await import("./bar");
  console.log(foo.foo);
})();
```

正如我们所看到的，命名导出是通过它们的属性名来访问的。所以`foo.foo`是 1。

# 结论

在 JavaScript 模块中有很多方法可以导出成员。我们可以有命名的和默认的导出。此外，我们可以从另一个模块中重新导出成员。最后，我们可以使用`import`函数来动态导入模块。它回报一个承诺。

## JavaScript 用简单的英语写的一个注释:

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**