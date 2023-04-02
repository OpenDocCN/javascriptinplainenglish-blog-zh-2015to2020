# TypeScript:第 2 部分中这些严格的编译器选项是什么

> 原文：<https://javascript.plainenglish.io/what-are-these-strict-compiler-options-in-typescript-part-2-a7e974b13e54?source=collection_archive---------6----------------------->

![](img/4460282c1b3ba14deb2df3785f98228f.png)

Type system in TypeScript

> 那是[上一篇](https://medium.com/javascript-in-plain-english/what-are-typescript-strict-compiler-options-part-1-5822c67c1df5)的第二部分。在这里，我将介绍 TypeScript 编译器的所有其他严格选项，尽可能保持文章的简单易懂，而不会涉及太多细节。

# - -严格函数类型

这个选项修复了在我看来是 TypeScript 编译器中的一个错误。如果这不是一个错误，那么这只是一个糟糕的设计决策，一个新的编译器选项的出现证明了我的观点。让我们从一个例子开始，默认情况下，下一个代码将被编译，没有任何问题:

```
*// Focus all your attention on callback signature
// It has* date parameter which is *a union type***function** getCurrentYear(callback: (date: **string | number**) => **void**) {
   callback((Math.random() > 0.5) ? '2020' : 2020);
}*// note that we ignored the fact that in 50% cases our callback returns* ***string*** *type instead of* ***number****.*getCurrentYear((date: **string**) => {
    console.log(date.charAt(0)); *// in 50% it is* ***RUNTIME ERROR***
});
```

所以传递给 *getCurrentYear* 的 arrow 函数缩小了“date”参数的类型，而 TypeScript 并不关心它。然而，同样的技巧在不同的上下文中使用变量时，即使没有任何严格的规则也会产生错误:

```
**let** x: **string** | **number** = (Math.random() > 0.5) ? '2020' : 2020;
**const** y: **number** = x; *//* ***COMPILE TIME ERROR***
```

这更有意义，启用`--strictFunctionTypes`将要求编译器在回调函数中遵循相同的行为。这肯定会帮助你防止大项目中的一些错误。

# --严格的 NullChecks

每一个有意识写代码的开发者都会遇到这样的情况，他不确定自己得到的值是否可以 **null** ，是否需要写处理它的代码。结果，如果你相当偏执，你最终会对空的**或未定义的**进行大量不必要的检查。**另一方面，一些不关心这种小事的马虎程序员可能会创建一个运行时错误。启用`--strictNullChecks`标志将使推理不熟悉的函数变得更容易，并将帮助每种开发人员在他们的代码库中适当地处理 **null** 和 **undefined** 。**

为了举例，让我们假设下一个代码编译时没有这个标志:

```
// some unfamiliar function from 3rd party module
**function** getUserId(isLogged: **boolean**): **number** {
    **return** isLogged ? 100 : null;
}// somewhere in your code
**const** id: **number** = getUserId(**false**);id.toString(); ***// RUNTIME ERROR*** *since id is null..*
```

我们没有注意到在这种情况下 id 可以为空，也没有做任何检查。但是如果我们用`--strictNullChecks` 标志编译，我们会在编译过程中得到下一个编译错误:

```
**ERROR**: Type **'100 | null'** is not assignable to type **'number'**.
  Type **'null'** is not assignable to type **'number'**.
```

因此，如果我们，事实上，想从函数中返回空值**,那么我们需要像这样改变我们的类型注释:**

```
**function** getUserId(isLogged: **boolean**): **number | null** {
    **return** isLogged ? 100 : null;
}**const** id: **number | null** = getUserId(**false**);id.toString(); ***// COMPILE TIME*** *error since id can be null..*
```

# **- -strictPropertyInitialization**

**该标志是前一个标志(`--strictNullChecks`)的扩展，您必须启用这两个标志才能使其工作。它将相同的约束应用于类属性类型定义。如果我们在没有这条规则的情况下编译下一个例子，即使前面的标志(`--strictNullChecks`)已经被启用，一切都会好的:**

```
**class** User {
   name: **string**;setName(name: **string**) {
      this.name = name;
   }
}**const** user = new User();
user.setName('John');
```

**但是如果我们启用了`--strictPropertyInitialization`,那么我们的代码将不会编译并产生下一个错误:**

```
**ERROR: Property 'name' has no initializer and is not definitely assigned in the constructor.**
```

**其思想是，如果我们确实期望一个名字是**未定义的**，在我们构造我们的对象之后，我们需要将*名字*字段类型定义为“**字符串|未定义”**如果一个**未定义的** *名字*是一个错误，那么你必须确保它有一些默认的非空值被分配或者在对象构造期间被初始化。所以这是可行的:**

```
**class** User {
   name: **string = ""**;setName(name: **string**) {
      this.name = name;
   }
}
```

# **——永远严格**

**这个选择是最简单的。您的代码将在 ECMAScript 5 严格模式下解析，并且`"use strict"` 指令将被添加到每个输出模块的顶部。真的，这里没什么好解释的。**

# **结论**

**我希望现在您已经清楚地理解了严格模式的所有优点，并且只要有可能，就会使用它来编译您的下一个 TypeScript 项目。**