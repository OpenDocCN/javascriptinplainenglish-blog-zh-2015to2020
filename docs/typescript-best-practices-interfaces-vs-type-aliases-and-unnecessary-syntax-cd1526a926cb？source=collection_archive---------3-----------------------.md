# TypeScript 最佳实践—接口与类型别名和不必要的语法

> 原文：<https://javascript.plainenglish.io/typescript-best-practices-interfaces-vs-type-aliases-and-unnecessary-syntax-cd1526a926cb?source=collection_archive---------3----------------------->

![](img/eaa5bcc6ed55119b2aadab3e35692f59.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 类名大小写

TypeScript 类名应该是 PascalCase。

这也适用于接口。

所以我们写道:

```
class Foobar {}
```

并且:

```
interface FooBar {}
```

# 对文件使用 UTF-8 编码

我们应该对文件使用 UTF 8 编码。

这样，在任何地方使用该文件都不会有任何问题。

# 文件名大小写

我们应该用一致的大小写命名我们的文件。

我们可以坚持骆驼案，帕斯卡案，烤肉串案，或蛇案。

骆驼案是`fileName.ts`。

帕斯卡格是`FileName.ts`，

烤肉串案是`file-name.ts`。

蛇案是`file_name.ts`。

我们只对所有文件使用一个。

# 使用显式递增或递减运算符

我们应该使用显式的递增或递减操作符，以确保我们只分配新值，而不使用返回值。

例如，不写:

```
++i;
i++;
--j;
j--;
```

该函数返回值并执行递增或递减操作，我们编写:

```
i += 1;
i -= 1;
j += 1;
j -= 1;
```

# 连接

我们可能希望拥有以`I`开头的界面，以区别于其他实体。

例如，我们可能想写:

```
interface IFoo {
  bar: string;
}
```

# 在类型文本上使用接口

接口可以被实现、扩展和合并，所以它们比文字类型更受欢迎。

例如，不写:

```
type Alias = {
  num: number
}
```

我们写道:

```
interface Foo {
  num: number;
}
```

# 匹配默认导出名称

如果我们有一个默认导出，那么我们的默认导入应该匹配导出的名称。

这减少了混淆。

所以如果我们有:

`bar.ts`

```
export default foo;
```

然后我们写道:

```
import foo from bar;
```

# 每个链接调用换行

如果我们有一连串的方法调用，那么我们可能希望将它们放在一个新的行中，这样就不会溢出页面。

例如，不写:

```
foo.bar().baz();
```

我们写道:

```
foo
  .bar()
  .baz();
```

# 没有尖括号类型断言

我们应该将`as`用于类型断言，因为除了`.ts`文件之外，我们还可以将它用于`.tsx`文件中的类型断言。

所以与其写:

```
const foo = <Bar>bar;
```

我们写道:

```
const foo = bar as Bar;
```

# 没有布尔文字比较

我们不应该和布尔文字比较，因为它们是多余的。

例如，我们可以缩短:

```
if (x === true)
```

收件人:

```
if (x)
```

# 参数属性的使用

我们可以在类型脚本代码中包含参数属性。

他们让我们传入`constructor`参数，同时给它赋值。

而不是写:

```
class Animal {
  constructor(private numLegs: number) {}
}
```

我们可以写:

```
class Animal {
  private numLegs: number = 2;
  constructor(numLegs: number) {
    this.numLegs = numLegs;
  }
}
```

# 没有引用导入

用`import`就不要用`reference`。

例如，如果我们有:

```
<reference path="foo.bar" />
```

我们写道:

```
import { bar } from 'foo';
```

它更加标准，我们不再需要`reference`从类型定义文件中提取类型定义。

# 没有无用的回调包装

我们的代码中不应该有无用的回调包装器。

例如，不写:

```
const handleContent = (content) => console.log('do something with', content);promise.then((content) => handleContent(content))
```

我们写道:

```
const handleContent = (content) => console.log('do something with', content);promise.then(handleContent)
```

干净多了。

# 没有不必要的初始化器

我们不应该将变量设置为`undefined`。

例如，不写:

```
let x =  undefined;
```

我们写道:

```
let x;
```

# 没有对象文字键引号

如果一个对象文字有有效的标识符作为属性名，那么我们就不需要在名称两边加引号。

例如，不写:

```
const obj = { 'foo': 1 };
```

我们写道:

```
const obj = { foo: 1 };
```

![](img/167eeb81852de384042e51a4e282c53b.png)

Photo by [Bruna Branco](https://unsplash.com/@brunabranco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该有一个一致的文件名大小写。

此外，我们不应该在代码中使用不必要的语法。

导入应该用于从外部来源提取数据类型定义。

无用的回调包装器也应该被移除。

应该使用接口而不是类型别名，因为它们更通用。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**