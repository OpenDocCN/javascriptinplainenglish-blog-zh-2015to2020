# JavaScript 基础—数组

> 原文：<https://javascript.plainenglish.io/javascript-basics-arrays-39c5e619f75d?source=collection_archive---------10----------------------->

![](img/b8cccd45d24f3bdcf3845770158e2327.png)

Photo by [nicolas reymond](https://unsplash.com/@nicolasreymond?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在这篇文章中，我们将看看数组。

# 数据集

我们可以使用数组来存储数据集。

在 JavaScript 中，数组只是普通的对象。

我们可以用它们来存储一系列的值。

这些值可以通过索引来访问。

例如，我们可以这样定义一个数组:

```
let nums = [1, 3, 5, 6, 8];
```

然后，我们可以通过写入以下内容来访问第一个值:

```
let num = nums[0];
```

`num`则被赋值为 1。

然后通过写入以下内容来访问第二个:

```
num = nums[1];
```

那么`num`就变成了 3。

我们必须记住数组的索引从 0 开始。

# 数组循环

我们可以使用循环来操作数组。例如，我们可以写:

```
let nums = [1, 3, 5, 6, 8];for (let i = 0; i < nums.length; i++) {
  console.log(nums[i]);
}
```

然后我们可以记录数组中的所有值。

`length`属性具有数组的长度。数组的最后一个索引应该比长度小 1，因为数组是零索引的。

为了方便起见，我们可以使用 for-of 循环来遍历数组中的所有元素。

例如，我们可以将示例重写如下:

```
for (const n of nums) {
  console.log(n);
}
```

现在，我们可以在不使用索引的情况下遍历这些项目。

# 索引 Of

我们可以使用`indexOf`方法来获取特定值的第一个实例的索引。

例如，我们可以写:

```
const index = nums.indexOf(3);
```

因为 3 在第二个位置。

如果找不到该值，则返回-1。

# lastIndexOf

`lastIndexOf`好比`indexOf`。它返回一个值的最后一个实例的索引。

例如，我们可以写:

```
const index = nums.lastIndexOf(3);
```

我们得到 1。

# 薄片

`slice`获取开始和结束索引，并返回一个包含它们之间的元素的数组。

开始索引是包含性的，结束索引是排他性的。

例如，我们可以写:

```
let nums = [1, 3, 5, 6, 8];
const arr  = nums.slice(1, 3);
```

然后我们得到`[3, 5]`,因为我们获取了第二个和第三个索引中的条目，把它们放在一个新的数组中并返回它。

# 休息参数

Rest 参数让函数接受任意数量的参数。

例如，我们可以创建自己的函数，将数组中的所有数字相乘:

```
const multiply = (...nums) => {
  return nums.reduce((prod, num) => prod * num, 1);
}
```

我们使用`...`操作符将参数传播到`nums`数组中。

然后我们将`nums`中的所有数字相乘。

如果我们调用`multiply`，我们写:

```
const product = multiply(2, 3, 4, 5, 6);
```

然后我们得到 720 的`product`。

类似地，我们可以使用 3 个点将数组条目分布到参数中。

例如，我们可以写:

```
const product = multiply(...[1, 2, 3]);
```

然后我们为`product`得到 6，因为我们将数组条目分散到数字中。

我们也可以使用一个数组中的 3 个点将一个数组扩展到另一个数组中。

例如，我们可以写:

```
const arr = [1, 2, 3]
const arr2 = [0, ...arr, 4];
```

然后我们得到`arr2`的 `[0, 1, 2, 3, 4]`，因为我们将`arr`的条目展开到`arr2`中。

![](img/bf5b01123b9bf65b8b419b771c8f7907.png)

Photo by [Han Chenxu](https://unsplash.com/@hanchenxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 解构

我们可以析构数组条目。

例如，我们可以写:

```
const multiply = ([a, b, c]) => {
  return a * b * c;
}
```

然后我们可以将一个数组传入`multiply`，并取回 3 个变量。

我们可以写:

```
multiply([1, 2, 3])
```

然后得到 6。

同样，我们可以写:

```
const [a, b, c] = [1, 2, 3];
```

那么我们得到`a`是 1，`b`是 2，`c`是 3。

# 结论

数组是可以顺序存储数据的对象。

它有许多有用的方法。

我们还可以对它做许多操作。

Rest 参数让我们将参数分散到数组中。

spread 运算符将数组扩展到参数。

我们可以使用析构赋值来设置变量的数组条目。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**