# 如果你不关心类型，使用 TypeScript 是没有意义的

> 原文：<https://javascript.plainenglish.io/there-is-no-point-to-use-typescript-in-your-project-if-you-dont-care-about-types-68131deeb43a?source=collection_archive---------3----------------------->

![](img/b84c1bca1f6d7bb39faa8cdb0daaca9f.png)

const cat: Cat = (dog as any);

> 经过一年的 TypeScript 制作经验，我非常自信地说，它是比普通 JavaScript 更好的工具，我准备在我的下一个项目中使用它。然而，我会从一开始就禁止所有隐式和显式的“任何”类型用法和类型断言。

# 如果在代码库中允许“任何”类型，就不能信任 TpeScript 类型。

让我们从一个绝对邪恶的——**任何**类型开始。我认为默认情况下 [typescript 编译器配置有一个“*strict”*规则设置为*“false”*](https://medium.com/javascript-in-plain-english/what-are-typescript-strict-compiler-options-part-1-5822c67c1df5)*是一个巨大的错误。*因此，由于违约的力量，许多不熟悉技术的人会听之任之。但是对于一个新的项目来说，这是一个可怕的想法，因为它减少了打字稿的优势*(这实际上也发生在我们的项目中)。为了理解我的意思，你可以看看下面的例子:*

## 1.“任何”可以分配给任何类型。

```
**const** b: **any** = 4;
**const** z: **string** = b; *// no compile time errors**// some time later somewhere in the code* 
z.toLowerCase(); *// runtime error!*
```

## 2.它打破了类型推理。所有带有“any”的操作都产生“any”:

```
**const** a: **number** = 10;
**const** b: **any** = 4;
**const** z: **string** = a + b;*// z is actually type of number
// but compiler now believes that it is a string*
```

所以不管你的类型定义有多好。即使是代码库中的几个“任何”类型也会导致对整个类型系统的不信任。

# 如果允许类型断言，就不能信任 TpeScript 类型。

另一个容易使类型系统充满谎言的类型脚本特性是类型断言。我认为生产代码中的所有类型断言都应该用 [tslint 规则](https://palantir.github.io/tslint/rules/no-object-literal-type-assertion/)禁止，除了“ [as const](https://blog.logrocket.com/const-assertions-are-the-killer-new-typescript-feature-b73451f35802/) ”断言。这就是为什么:

## 1.如果很容易，你更有可能走捷径:

```
**const** catFromApi: **unknown** = {
   meow: () => console.log("meow")
};// Less than 10 characters to lie TS compiler..
**const** cat: **Dog** = catFromApi **as Dog**;
```

如果你禁止类型断言语法，但是有一种情况，当你需要断言你的类型来帮助 TS 编译器时，使用[类型守卫](https://www.typescriptlang.org/docs/handbook/advanced-types.html#user-defined-type-guards):

```
**function** isCat (obj: **unknown**): obj **is Cat** {
   if (**typeof** obj **!==** 'object' **||** obj **===** null) {
      **return** false;
   }

   *// now we with TS compiler know for sure that it is an object.* **const** possibleCat: **Partial<Cat>** = obj; // no errors
   **return typeof** possibleCat.meow **===** 'function';
}**if** (isCat(catFromApi)) {
   **const** cat: **Cat** = catFromApi;
} **else** {
   **throw new** **Error**("Invalid api data, cat is expected.");
}
```

是的，更多的代码，但是更少的出错的可能性。还有一些像 [io-ts](https://github.com/gcanti/io-ts) 这样的库可以帮助你以一种更具可读性和声明性的方式编写你的类型保护。

## 2.在重构过程中，你有更大的机会引入错误。

```
**interface** **Cat** {
   meow: **Function**,
   name: **string**
}**const** cat1: **Cat** = {
   meow: () => "meow",
   name: 'daisy',
   hoof: ''  // that line will produce compile error
}**const** cat2: **Cat** = (<**Cat**> {
   meow: () => "meow",
   name: 'daisy',
   hoof: ''  // that line will NOT produce compile error
})
```

# 您的团队成员必须学习文档，以找出如何处理上面提到的约束。

这是最重要的部分。JS 世界中没有 Java 等静态类型语言经验的人通常习惯于不关心类型。他们中的许多人将类型视为可选的东西。因此，在每一种复杂的情况下，当我们需要一些[](https://www.typescriptlang.org/docs/handbook/generics.html)**类型的一般类型*或*条件类型*时，他们可能会试图做类型断言或定义“任何”，而不会试图理解发生了什么。这就有可能在你的代码库中传播“谎言类型”。*

*因此，如果你的团队不熟悉高级类型，那么最初拥有如此严格的编译器配置可能会对他们的速度产生一些影响。[但是最初提高知识的努力值得一个健全的打字系统的益处](http://www.typescriptlang.org/docs/handbook/advanced-types.html)。从长远来看，你的代码库不容易出现错误，尤其是如果你没有 100%的高质量单元测试覆盖率。*

# *了解有关 TypeScript 的更多信息:*

*[TypeScript:第 1 部分](https://medium.com/javascript-in-plain-english/what-are-typescript-strict-compiler-options-part-1-5822c67c1df5)中的这些严格的编译器选项是什么*

*[TypeScript:Part 2](https://medium.com/@kreznykov/what-are-these-strict-compiler-options-in-typescript-part-2-a7e974b13e54)中的这些严格的编译器选项是什么*