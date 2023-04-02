# JavaScript 最佳实践—类型转换和大小写

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-type-conversions-and-casing-d1233f6515d4?source=collection_archive---------7----------------------->

![](img/7550119848aa0b434de160a52c329cb5.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **简单明了的 JavaScript**

*通过* [***订阅我们的 YouTube 频道***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) ***来表达爱意吧！***

JavaScript 是一种容易学习的编程语言。编写运行并做某事的程序很容易。然而，很难解释所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将研究在 JavaScript 程序中转换类型的最佳方法。此外，我们还会查看标识符的大小写。

# 类型铸造&强制

在铸造类型时，我们应该遵守一些规则。

# 执行类型强制的最佳位置

我们应该在声明的开头做类型强制。

这样，我们就知道我们正在转换类型。

# 没有包装纸

我们永远不应该使用包装对象来转换类型。

相反，我们应该使用工厂函数。

例如，不写:

```
const totalScore = new String(score);
```

我们写道:

```
const totalScore = String(score);
```

这是因为包装对象返回的是一个对象，而不是一个原语。

这对我们没有好处。

JavaScript 使用包装对象将它们转换成对象，这样我们就可以调用它的方法。

# 使用`Number`进行数字铸造

`Number`用于将任何事物转化为数字。

例如，我们写道:

```
const val = Number(inputValue);
```

这样，我们得到一个返回的数字基元值。

这比使用构造函数或其他函数要好。

例如，写作:

```
const val = new Number(inputValue);
```

是坏的，因为它返回的是一个对象而不是一个数字。

使用`+`如下:

```
const val = +inputValue;
```

可能会使不熟悉`+`操作人员的人感到困惑。

如果我们使用`parseInt`的话，我们应该传入我们想要转换的基数。

例如，我们写道:

```
const val = parseInt(inputValue, 10);
```

我们将 10 作为第二个参数，将`inputValue`转换为十进制数/

# 不要使用布尔构造函数

像数字和字符串一样，我们永远不应该使用`Boolean`构造函数。

使用它没有好处。

相反，我们使用`Boolean`工厂功能。

例如，我们写道:

```
const hasAge = Boolean(3);
```

代替写作:

```
const hasAge = new Boolean(3);
```

# 命名规格

我们不应该使用单个字母的名字。它们从来没有那么具有描述性。

所以与其写作:

```
const q = () => {
  //...
}
```

我们写道:

```
const question = () => {
  //...
}
```

# 将 camelCase 用于标识符

我们应该使用 camelCase 来命名对象、属性和构造器实例。

例如，我们写道:

```
const bigObject = {};
```

# 将 PascalCase 用于构造函数

PascalCase 用于命名构造函数或类。

例如，我们写道:

```
class StudentUser {
  //...
}
```

或:

```
function StudentUser(name) {
  //...
}
```

# 无前导或尾随下划线

JavaScript 构造函数或类中没有私有变量，所以我们不应该用下划线来表示它们是私有的。

所以我们不应该写:

```
class StudentUser {
  constructor(name) {
    this._name = name;
  }
  //...
}
```

当它不是隐私的时候，用下划线来表示隐私是一种误导。

# 不要保存对此的引用

我们不需要保存对`this`的引用，因为我们可以使用箭头函数来避免这样做。

例如，我们可以写:

```
function bar() {
  return () => {
    console.log(this);
  };
}
```

而不是:

```
function bar() {
  const self = this;
  return function () {
    console.log(self);
  };
}
```

作为额外的好处，它也更短。

![](img/5d9150138c7607fa90416aca047b4937.png)

Photo by [Honey Fangs](https://unsplash.com/@honeyfangs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 基本文件名应该与其默认导出的名称相匹配

默认导出应该与导出文件的名称相匹配，以避免混淆。

例如，我们写道:

```
import Foo from './Foo';
```

或者:

```
import bar from './bar';
```

这样，我们就不会混淆。

同样，对于出口，我们写道:

```
class Foo{
  // ...
}
export default Foo;
```

我们使案例保持一致，以便于阅读。

# 对用作默认导出的函数使用 camelCase

因为函数应该是 camelCase，所以我们应该以同样的方式使用 name 默认导出。

所以我们写道:

```
function eatLunch() {
  // ...
}

export default eatLunch;
```

# 结论

对于函数、类和其他标识符，我们应该坚持使用标准的 JavaScript 大小写。

这也应该适用于模块中的导入和导出。

要对类型进行强制转换，我们应该使用工厂函数，如果我们试图转换为原始值，就让它返回原始值。