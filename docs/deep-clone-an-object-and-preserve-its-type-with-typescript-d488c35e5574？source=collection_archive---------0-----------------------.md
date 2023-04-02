# 深度克隆一个对象并用 TypeScript 保留其类型

> 原文：<https://javascript.plainenglish.io/deep-clone-an-object-and-preserve-its-type-with-typescript-d488c35e5574?source=collection_archive---------0----------------------->

## 从浅层复制到深层克隆

![](img/e54c037b713b92da04c3455849a5a356.png)

JavaScript 世界中的一切都是对象。我们经常需要克隆一个对象。使用 TypeScript 时，可能还需要保留对象类型。

本文将探索使用 TypeScript 深度克隆对象的选项。克隆的实现不依赖于外部库。

## 浅拷贝

使用`Object.Assign`或 Spread 操作符的浅层复制将复制顶级属性。但是作为对象的属性在浅层复制后作为引用被复制，因此它在原始源和目标(复制的对象)之间被共享。

```
const objShallowCopy = Object.assign({}, Obj1);
// or
const objShallowCopy = {...Obj1};
```

上述方法不能正确地对复杂对象进行深度克隆。但是对于不需要嵌套对象属性的情况，这已经足够好了。

## 做深层拷贝最简单的方法

使用`JSON.parse`和`JSON.stringify`是深度克隆一个对象最简单的方法。使用下面的一行代码，可以对复杂对象的嵌套属性进行深度克隆。

```
const objCloneByJsonStringfy = JSON.parse(JSON.stringify(Obj1));
```

但是它确实有一些警告。

*   这个过程很慢，因为这个方法的本质涉及到 JSON 对象的序列化和反序列化。使用这种方法克隆大型对象时，性能将是一个问题。
*   不支持`date` 类型。日期将被解析为字符串，因此源对象中的日期对象将在复制后丢失。
*   它不保留对象的类型和方法。如下面的代码片段所示，`instanceof` 返回`false`，因为它在对象的原型链中找不到构造函数。复制后源对象中的函数将会丢失。

```
const objCloneByJsonStringfy = JSON.parse(JSON.stringify(obj1));
// the type of obj1 is ObjectWithName
console.log(objCloneByJsonStringfy instanceof ObjectWithName);
// the output is false
```

## 保留类型

为了解决上述问题，开发了以下递归深度克隆函数。它支持`Date` 数据类型，并在其原型链中保留原来的对象类构造函数和方法。它也是紧凑和高效的。

代码的要点如下。用源对象原型实例化一个新对象，并使用`reduce` 操作符递归复制每个属性。

```
return Array.isArray(source)
  ? source.map(item => deepCopy(item))
    : source instanceof Date
    ? new Date(source.getTime())
    : source && typeof source === 'object'
    ? Object.getOwnPropertyNames(source).reduce((o, prop) => 
      o[prop] = deepCopy(source[prop]);
      return o;
      }, Object.create(Object.getPrototypeOf(source))
      : source as T;
```

上面代码的关键是`Object.create`，它相当于下面的:

```
Object.create = function (o) {
    function F() {}
    F.prototype = o;
    return new F();
};
```

因此复制后对象将指向源对象的同一原型。

## 属性描述符

JavaScript 中的每个属性不仅有一个`value`，还有三个属性(`*configurable*`、`*enumerable*`、T5、和`*writable*`)。所有四个属性被称为[属性描述符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)。

要完成上面的深度克隆功能并使其成为原始对象的“真实”副本，应该克隆属性描述符以及属性值。我们可以使用“Object.defineProperty”来实现这一点。

这里列出了完整的功能

## 摘要

在本文中，我们将讨论`JSON.Parse/JSON.Stringify`在深度克隆对象中的用法和缺陷。提出了一种定制的解决方案来实现真正的深度克隆并保持对象的类型。

希望这篇文章能帮助你用 TypeScript 复制对象。

如果你喜欢这篇文章，你可能也会喜欢我最近的其他文章。

[](/case-study-a-practical-usage-of-typescript-discriminated-union-type-and-generics-87e75a2717f8) [## TypeScript 区分联合类型和泛型的用例

### 我非常喜欢 Typescript 的一个原因是它的类型系统，它实用且功能丰富。应用右边的类型…

javascript.plainenglish.io](/case-study-a-practical-usage-of-typescript-discriminated-union-type-and-generics-87e75a2717f8) 

编程快乐！