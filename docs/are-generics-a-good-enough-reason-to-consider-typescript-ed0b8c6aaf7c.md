# 泛型是使用 TypeScript 的足够好的理由吗？

> 原文：<https://javascript.plainenglish.io/are-generics-a-good-enough-reason-to-consider-typescript-ed0b8c6aaf7c?source=collection_archive---------7----------------------->

JavaScript 是一种动态类型的语言，这很棒。但是，当您作为一个团队与许多开发人员一起工作时，这确实会增加一些混乱。接口、类型和泛型有助于开发可重用、类型安全和无错误的代码。

TypeScript 泛型确实有助于提高代码的可读性和可用性。如果你看到的 TypeScript 代码变得如此复杂，以至于到处都可以看到“任何”类型，通常可以用泛型来解决。

![](img/219d518f0bf84f1a50608c172a16231e.png)

Logo Provided by TypeScript.org Branding

# 什么是通用？

泛型是一个用来标识方法签名的术语，它支持许多不同的类型。你们可能都用过的最常见的通用名是:`Array`。例如，你可以让你的数据类型成为一个任意:`Array<any>`的数组或者一个数字:`Array<number>`的数组，你明白了。注意这相当于`number[]`。

所以这里的“泛型”部分就是数据类型所在的地方。数组被定义为`Array<T>`，其中“T”(可以是`<` `>`之间的任何字符)是由泛型类型的使用者定义的。

# 我怎样才能把它合并到我的代码中呢？

假设你创建了一个方法，你可以传入这些类型中的任何一个:一个字符串，一个数字或字符串的数组，一个自定义对象的数组，比如一个用户。现在，你怎么打这个？你可能会说:

```
function myFunction(arg: string | number[] | string[] | User[]);
```

看起来真的很乱。在某种程度上，您的方法签名会变得如此之大，以至于您最终会做下面的事情，并破坏您从使用 TypeScript 中获得的任何好处。

```
function myFunction(arg: any[]);
```

我们可以在这里使用通用的吗？绝对的！让我们给我们的方法一些意义。这个方法的目的是从传入的列表中返回一个随机元素。我们知道这不是任何数组，而是一个泛型，它将从数组中返回一个元素。我们可以这样定义:

```
function getRandomElement<T>(list: Array<T>): T;
```

我们可以将这种方法用于上面提到的任何类型。值得注意的是，类型既可以由开发人员显式定义，也可以在 TypeScript 中推断。在第二个例子中，`el`被推断为`string`类型，因为我们传入了一个字符串数组。

```
const n: number = getRandomElement<number>([4, 23, 42, 54]);const el = getRandomElement(["hello", "world", "medium"]);
```

## 泛型的默认值

通常，如果没有提供，泛型的默认值最终是“any”。但是，如果愿意，您可以提供默认值:

```
function getRandomElement<**T = number**>(list: T[]): T;
```

## 扩展泛型

如果不是返回一个随机元素，而是在函数中返回一个用户的年龄，会怎么样？你会想保护自己不受年龄的影响，对吗？这就是你的泛型需要扩展 T4 的地方。让我们在实践中看到这一点，如果您有一个用户、管理员和人员类型会怎么样:

```
interface User { 
  name: string;
  age: number;
  data: WebData;
}
interface Person { 
  name: string; 
  age: number;
  data: PersonalData;
}
interface Admin {
  name: string;
  organization: string
}function getRandomAge(arr: User[] | Person[]): number;
```

我们可以看到，只有用户和个人拥有“年龄”属性，而管理员没有。在上面的例子中，我们可以使用一个通用的函数，而不是专门键入您的函数“User”或“Person ”:

```
function getRandomAge<**T extends {age: number}**>(arr: T[]): number;
```

我们知道这个函数仍然会返回一个数字，但是我们保证传递给`getRandomAge`的对象至少有一个`age`属性，它的类型是 number。如果我们在函数中传递一个“Admin ”, TypeScript 不会允许:

```
Type 'Admin' is missing the following properties from type '{ age: number; }[]
```

## 我们还能提高吗？

在上一节中，我们创建了一个人和一个用户，两者都有相同的三个属性:姓名、年龄和数据。唯一的区别是 Person 有一个“PersonalData”类型的数据，而 User 有一个“WebData”类型的数据。我们能重新组织一下吗？

让我们尝试使用泛型来解决这个问题。我们知道所有 3 个属性名称都是相同的，所以让我们用一个通用类型创建一个接口。

```
interface GenericPerson<T> {
  name: string;
  age: number
  data: T;
}
```

这意味着，必须用另一种类型创建 GenericPerson，这种类型是为对象的“数据”属性保留的。现在，我们可以使用这个 GenericPerson 创建我们的 Person 和 User 类型。我们将定义一个“类型”而不是一个接口。

```
type Person = GenericPerson<PersonalData>;
type User = GenericPerson<WebData>;
```

上述人员和用户类型的工作方式与我们在上一节中的界面相同。唯一的区别是，我们减少了代码的重复，并抽象出了通用接口中的一些逻辑。

# 概括起来

在这种情况下，您可以清楚地看到使用泛型的好处，因为它允许您的代码是类型安全的和可重用的。使用“any”会产生潜在的 bug，这些 bug 只能在运行时被发现。TypeScript 允许您在这些错误到达任何运行环境之前就捕捉到它们，因为它们是在将代码编译成 JavaScript 之前被捕捉到的。如果您想了解更多，您可以跟随我的“构建应用程序”系列，该系列在实践中使用了 TypeScript。