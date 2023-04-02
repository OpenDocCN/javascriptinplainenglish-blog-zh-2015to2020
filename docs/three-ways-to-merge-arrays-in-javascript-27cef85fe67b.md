# JavaScript 中合并数组的四种方法

> 原文：<https://javascript.plainenglish.io/three-ways-to-merge-arrays-in-javascript-27cef85fe67b?source=collection_archive---------1----------------------->

## 学习在 JavaScript 中合并数组的不同方法

![](img/a42aa7a02f79f38f9f0d23c8446c3ecd.png)

Image by [Makarios Tang](https://unsplash.com/@makariostang?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 1.使用 for 循环

我们可以循环遍历需要推送的数组源数组，然后逐个添加元素

```
function push(fromArray, toArray) { **for(let i = 0, len = fromArray.length; i < len; i++) {
            toArray.push(fromArray[i]);
     }** return toArray;
} var array1 = [1,2,3,4,5];var array2= [6,7,8,9,10];var array3 = [];push(array1, array3);push(array2, array3);
```

# 2.使用扩展运算符

> **扩展语法**允许在应该有零个或多个参数(用于函数调用)或元素(用于数组文字)的地方扩展一个可迭代对象，如数组表达式或字符串

```
var array1 = [1,2,3,4,5];var array2 = [6,7,8,9,10];var array3 = [...array1, ...array2];array3; // [1,2,3,4,5,6,7,8,9,10];
```

我们可以使用带有 push 的扩展运算符

```
var array1 = [1,2,3,4,5];var array2 = [6,7,8,9,10];var array3 = []array3.push(...array1, ...array2);array3; //  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

我们也可以将 spread 运算符与`Array.of`一起使用

```
var array1 = [1,2,3,4,5]
var array2 = [6,7,8,9,10];var array3 = Array.of(...array2, ...array1);
```

我们可以使用 spread 运算符将字符串转换为数组

```
var string ="two";var arrayOfChar = [...string];arrayOfChar; //["t", "w", "o"]
```

# 3.使用 concat 方法

> `**concat()**`方法用于合并两个或多个数组。这个方法不改变现有的数组，而是返回一个`**new array.**`

```
var array1 = [1,2,3,4,5]var array2 = [6,7,8,9,10]; var array3 = array1.concat(array2);// orvar array3 = [].concat(array1, array2);
```

我们可以传递数组以外的参数。

```
var a = "string";var b = 1;var c = {};var combined = [].concat(a, b, c)combined; //  ["string", 1, {…}]
```

# 4.使用 Reduce 方法

> `**reduce()**`方法对数组的每个元素执行一个 **reducer** 函数(您提供的),产生一个输出值。

```
var array1 = [1,2,3,4,5];var array2 = [6,7,8,9,10];var array3 = array2.reduce((newArray, item) => { **newArray.push(item);
          return newArray;** ), **array1**);
```