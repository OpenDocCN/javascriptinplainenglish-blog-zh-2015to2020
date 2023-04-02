# 迭代 TypeScript 字符串枚举

> 原文：<https://javascript.plainenglish.io/iterating-on-a-typescript-string-enum-45f7e36b015b?source=collection_archive---------0----------------------->

枚举太棒了。今天我们来学习一个很酷的小技巧。

![](img/f9d982f3e62320cb48fc515cdef68cb5.png)

Picture courtesy of [Srinivas JD](https://unsplash.com/@kirisrini)

枚举很酷，但字符串枚举更好。我在我的项目中使用了很多，因为它们允许编写清晰可读的代码。

在本文中，我将向您展示如何在 TypeScript 中迭代字符串枚举的键和值。

# 例子

假设我们有以下字符串枚举:

这是一个基本的枚举，表示一些动作的可能状态(欢迎回到 Todo 应用仙境！).

# 迭代这些值

为了迭代这个枚举的值，我们可以使用 [Object.values()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Object/values) 内置函数，该函数返回一个数组，该数组的元素是在对象上找到的可枚举属性值。

注意，如 MDN 文档中所述，顺序与手动循环对象的属性值的顺序相同。

这里我使用了函数式风格，但是您也可以使用一个`for..of`循环，这样更有效:

使用这两种方法，您应该得到以下结果:

```
console.log src/database/services/database.service.spec.ts:207
    To doconsole.log src/database/services/database.service.spec.ts:207
    In progressconsole.log src/database/services/database.service.spec.ts:207
    Done
```

整洁！

字符串枚举非常棒，因为它们易于重构，并且在代码库中容易“找到”。这就是为什么我通常更喜欢字符串枚举而不是字符串联合。虽然，正如弗雷德里克·坎布洛指出的那样，这完全取决于你需要能够做什么；如果您想在运行时迭代这些值，那么最好使用枚举；如果只想在编译时迭代这些值，那么可以使用映射类型的联合字符串。

# 对键进行迭代

当然，也可以使用 [Object.keys()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) 循环遍历字符串枚举的键。

# 参考

这里有一些关于这方面的参考资料:

*   [https://github.com/Microsoft/TypeScript/issues/20813](https://github.com/Microsoft/TypeScript/issues/20813)
*   [https://github.com/microsoft/TypeScript/issues/4753](https://github.com/microsoft/TypeScript/issues/4753)
*   [https://stack overflow . com/questions/39372804/typescript-how-to-loop-through-enum-values-for-display-in-radio-buttons](https://stackoverflow.com/questions/39372804/typescript-how-to-loop-through-enum-values-for-display-in-radio-buttons)

# 结论

在这篇小文章中，我向您展示了如何在 TypeScript 中轻松地循环字符串枚举的键和值。

正如我在我的书中所解释的，字符串枚举对于很多用例来说都非常有用。我希望有像 Java/Kotlin 那样强大的枚举，但也许那会是以后的事……-)

今天到此为止！

# 喜欢这篇文章吗？

如果你想了解关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的大量其他很酷的东西，那么不要犹豫去[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！