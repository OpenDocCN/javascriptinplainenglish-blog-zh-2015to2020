# JavaScript:2020 年发布了哪些功能？

> 原文：<https://javascript.plainenglish.io/javascript-whats-new-for-2020-8a4ee27fe45a?source=collection_archive---------14----------------------->

![](img/6780e9e5daea7dde64148bb64860e39d.png)

Photo by [Dean Pugh](https://unsplash.com/@wezlar11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

尽管 Javascript 最初是在 1995 年编写的，但由于其压倒性的流行，它仍在不断发展；根据 [2020 堆栈溢出开发者调查](https://insights.stackoverflow.com/survey/2020#overview)报告为最常用的语言。Javascript 的新特性由欧洲计算机制造商协会(ECMA)的技术委员会 39 (TC39)管理和实现。在 TC39 的 Github 上可以找到即将推出的特性列表。2020 年增加了几个新特性，让我们看看可选链接、无效合并和动态导入。

## 可选链接

此功能允许从位于连接对象链深处的对象读取值，而不必验证链的每一级。这将为链的每个嵌套级别节省大量编写冗余错误检查的时间。要使用可选的链操作符，只需将链操作符`.`替换为`.?`，如下例所示。

```
// Example object
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};// Without Optional Chaining
const dogName = adventurer.dog.name;
console.log(dogName);
// expected output: Error: Cannot read property 'name' of undefined// With Optional Chaining
const dogName = adventurer.dog?.name;
console.log(dogName);
// expected output: undefined **Figure 1:** Optional Chaining
```

## 无效合并

在 Javascript 中，所有的值都可以被逻辑地评估为“真”或“假”值。例如，一个空字符串(`“”`)的计算结果为假，任何其他字符串的计算结果为真。这在执行逻辑操作时非常有用，比如 OR 操作符(`||`)，但也会导致代码中出现一些偷偷摸摸的错误。现在有了 nullish 合并操作符(`??`)你可以避免那些偷偷摸摸的错误，因为如果是`null`或`undefined`，这个操作符只返回左边；也就是说，即使右边是“错误的”,它也会被返回。参见图 2 中的示例。

```
// With Logical OR Operator
const baz = 0 ||42;
console.log(baz);
// expected output: 42// With Nullish Coalescing Operator
const baz = 0 ?? 42;
console.log(baz);
// expected output: 0**Figure 2:** Nullish Coalescing
```

## 动态导入

通常，如果您想使用一个 Javascript 模块，您必须将它导入到文件的顶部，如图 3 所示。通过添加动态导入，您可以仅在需要时异步导入模块。这可以节省加载时间和内存，因此您不必等到实际需要时才加载模块。

```
// Using Standard Import
import '/modules/my-module.js';// Using Dynamic Import
(async () => {
  if (somethingIsTrue) {
    // import module for side effects
    await import('/modules/my-module.js');
  }
})();**Figure 3:** Dynamic Imports
```

## 还有什么？

今年 JavaScript 增加了不止这三个特性，要查看完整列表，请访问 [TC39 的 Github](https://github.com/tc39/proposals) 。我还建议看看[JavaScript 的未来——2020 年的新特性和颠覆性趋势](https://www.youtube.com/watch?v=f0DrPLKf6Ro&list=PL0vfts4VzfNixzfaQWwDUg3W5TRbE7CyI&index=6)视频，其中涵盖了这些特性以及其他一些特性。

## 参考

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) [## 可选链接(？。)

### 可选的链接操作符()允许读取一个位于连接的…链深处的属性值

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator) [## 零化合并算子(？？)

### 零化合并算子(？？)是一个逻辑运算符，当它的左…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) [## 进口

### 静态导入语句用于导入由另一个模块导出的只读活动绑定。已导入…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)