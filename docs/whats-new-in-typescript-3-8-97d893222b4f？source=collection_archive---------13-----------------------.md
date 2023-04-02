# TypeScript 3.8 有什么新功能？

> 原文：<https://javascript.plainenglish.io/whats-new-in-typescript-3-8-97d893222b4f?source=collection_archive---------13----------------------->

![](img/dfa1e47428c6c82ba2628d90d3bbb462.png)

Photo by [Євгенія Височина](https://unsplash.com/@eugenivy_reserv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是一种扩展 JavaScript 的语言，使编写 JavaScript 应用程序变得更容易。

TypeScript 3.8 带来了我们都能从中受益的新特性。

在本文中，我们将研究 TypeScript 3.8 的新特性。

# 仅键入的导入和导出

我们可以在这个版本中导入数据类型。

为此，我们使用了`type`关键字。

为了导入，我们写:

```
import type { SomeThing } from "./module";
```

我们可以出口:

```
export type { SomeThing };
```

我们从类型声明中导入项，并在编译后将其移除。

这种行为是可以控制的。

我们可以在编译器选项中设置`importsNotUsedAsValues`标志。

`remove`将它们从编译后的代码中完全删除。

`preserve`将它们保留在代码中，不用它们。

`error`也在代码中保留它们，但是如果一个有值的导入只作为一个类型使用，就会给我们一个错误。

# JavaScript 私有类成员

JavaScript 正在将私有类成员作为一个特性添加到类中。

它正处于第三阶段，这意味着它接近完成。

我们用`#`操作符使一个成员私有。

例如，我们写道:

```
class Person {
  #name: string; constructor(name: string) {
    this.#name = name;
  } greet() {
    console.log(`Hello ${this.#name}`);
  }
}
```

`#name`表示`name`是私有的。

这是一个真正的私有字段，不像`private`关键字，它只用于编译时检查。

它真正的作用域是类本身。

我们不能对这些字段使用类型脚本特定的关键字，比如`public`或`private`。

如果我们有一个子类，我们不能从子类中修改父类的私有字段的值。

例如，如果我们有:

```
class C {
  #foo = 10; getFoo() {
    return this.foo;
  }
}class D extends C {
  #foo = 20; getFoo() {
    return this.foo;
  }
}
```

那么`foo`是`C`实例是 10，`D`实例中的`foo`是 20。

如果它不是私有的，那么子类将修改父类的值。

与普通 JavaScript 不同，我们必须在 TypeScript 中声明私有字段。

例如，以下内容:

```
class C {
  constructor(foo: number) {
    this.#foo = foo;
  }
}
```

会在 TypeScript 中给我们一个语法错误，但是在 JavaScript 中是有效的。

如果我们使用私有字段，那么我们只能编译到 ES2015 目标和更高版本。

# `export * as ns`语法

我们可以用`*`操作符导入一个模块的所有成员。

为了使用它，我们写:

```
import * as module from "./module.js";
```

我们还可以从中导出所有成员。

例如，我们可以写:

```
import * as module from "./module.js";
export { module };
```

ES2020 采用新的语法将两行合并为一行:

```
export * as module from "./module.js";
```

# 顶级`await`

除了在异步函数中使用之外，我们还可以在顶层使用`await`。

为了使用它，我们写:

```
const response = await fetch("...");
const greeting = await response.text();
console.log(greeting);
```

我们不再需要把它放在异步函数中。

如果构建目标是 ES2017 和更高版本，这将有效。

# `es2020`为`target`和`module`

`es2020`现在是 TypeScript 的构建目标选项。

现在像`bigint`这样的特色在`esnext`以下有稳定的目标。

# JSDoc 属性修饰符

可以将`@rs-check`修饰符放在`.js`文件的顶部来添加类型检查。

Typescript 3.8 理解一些用于类型检查的 JSDoc 标记。

我们可以放置`@public`标签来检查公共字段。

让我们检查私有字段。

`@protected`让我们检查受保护的字段。

`@readonly`让我们确保一个字段是只读的。

# Linux 上更好的目录监视

TypeScript 3.8 将会在 Linux 上的`node_modules`文件夹中进行修改。

这是因为它在监视更改之前，会等待更改频繁的文件夹稳定下来。

我们可以用`watchOptions`改变观看策略。

`usFsEvents`让我们观察本地文件事件。

`dynacPriorty`可以设置为`fallbackPolling`的选项，使编译器在文件系统大量更新时轮询文件系统。

![](img/bd56a2bfa0ccadaf1b9e5672b74ed576.png)

Photo by [Jerry Wang](https://unsplash.com/@jerry_318?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

TypeScript 为我们带来了许多方便的 ES2020 功能，如私有类字段、顶级 await 和导出模块语法。

它也比以前更好地监视文件的变化。

它可以使用 JSDoc 注释对 JavaScript 文件进行类型检查。

## **简单英语的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**