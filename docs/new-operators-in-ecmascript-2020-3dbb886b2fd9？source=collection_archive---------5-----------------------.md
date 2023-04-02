# ECMAScript 2020 中的新运算符

> 原文：<https://javascript.plainenglish.io/new-operators-in-ecmascript-2020-3dbb886b2fd9?source=collection_archive---------5----------------------->

![](img/2193833f8bed7e8174f6fd33a0de5f19.png)

我们已经进入 2020 年，新的 ECMAScript 已经发布了一段时间。但是这是复杂的一年，也许你还没有找到时间来看看它的两个新操作符:nullish 合并操作符和可选链接。从 3.7 版本开始，这两个版本都已经可供 TypeScript 开发人员使用，但现在也支持普通 JavaScript。尽管它们的名字可能晦涩难懂，但它们是处理`undefined`和`null`值的非常酷的快捷方式。

# 零融合算子

这个算子是`||`算子的改进。`||`在 JavaScript 中原本是逻辑 OR:

```
true || false -> true
false || false -> false
false || true -> true
true || true -> true
```

如果第一个操作数是`false`，则返回第二个。如果第一个操作数不是严格意义上的`false`，而是具有另一个“假”值，如`null`或`undefined`，情况也是如此。这就是为什么如果您试图分配的值是`null`或`undefined`时，该运算符经常被用作分配默认值的一种方式:

```
let value;
const a = value || 1; // a is 1
value = 2;
const b = value || 1; // b is 2
```

这基本上是`const b = value ? value : 1;`的快捷方式，非常方便。这种语法的问题是，如果第一个操作数为 falsy，则采用第二个操作数。`undefined`、`null`、`false`是假的，但`0`、`''`和`NaN`也是假的。这可能会被遗忘，并导致意想不到的错误。例如，在下面的情况中，即使定义了`value`，也要为`b`赋值`1`:

```
let value = 0;
const b = value || 1; // b is 1
```

这可能正是您想要做的，但也可能是一个 bug，而且经常是。这就是 nullish 合并操作符的用武之地:在我们想要指定一个缺省值而不是`null`或`undefined`但`0`、`''`和`NaN`是有效值的情况下，它被用来代替`||`操作符:

```
let value = 0;
const b = value ?? 1; // b is 1
value = '';
const c = value ?? 'test'; // c is ''
value = NaN;
const d = value ?? 1; // d is NaN
```

# 可选链接

ECMAScript 的另一个新操作符也提供了处理`null`和`undefined`值的快捷方式。在 JavaScript 中，访问一个结果为`null`或`undefined`的对象的属性会抛出一个错误。为了防止这种情况发生，当您不能确定您的对象是否已定义时，您必须执行如下操作:

```
const a = myObject ? myObject.a : 'a';
```

使用可选的链接操作符`?.`，您可以将其简化为:

```
const b = myObject?.a;
```

您可以将链接运算符与空合并运算符结合使用。在下面的例子中，如果定义了`myObject`，并且属性`a`不是`null`或`undefined`，那么`b`将等于它。否则`b`将被赋予值`1`。用两句话描述的事情可以用一行代码写出来:

```
const b = myObject?.a ?? 1;
```

您不能完全控制从 DOM 或网络上接收的值，因此处理可能的`undefined`或`null`值是 JavaScript 中的一个常见问题。这导致了相当沉重和容易出错的`if`语句，但是现在可以通过新的 ECMAScript 2020 操作符`??`和`?.`使它们更具可读性和可维护性。

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到一切的链接！