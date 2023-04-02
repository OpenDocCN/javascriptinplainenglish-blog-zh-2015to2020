# JavaScript 原语值和对象有什么区别？

> 原文：<https://javascript.plainenglish.io/whats-the-difference-between-javascript-primitive-values-and-objects-d361ccd2bd67?source=collection_archive---------9----------------------->

![](img/244d307b85525c4e4a3911c2947479af.png)

Photo by [Lina Trochez](https://unsplash.com/@lmtrochezz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，有两种类型的数据——原始值和对象。了解它们之间的区别是很重要的，这样我们才能正确地处理它们。

在本文中，我们将看看 JavaScript 原始值和对象之间的区别。

# 有什么区别？

在 JavaScript 中，原始值是类型为`undefined`、`null`、`boolean`、`number`、`string`和`symbol`的实体。

其他的都是对象。

## 传递和分配

原始值被视为与对象相同。然而，它们之间也有一些不同之处。

原始值通过值传递。它们被赋值给变量，并通过复制它们的值，然后执行这些操作，传递给函数。

## 比较

当它们被比较时，它们是通过价值来比较的。也就是说，比较它们的内容。

对象由不同于原始值和作为值的函数的多个属性组成。

它们是通过引用传递的。所以在它们被传入之前不会复制一份。

当它们被比较时，它们的引用被比较。这意味着即使它们具有相同的内容，但被赋予不同的变量或常数，它们也不会被视为相等。

例如，如果我们有:

```
const foo = {
  a: 1
};
const bar = {
  a: 1
};
console.log(foo === bar);
```

我们记录了`false`,因为它们在内存中没有引用相同的对象，即使它们有相同的内容。

另一方面:

```
const foo = {
  a: 1
};
const bar = {
  a: 1
};
console.log(foo === foo);
```

上面的`console.log`记录了`true`，因为`foo`具有与自身相同的引用。

同样，如果我们将`foo`分配给`bar`，如下所示:

```
let foo = {
  a: 1
};
let bar = foo;
console.log(foo === bar);
```

我们也从`console.log`得到`true`。

## 易变性

原始值是不可变的。我们不能做任何改变他们价值观的操作。

例如，在严格模式下，如果我们试图设置一个字符串的`length`属性，我们会得到一个错误:

```
'use strict';
let str = 'foo';
str.length = 1;
```

但是，这可以通过数组来实现:

```
'use strict';
let arr = [1, 2, 3];
arr.length = 1;
```

这使我们将`arr`数组截断为`[1]`。

这是因为对象，也就是数组，是可变的。默认情况下，我们可以更改它们的属性。

然而，我们可以通过将单个属性描述符设置为不可写，或者使用`Object.freeze`方法来防止在对象及其原型的顶层对所有属性描述符进行更改，从而将它们的属性或整个对象设置为不可变的。

例如，我们可以如下使用`Object.freeze`:

```
let obj = {
  a: 1
}
Object.freeze(obj);
```

当我们处于严格模式时，下面的赋值会给我们一个错误:

```
obj.a = 2;
```

我们得到错误“未捕获的类型错误:无法分配给对象“# ”的只读属性“a”

同样，我们可以使用`defineProperty`来添加一个不可写的属性，如下所示:

```
'use strict';
let obj = {};
Object.defineProperty(obj, 'a', {
  value: 1,
  writable: false
})
obj.a = 2;
```

然后我们得到和以前一样的错误，因为我们使属性`a`不可写。

![](img/b3e57e86433c63fbabad0af7ba4d992c.png)

Photo by [Helena Lopes](https://unsplash.com/@wildlittlethingsphoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 从构造函数中创建的任何东西都是对象

每当我们使用`new`关键字创建一个实体时，我们就在创建一个对象。这也适用于`Boolean`、`String`和`Number`构造器。

例如，如果我们写:

```
let foo = new Boolean(1);
```

`foo`是一个对象。

这同样适用于`new String()`和`new Number()`。

要将其转换回原始值，我们必须使用`valueOf`方法将其转换回原始值。

例如，如果我们有以下代码:

```
let foo = new Boolean(1);
console.log(typeof foo);
foo = foo.valueOf();
console.log(typeof foo);
```

我们从第一个`console.log`和第二个`'boolean'`获取`'object'`日志。

使用构造函数创建原语没有任何好处，而且会增加额外的混乱。所以我们应该只使用文字，或者当我们从一种类型的值转换到另一种类型的值时，我们使用工厂函数`Boolean`、`String`和`Number`。

例如，我们可以如下使用`Number`将字符串转换成数字:

```
let foo = Number('1');
console.log(typeof foo);
```

然后我们得到从`console.log`输出的`'number'`。

# 结论

了解 JavaScript 中原始值和对象之间的区别很重要。

他们非常不同。原始值是不可变的，它们通过引用传递到函数中，通过它们的值进行比较，当我们给它们赋值时，会产生一个副本。

另一方面，对象是可变的，通过引用传递，通过引用进行比较，当我们把它们赋给另一个变量时，并没有复制。

为了减少混淆，我们应该使用文字和工厂函数来创建原始值，因为构造函数总是创建对象。