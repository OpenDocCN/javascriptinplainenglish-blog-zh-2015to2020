# 您可能还没有使用的一个重要的 TypeScript 特性

> 原文：<https://javascript.plainenglish.io/an-important-typescript-feature-you-might-not-be-using-yet-e3a982eabcc7?source=collection_archive---------2----------------------->

## TypeScript 中的映射类型:理解语法并创建自己的类型

![](img/725e588529ef5beb79264a7c096acb53.png)

TypeScript 用类型扩展了 JavaScript，使您能够在错误发生之前捕捉它们。在原语和简单对象上设置类型通常很简单，但是当您需要更复杂结构的小变化时，这可能会变得很棘手。您可能需要一个只读版本，一个只有一些属性的版本，一个所有属性都可以为空的版本。

如果您手动创建这些变体中的每一个，您将会以 10 种不同的类型表示相同的结构而告终，并且可能会认为 TypeScript 实际上不值得这样做。

TypeScript 的映射类型允许您从现有类型创建新类型，从而大大减少了键入工作。映射的类型如下所示:

```
type Readonly<T> = {
  readonly [P in keyof T]: T[P]
};
```

您可以映射属性，并且可以通过指定一些规则来转换每个属性类型。要映射所有属性，可以使用索引类型查询操作符`keyof`。在类型上调用时，它创建其属性名的字符串联合。

```
interface Coordinates {
  longitude: number;
  latitude: number;
}

type Properties = keyof Coordinates;
// Properties = "longitude" | "latitude"
```

使用`in`关键字，可以迭代从`keyof`获得的属性。一旦你拥有了接口`T`的属性`P`，你就可以通过简单地调用`T[P]`来访问`P`的类型。`T[P]`是查找类型，代表`T`中属性`P`的类型。在`Coordinates`示例中，您可以通过调用将返回`number`的`Coordinates['longitude']`来获取`longitude`属性的类型。

例如，我们可以创建一个映射类型`Identity`，它创建一个与原始类型完全相同的新类型。如果你给它传递一个类型`T`，它映射`T`的属性，并为每个属性`P`设置`P`的类型为`T[P]`。

```
type Identity<T> = {
  [P in keyof T]: T[P]
};
```

您可以通过调用`Identity<Coordinates>`将其应用于`Coordinates`。它将创建一个类型为`Coordinates['longitude']`的属性`longitude`，也就是`number`，以及一个类型为`Coordinates['latitude']`的属性`latitude`，再次是`number`。

`Identity`映射类型没有实际用途，但是您可以在映射类型中的每个属性类型上应用一些转换。例如，您可以将属性设置为 readonly，正如我们在第一个示例中所做的那样:

```
type Readonly<T> = {
  readonly [P in keyof T]: T[P]
};
```

这是一个非常典型的用例，它是 TypeScript 的一部分。你可以在你的类型上使用`Readonly`，例如`Readonly<Coordinates>`，而不必重新定义它。另一个常见的应用是使字段可选:

```
type Partial<T> = {
  [P in keyof T]?: T[P]
};
```

它也是 TypeScript 实用工具类型。但是在 TypeScript 中，并不是所有的用例都被默认覆盖。例如，如果您将`tsconfig`中的`strict`属性设置为`true`，则不允许您将`null`值赋给变量，除非您将其显式键入为`null`。如果要使现有接口的属性可为空，可能需要定义自己的映射类型:

```
type Nullable<T> = {
  [P in keyof T]: T[P] | null
};
```

如果你有一个只读类型，你也可以创建一个映射类型，其中的字段是**而不是**只读的:使用`-`你可以从你的属性中删除`readonly`修饰符:

```
type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
};
```

映射类型是 TypeScript 的一项强大功能，使您能够减少复杂类型的键入工作量，同时仍然拥有强类型对象。您可以在属性类型中添加或移除修饰符，以便从原始类型创建各种版本。最常见的用例，如`Readonly`或`Partial`，是 TypeScript 语言的一部分，不需要重新定义，但是一旦你理解了语法，你就可以创建自己的映射类型来满足你的所有需求。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**