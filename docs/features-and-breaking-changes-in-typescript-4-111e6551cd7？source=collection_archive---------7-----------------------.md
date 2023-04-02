# TypeScript 4 中的功能和重大更改

> 原文：<https://javascript.plainenglish.io/features-and-breaking-changes-in-typescript-4-111e6551cd7?source=collection_archive---------7----------------------->

![](img/549fc5e86886d47b9b10273cc31486ca.png)

Photo by [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

8 月 26 日发布的新版 TypeScript，typescript 4。在本文中，我们将讨论最新 Typescript 版本中发布的所有新特性、改进和重大变化。

# 谁应该阅读这篇文章？

1.  **任何具备 JavaScript 或打字基础知识的人**
2.  **前端开发者(Angular/React):** Typescript 是 Angular 的基础。我们已经在 angular 版本 11 和 12 即将到来，作为 angular/前端开发人员，我们有责任让自己与 typescript 保持同步，这样我们就可以编写不会真正过时的代码，并且我们可以利用微软和 typescript 团队每天都在开发的所有新功能来改善我们的生活。
3.  **与时俱进的程序员**

# [什么是 TypeScript？](https://medium.com/javascript-in-plain-english/typescript-101-understanding-the-basics-in-3-minutes-e2113a9c4c5f)

1.  这是一种建立在 JavaScript 之上的语言。
2.  它基本上为我们的静态类型增加了语法。
3.  其思想是使用 typescript 对代码进行基本的类型检查，并在运行代码之前告诉您错误。

例如，在 javascript 环境中，您可以编写:

```
let a = "cat";
```

在下一行，你可以写:

```
a = 24; // and this is a problem!
```

没有人会阻止你，你的编译器不会，你的 IDE 也不会，你自己也不会知道，除非代码崩溃了，因为你期待的是一个字符串，而你得到的是一个整数，你只能在后期制作时才知道。

> 因此，基本上 typescript 的目标是避免那些错误，避免那些明显的错误，使我们的代码更干净，更可读。

# TypeScript 4.0 新增功能摘要

1.  标记元组元素
2.  从构造函数推断类属性
3.  短路赋值运算符
4.  更好的 catch 子句绑定
5.  VS 代码改进

# 标记元组元素

我最感兴趣的第一个变化是标签元组元素。不熟悉编程的人都知道，我们可以将元组作为可变数量的参数传递给任何函数，这样函数就可以真正动态化。

```
// Javascript =>function addressJs(...args) {
    // ...
}addressJs(2505, 'Sherbrooke East');addressJs('Sherbrooke East', 2505, 10); // oops!
```

尽管 TypeScript 从一开始就支持它:

```
// TypeScript < 4.0 => function addressTs(...args: [number, string]): void {
    // ...
}addressTs(2505, 'Sherbrooke East'); // worksaddressTs(2505, 'Sherbrooke East', true); // error
addressTs(2505); // error
```

在这个函数中，我们有两个参数，第一个是数字，第二个是字符串。

但是，它看起来与下面的函数没有什么不同:

```
function addressTs2(arg0: number, arg1: string): void {
    // ...
}addressTs2(2505, 'Sherbrooke East'); // works
```

它基本上意味着参数 0 是一个数字，参数 1 是一个字符串。

> 这已经比 javascript 好得多了，因为在 javascript 中你不知道我们是否必须先传递一个字符串，再传递一个数字。

**问题:**在这个例子中，对于程序员来说，他/她是应该将公寓号作为第一个参数，将城市作为第二个参数，还是将街道号或街道名作为第二个参数，这一点并不明显！

可能会有各种各样的场景，可能会有很多困惑。

## TypeScript 4 有什么变化？

现在更好的是我们可以命名元组。例如，我们必须定义范围，我们实际上可以说第一个变量被标记为`start`，第二个变量被标记为`end`，这大大增加了代码的可读性。

```
// In TypeScript 4.0, tuples types can now provide labels =>type StreetAddress = [streetNumber: number,streetName: string] 
| [streetNumber: number, streetName: string, city: string];function addressWithLabels(...address: StreetAddress): void {
    //...
}addressWithLabels(2525, 'Sherbrooke East'); //works
addressWithLabels(2525, 'Sherbrooke East', 'Montreal'); //works
```

现在我们可以定义或标记我们的论点。我们可以说第一个应该是`streetNumber`，第二个应该是`streetName.`

此方法接受街道地址。现在，街道地址可以包含街道编号和街道名称，也可以包含街道编号、街道名称和城市。因此，现在很清楚了。

当我们有一个大的代码库时，这将有所帮助！在大公司里，我们有成千上万行代码，如果这些东西能让程序员的生活变得更容易、更好，那么为什么不实现它们呢？

> 此外，我们不必依赖文档或注释。这种代码可读性很好，我们需要传递什么样的参数也很明显。

如果您真的想利用可变数量的参数:

```
type StreetAddressComplete = [
    streetNumber: number,
    streetName?: string,
    ...rest: any[]
];
```

在这个例子中，我们可以说第一个必须是一个 **streetNumber** ，这将是一个 **number** 类型，第二个必须是一个**字符串**，第三个可以有我们想要的任何类型的参数。所以我们仍然可以利用可变数量的参数。

# 从构造函数推断类属性

现在，typescript 可以使用控制流来分析这些变量的类型，在这种情况下，您定义了一个类 **Square** ，具有 **area** 和 **sideLength** ，但是您没有给出类型:

```
class Square {
    // Previously: implicit any!
    // Now: inferred to `number`!
    area;
    sideLength; constructor(sideLength: number) {
        this.sideLength = sideLength;
        this.area = sideLength ** 2;
    }
}
```

它分析说，在构造函数中，你传递了一个数字，然后你分配了这个数字，对它做了一些操作，并把它分配给了这个区域。但是 typescript 足够聪明，知道它仍然是一个数字，它会假设你的变量是数字。

但是，它有一个限制，例如:

```
class Square {
    sideLength; constructor(sideLength: number) {
        if (Math.random()) {
            this.sideLength = sideLength;
        }
    } get area() {
        return this.sideLength ** 2;
        //     ~~~~~~~~~~~~~~~
        // error! Object is possibly 'undefined'.
    }
}
```

在这种情况下，它在一个 **if** 条件中被赋值。所以在这种情况下，typescript 的编译器不能确定这个变量的类型是不是数字，因为它有可能是未定义的。

# 短路赋值运算符

在 TypeScript 4 中，他们宣布现在我们有三个新的赋值操作符:

1.  `&&=`
2.  `||=`
3.  `??=`

所以基本上我们可以替换这些作业

```
a = a && b; 
a = a || b; 
a = a ?? b;
```

有了新的。

> 每个程序员都喜欢更短的代码。

# 更好的 catch 子句绑定

```
try {
    // ...
}
catch (x) {
    // x has type 'any' - have fun!
    console.log(x.toUpperCase());
    x++;
}
```

这基本上意味着，如果我们之前尝试了一个 catch block，我们不知道`type x`是什么，它可以是`any`类型，我们可以做一个应该只针对整数的操作，并且它不会在代码实际执行之前抛出错误。

现在我们有了一个类型`unknown`，我们可以定义它，并将其分配给异常:

```
try {
    // ...
}
catch (e: unknown) {
    console.log(e.toUpperCase()); // compile error
    if (typeof e === "string") {
       console.log(x.toUpperCase()); 
    }
}
```

现在如果我们试着说`e.toUpperCase()` 编译器会抛出一个错误，因为它不认为`type unknown`有这个方法。所以我们不得不缩小范围。如果异常(e)的类型是字符串，那么我们可以调用它。

> *这非常方便，因为您不希望捕获代码中出现错误或异常！*

# 打破变化

最后，我只想提一下，这个版本没有重大的突破性变化，所以您可以简单地更新代码库中的 typescript 版本，您就可以开始了！

我希望本文能帮助您理解最新的 TypeScript 版本。

谢谢你的阅读。如果您有任何问题，请随时留言回应。

# 资源

1.  [https://dev blogs . Microsoft . com/type script/notify-type script-4-0/](https://devblogs.microsoft.com/typescript/announcing-typescript-4-0/)