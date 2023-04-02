# TypeScript:第 1 部分中这些严格的编译器选项是什么

> 原文：<https://javascript.plainenglish.io/what-are-typescript-strict-compiler-options-part-1-5822c67c1df5?source=collection_archive---------2----------------------->

![](img/ebb6d5a06d0a76d5f3e9a27ad66281f8.png)

When TypeScript compiler runs in strict mode

> [在我的上一篇文章](https://medium.com/javascript-in-plain-english/there-is-no-point-to-use-typescript-in-your-project-if-you-dont-care-about-types-68131deeb43a)中，我简要提到了每个严肃的 TypeScript 项目都应该启用`*--strict*` 编译器选项，但是我没有提供任何关于它做什么的额外细节。[官方打字手册第](https://www.typescriptlang.org/docs/handbook/compiler-options.html)页很没用，也没有太多细节。因此，为了缩小这一差距，我将详细描述严格家庭中最重要的选择。

# -不需要

该选项防止您在代码库中添加意外的**任何**类型。为了将您的注意力集中在该标志的好处上，我将向您展示一个尽可能简单的代码示例。这可能看起来很傻，但这种问题是真实的，并且经常发生在没有质量测试覆盖的大规模代码库中。因此，让我们假设您有一个普通的 *isOddNumber* 函数，其中您忘记了添加类型注释:

```
**const** isOddNumber = (num) => (num % 2) !== 0;
**const** nums = [1, 2, 3];console.log(isOddNumber(nums[0])); *// => prints true* console.log(isOddNumber(nums[1])); *// => prints false**// oops there is a typo in our code, we planned to pass nums[2];*
console.log(isOddNumber(nums)) *// => prints true**// funny that it would work as intended with 1 element in array*
console.log(isOddNumber([3])) *// => prints true*
```

尽管该代码可以成功编译，并且不会产生运行时异常，但是第三个 console.log 语句有一个令人讨厌的错误，如果不加注意，它可能会在您的应用程序中存在很长时间。罪魁祸首是**任何**类型。即使您没有在代码中显式地看到它，它也是存在的。这是因为每当 TypeScript 编译器找不到更好的方法时，它就有权添加任何类型。所以在编译的时候，你的前两行将被转换成这样:

```
**const** isOddNumber = (num: **any**): **boolean** => (num % 2) !== 0;
**const** nums: **number**[] = [1, 2, 3];
```

这里，TypeScript 编译器毫无问题地正确推断了两种类型(对于数组和返回值)，但是对于它来说， *isOddNumber* 函数的输入值是一项不可能完成的任务。但是，如果您的编译器在启用了`*--noImplicitAny*` 选项的情况下运行，它将无法在您的控制台中构建和打印以下错误消息:

```
**ERROR: Parameter 'num' implicitly has an 'any' type**
```

太棒了，现在作为开发人员，您可以控制是否为 *num* 参数编写一个 **number** 类型，或者将其声明为 **any** 。

# -不影响这个

因为 JavaScript 是一种非常动态的语言，能够调用任意传递**这个**上下文的函数，所以在很多情况下 TypeScript 不能进行正确的类型推断。因此，如果我们不启用该选项，TypeScript 会假设您不关心这个关键字的类型**，并且会对它使用**任何**类型，直到您指定了其他内容。**

```
**function** foo(hello: **string**) {
    console.log(hello + ' ' + **this**.fullName);
}foo.call({ fullName: 'Kirill' }, 'hi'); // => prints "hi Kirill"**const** x = { fullName: 'Max', foo: foo }
x.foo("hello"); // => prints "hello Max"foo.call(10, "hey"); // no errors, prints "hey undefined"
foo('hi'); // runtime error, but no compile time error
```

为了向 TypeScript 显示您关心这个关键字的**类型，并防止它默认为**任何一个**只要您忘记声明它，我们需要启用`*--noImplicitThis*` 选项。在此之后，您将在构建时遇到错误，直到您为此**提供类型。要定义**这个**类型，需要先添加一个**的** *伪参数*，并命名为**这个**:****

```
**interface** User { fullName: **string** }**function** foo(**this**: User, hello: **string**) {
    console.log(hello + ' ' + this.fullName);
}foo.call({ fullName: 'Kirill' }, 'hi'); // => prints "hi Kirill"**const** x = { fullName: 'Max', foo: foo }
x.foo("hello"); *// => prints "hello Max"*foo.call(10, "hey"); // **compile error**, '10' is not a type of 'User'
foo('hi'); *//* ***compile error****, 'void' is not assignable to 'User'.*
```

**太好了，现在**的类型这个**关键词在你的应用中永远不会是**任何**直到开发者决定它是某个特定功能所必需的。**

# **---------------------------------------------------------------------------------------------------------------------------------------------------------------------------**

**当您使用**绑定**、**调用**或**应用**时，该选项要求 TypeScript 编译器检查参数类型。默认情况下，它不关心这种上下文中的类型，给你留下更多出错的机会:**

```
**function** bar(a: **string**, b: **number**) {
   console.log(a, b);
}**// no errors**
bar('cool', 10);**// error** since we didn't pass the second argument
bar('cool');**// no errors**
bar.call(null, 'cool');
bar.call(null, true, false); bar.bind(null)();
bar.bind(null, 'cool')(10);
```

**一旦我们启用`*--strictBindCallApply*`选项，在编译过程中将会看到完全不同的结果:**

```
**// no errors**
bar('cool', 10);**// error** since we didn't pass the second argument
bar('cool');**// error** since we didn't pass the second argument
bar.call(null, 'cool');**// error** since types are not matching
bar.call(null, true, false);**// error** since we didn't pass arguments at all
bar.bind(null)();**// no errors** type and amount of parameters mathes
bar.bind(null, 'cool')(10);
```

# **结论**

**我希望这篇文章能够向你解释这些选择有多重要。然而，这并不是一个详尽的列表，我在第二部分中写了剩余的严格选项。别错过了。**