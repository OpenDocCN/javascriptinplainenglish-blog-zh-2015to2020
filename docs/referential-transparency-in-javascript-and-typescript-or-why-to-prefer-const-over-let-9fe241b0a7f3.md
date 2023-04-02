# JavaScript 和 TypeScript 中的参考透明度:或者为什么喜欢常量而不是字母

> 原文：<https://javascript.plainenglish.io/referential-transparency-in-javascript-and-typescript-or-why-to-prefer-const-over-let-9fe241b0a7f3?source=collection_archive---------6----------------------->

![](img/289cce903df77530ee176691ef9b59bd.png)

our code through the eyes of the compiler

> 当我回顾我的编程经验时，我明白在我职业生涯的开始，读别人的代码比写代码困难得多。尽管现在我认为我擅长这两项任务，但我正在阅读的代码的风格有所不同。

# 什么是参照透明度？

如果我们能在不改变程序行为的情况下用相应的值替换它，一个被称为[的表达式是可引用透明的](https://en.wikipedia.org/wiki/Referential_transparency)。下面的两个例子将帮助你理解最后一句话:

## 1.不透明的表达:

```
**let** x = 10;*// ... imagine 100 lines of code between that 2 lines***const** z = x + 5; *// what is the value of z?*
```

当我们定义 **z** 时，不能保证 **x** 仍然有值 **10** (因为**让**允许重新分配)。所以现在我们需要努力搜索 100 行代码来回答这个问题"*z 的值是多少？*”。

## 2.指代透明表达:

```
**const** x = 10;*// ... imagine 100 lines of code between that 2 lines***const** z = x + 5; *// what is the value of z?*
```

在这里您只需要用 **x** 的赋值来检查一行，就可以理解 **z** 的值了，因为 **const** 关键字保证了 **x** 在运行时的值永远是 **10** 。所以表达式可以用它的相应值代替，而不改变程序的行为:

```
**const** z = 15;
```

注意:参考透明度的主题要宽泛得多，可以触及不可变数据结构和纯函数的概念。我特意想在这里只关注 **const vs let** 部分。

# 参照透明表达的好处是什么？

我想您可以从前面的例子中猜到，主要的好处是代码更加清晰。人们不需要从赋值开始就怀疑变量值的任何变化，就像他们通常对 **let** 所做的那样。这在使用部分 git diff 进行代码审查时尤其有用。

此外， **const** 提供了编译器级优化的可能性。

这就是为什么在你没有意图变异你的变量之前，要使用 **const。**如果你习惯于编写你认为变量不可重分配的代码，你会发现你的代码库中只有不到 1%的**让**变量的真实情况。不要害怕在你的项目中启用[“prefere-const”](https://eslint.org/docs/rules/prefer-const)lint 规则。

# 人们误用 let 关键字的一些典型案例。

## 1.条件变量赋值

```
**let** x;**if** (isTrue) {
   x = 10;
} **else** {
   x = 100;
}
```

你可以很容易地在这里使用一个三元运算符来声明 x 为**常量**

```
**const** x = isTrue **?** 10 **:** 100;
```

## 2.变量的无意义重新赋值

```
**let** str = 'cats,love,fish';
str = str.split(',').join(' ');
```

不要偷懒，用一个更好的名字创建一个新的**常量**变量:

```
**const** str = 'cats,love,fish';
**const** transformedStr = str.split(',').join(' ');
```

## 3.对内置 Array.prototype 函数了解不多

```
**const** result = [];**for** (**let** i=0; i < arr.length; i++) {
    **if** (arr[i] % 2) {
        result.push(arr[i]);
    }
}
```

这里不需要显式的循环计数器:

```
**const** result = arr.filter(element => element % 2);
```

## 4.缺乏抽象思维[一个先进的例子]

然而，在某些情况下，您可能需要使用 let，但是您可以将它隐藏在一个抽象的实用函数中，或者放在一些像 lodash 这样的库中。这里有一个简单的例子:

```
**let** isLikeSent = **false**;*// that function likes an article only once*
**function** sendUsersLike() {
   **if** (!isLikeSent) {
      isLikeSent = true;
      likeAnArticle('anID');
   }
}
```

让我们用 [lodash once](https://lodash.com/docs) 函数重写它。它能满足我们的需求:

```
**import** once **from** 'lodash/once';*// that function likes an article only once*
**const** sendUsersLike = once(() => likeAnArticle('anID'));
```

如果您想知道如何实现自己的类似于 lodash 的函数，请查看下一段代码。最后，一个有效的案例为 **let** 声明:

```
**const** once = (func) => {
   **let** isCalled = **false**;**return** (...args) => {
      **if** (!isCalled) {
         isCalled = **true**;
         func(...args);
      }
   }
}
```

# 额外收获:说几句关于打字稿的话

当 TypeScript 编译器进行类型推断时，如果您的代码具有更多引用透明的表达式，它往往会更加精确和有效。查看下一个示例:

```
**const** x = 'a'; // here inferred type of x is a constant **'a'** which is subtype of **string**;**function** foo(param: **'a' | 'b'**) {}
**function** bar(param: **string**) {}foo(x); // no errors **'a'** is one of union type values
bar(x); *// no errors, compiler will make* [*type widening*](https://mariusschulz.com/blog/literal-type-widening-in-typescript)
```

现在让我们看看如果我们将 **x** 声明为 **let** 会发生什么

```
**let** x = 'a'; *// here inferred type of x is a* **string**;**function** foo(param: **'a' | 'b'**) {}
**function** bar(param: **string**) {}foo(x); *//* **error!** *type string is wider than the union* **'a' | 'b'**
bar(x); *// no errors, because type matches.*
```

但是如果在没有赋值的情况下声明了 **let** ，情况会变得更糟:

```
**let** x; *// we forgot to declare type, so now x has* **any** *type!*x = 10; // it doesn't matter what we assign it will remain **any**// so now your x starts spreading [**any** type epidemy](https://medium.com/@kreznykov/there-is-no-point-to-use-typescript-in-your-project-if-you-dont-care-about-types-68131deeb43a)..
```

# 结论

所以在代码中避免使用 **let** 可以确保你不会无意中重新分配变量。它帮助人们更快地理解你的代码。它可能会在未来给你的应用程序带来性能优势。TypeScript 提供了更好的类型推断。它帮助您编写更接近函数式编程风格的代码。这里唯一的缺点是，它需要一些努力来改变你的思维模式。但是相信我，这是值得的。