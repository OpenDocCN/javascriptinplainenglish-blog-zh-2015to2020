# JavaScript 中 8 个最常用的数组操作(ES6+)

> 原文：<https://javascript.plainenglish.io/8-most-used-array-operations-in-javascript-es6-77a574905d7a?source=collection_archive---------5----------------------->

## 增强你的数据处理技能

![](img/324f35fb844c7499ae8ab57fed7c263d.png)

Image by [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2761159) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2761159)

数组是最常见的数据结构之一，你需要绝对自信地使用它。在这里，我将列出 JavaScript 中最常见的 8 个数组操作片段，包括数组长度、替换元素、排序等等。

# 1.数组长度

大多数人都知道可以像这样得到数组的长度:

```
const a = [1, 2, 3]; 
console.log(a.length); // 3
```

有趣的是，你可以手动修改长度。这就是我所说的:

```
const a = [1, 2, 3]; 
a.length = 2; 
a.forEach(i => console.log(i)); // 1 2
```

甚至创建指定长度的新数组:

```
const a = []; 
a.length = 100; 
console.log(a) // [undefined, undefined, undefined ...
```

这不是一个很好的实践，但是仍然值得*了解*。

# 2.替换数组元素

有几种方法可以解决这个问题。如果需要**替换指定索引**处的元素，使用`[splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)`:

```
const a = [1, 2, 3]; a.splice(2, 1, 4); // change 1 element at index 2 to 4 console.log(a); // [1, 2, 4] a.splice(0, 2, 5, 6) // change 2 elements at index 0 to 5 and 6 console.log(a); // [5, 6, 4]
```

如果您需要根据内容替换项目**或者**您必须创建一个新的数组**，请使用`[map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)`:**

```
const a = [1, 2, 3, 4, 5, 6]; // Now square all odd numbers 
const b = a.map(item => item % 2 == 0 ? item : item*item); console.log(b); // [1, 2, 9, 4, 25, 6];
```

`map`接受一个函数作为其参数。它将为数组的每个元素调用这个函数*一次*，并创建一个函数返回的新的项目数组。

# 3.过滤数组

在某些情况下，您需要删除数组中的某些元素，创建一个新的元素。在这种情况下，使用 ES6 中引入的令人惊叹的新`[filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)`方法:

```
const a = [1, 2, 3, 4, 5, 6, 7]; // Filter out all odd numbers 
const b = a.filter(item => item % 2 == 0); 
console.log(b); // [2, 4, 6];
```

`filter`的工作原理与`map`非常相似。你给它提供一个函数，`filter`会在数组的每个元素上调用它。如果要在新数组中包含这个特定元素，函数必须返回 true，否则返回 false。

# 4.合并数组

如果您想将多个数组合并成一个数组，有两种方法。**ES6 之前的** JS 提供了`[concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)`功能:

```
const a = [1, 2, 3]; 
const b = [4, 5, 6]; 
console.log(a.concat(b)); // [1, 2, 3, 4, 5, 6]
```

然而，有一个更方便的方法，这要归功于 ES6 中引入的**传播**操作符:

```
const a = [1, 2, 3]; 
const b = [4, 5, 6]; 
console.log([...a, ...b]); // [1, 2, 3, 4, 5, 6]
```

# 5.复制数组

由于忙于编码，很容易忘记变量并不存储数组，而仅仅是一个引用。我的意思是:

```
const a = [1, 2, 3]; 
const b = a; 
b[0] = 4; 
b[1] = 2; 
b[2] = 0; 
console.log(a); // [4, 2, 0]
```

由于`b`持有对`a`的引用，所以对`b`的任何更改也是对`a`的更改。要解决此 **pre-ES6** ，请使用`[slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)`功能:

```
const a = [1, 2, 3]; 
const b = a.slice(0); 
b[1] = 5; 
console.log(a); // [1, 2, 3]; 
console.log(b); // [5, 2, 3];
```

如果您在 ES6+友好的环境中，也可以使用 spread 运算符:

```
const a = [1, 2, 3]; 
const b = [...a]; 
b[1] = 5; 
console.log(a); // [1, 2, 3]; 
console.log(b); // [5, 2, 3];
```

# 6.从数组中删除重复项

有 3 种方法可以从数组中删除重复项。前两个是 ES6 之前的版本，第三个是使用新功能。

**使用** `**reduce**` **。**您可以使用`[reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)`功能从数组中删除所有的重复项。然而，这有点棘手。`reduce`将接受一个函数并在其中传递两个参数:来自数组的当前值和*累加器*。累加器在项目间保持不变，最终返回:

```
const a = [1, 1, 2, 3, 1, 5, 9, 4, 2]; const b = a.reduce(
  (acc, item) =>
    acc.indexOf(item) == -1 ? acc : [...acc, item],
  [] // initial accumulator
);console.log(b); // [1, 2, 3, 5, 9, 4]
```

**使用过滤器。**另一个可以帮助你在 JavaScript 中从数组中删除重复项的函数是`filter`，我们之前已经介绍过了:

```
const a = [1, 1, 2, 3, 1, 5, 9, 4, 2]; const b = a.filter((item, index) => a.indexOf(item) == index);

console.log(b); // [1, 2, 3, 5, 9, 4]
```

**使用设定**。最后，您可以使用`[Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)`(ES6 中引入的新数据结构)和 spread 操作符:

```
const a = [1, 1, 2, 3, 1, 5, 9, 4, 2]; 
const b = [...(new Set(a))]; 
console.log(b); // [1, 2, 3, 5, 9, 4]
```

# 7.转换数组

有时你必须将其他数据结构如集合或字符串转换成数组。在这种情况下，使用`[Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)`功能:

```
const str = 'abacaba'; 
const st = new Set(1, 2, 3); console.log(Array.from(str)); // ['a', 'b', 'a' ... console.log(Array.from(st)); // [1, 2, 3]
```

# 8.遍历数组

有两种简便的方法可以迭代数组。适用于所有 JS 版本的最简单的方法是常规 for 循环:

```
const a = [1, 2, 3]; for (let i = 0; i < a.length; i++) { 
  console.log(a[i]); 
} // 1 2 3
```

使用更新的、符合 ES6 的 JS，您还可以获得 foreach 循环:

```
for (const i of a) {
  console.log(i); 
} // 1 2 3
```

最后，还有`[forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)`功能:

```
a.forEach(i => console.log(i)); // 1 2 3
```

`forEach`的工作方式与`map`、`filter`和`reduce`非常相似:它会对每个项目调用一次函数，但不会返回任何内容。

# 结束语

谢谢你的阅读，我希望你喜欢我的文章。继续订阅更多关于 JavaScript 和软件开发的有趣材料！

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**

# 资源

*   [MDN 上的数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
*   [10 JS 2020 年回顾问题](https://medium.com/javascript-in-plain-english/10-javascript-interview-questions-for-2020-697b40de9480)
*   [通过 JS 代理 API 实现无限甚至更远的距离](https://medium.com/javascript-in-plain-english/to-infinity-and-beyond-with-javascript-proxy-api-8d4f7a26c8dc)