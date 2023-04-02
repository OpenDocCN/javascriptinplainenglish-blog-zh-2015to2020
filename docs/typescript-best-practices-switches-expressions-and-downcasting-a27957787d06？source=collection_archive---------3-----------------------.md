# TypeScript 最佳实践—开关、表达式和向下转换

> 原文：<https://javascript.plainenglish.io/typescript-best-practices-switches-expressions-and-downcasting-a27957787d06?source=collection_archive---------3----------------------->

![](img/935b96bf45119442dd4564fb8628ed3d.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 没有字符串文字属性访问

如果属性是有效的标识符，我们就不应该使用字符串来访问它们。

例如，不写:

```
obj['prop']
```

我们写道:

```
obj.prop
```

但是我们仍然可以使用括号来访问不是有效标识符的属性。

例如，我们仍然可以写:

```
obj['foo-bar'];
```

# 不要扔线

我们不应该抛出字符串，因为我们抛出了很多信息，如堆栈跟踪和错误类型。

例如，不写:

```
throw 'error';
```

我们写道:

```
throw new Error("error");
```

# 没有开关盒脱落

我们不应该有没有`break`或`return`语句的 switch cases。

例如，不要写:

```
switch (foo) {
  case 1:
    doWork(foo);
  case 2:
    doOtherWork(foo);
}
```

我们写道:

```
switch (foo) {
  case 1:
    doWork(foo);
    break;
  case 2:
    doOtherWork(foo);
    break;
}
```

# 没有同义反复的表达

我们不应该比较一个价值本身。

他们总是跑或者不跑。

例如，不写:

```
3 === 3
```

我们写道:

```
val === 3
```

# 对此没有分配任务

我们不应该给`this`赋值。

如果我们需要这样做，我们应该使用箭头函数。

所以与其写:

```
const self = this;setTimeout(function() {
  self.work();
});
```

我们写道:

```
setTimeout(() => {
  this.work();
});
```

# 没有未绑定的方法

我们不应该在方法调用之外使用实例方法。

例如，不写:

```
class Foo {
  public log(): void {
    console.log(this);
  }
}Foo.log();
```

我们写道:

```
class Foo {
  public log(): void {
    console.log(this);
  }
}const foo = new Foo();
foo.log();
```

如果不需要引用类实例，我们也可以编写一个箭头函数并使用`bind`:

```
class Foo {
  public log = (): void => {
    console.log(foo);
  }
}const foo = new Foo();
foo.log();
```

或者:

```
class Foo {
  public log() {
    console.log(this);
  }
}const foo = new Foo();
const manualLog = foo.log.bind(foo);
manualLog.log();
```

# 没有不必要的课

我们可以把很多东西放到课外。

例如，我们可以拥有不包含在类中的顶级函数和变量。

# 没有不安全的任何铸造

我们不应该给`any`赋予任何变量或价值。

这意味着它可以包含任何内容。

如果它有动态属性，我们可以使用索引签名、联合或交集类型等等。

而不是铸造到`any`:

```
const foo = bar as any;
```

我们可以写:

```
interface Bar {
  [key: string]: string | number; 
  [index: number]: string; 
  baz: number;
}const foo: Bar = bar;
```

我们在`Bar`接口中有`baz`和其他动态属性。

# 终于没有不安全了

我们不应该在`finally`块中有`return`、`continue`、`break`或`throws`，因为它们覆盖了`try`和`catch`块的控制流语句。

例如，不写:

```
try {
  doWork();
}
catch(ex) {
  console.log(ex);
}
finally {
  return false;
}
```

我们写道:

```
try {
  doWork();
  reutrn true;
}
catch(ex) {
  console.log(ex);
  return false;
}
```

# 没有未使用的表达式

我们的代码中不应该有任何未使用的表达式语句。

例如，不写:

```
a === 1
```

我们删除它们或者在语句中使用它们。

# 在声明变量之前不要使用它们

我们不应该使用变量来声明它们。

使用`var`这是可能的，但是使用`let`和`const`会抛出错误。

而不是写:

```
console.log(x);
var x = 1;
```

我们写道:

```
var x = 1;
console.log(x);let y = 2;
console.log(y);const z = 3;
console.log(z);
```

# 没有 var 关键字

我们应该对使用关键字`var`三思。

我们不使用`var`，它有一个复杂的函数范围和提升，而是使用`let`和`const`，它们是块范围的，没有复杂的范围问题。

例如，不要写:

```
var x = 2;
```

我们写道:

```
let a = 1;
const bar = 3;
```

![](img/dcacfee64db72e173fedc5442fcc850d.png)

Photo by [Brian Yurasits](https://unsplash.com/@brian_yuri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该尽可能用点符号来访问属性。

此外，我们应该使用现代语法来声明变量。

向下转换到`any`也不好，因为我们可以给它赋值。

我们还应该在我们的`case`模块中有`break`或`return`。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**