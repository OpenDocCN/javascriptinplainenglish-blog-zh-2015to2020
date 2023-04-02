# 清空 JavaScript 数组的 3 种方法

> 原文：<https://javascript.plainenglish.io/3-ways-to-empty-an-javascript-array-73f9032950ed?source=collection_archive---------12----------------------->

数组是为 JavaScript 开发者准备的，就像螺丝钉和钉子是为木匠准备的。因此，了解它是如何工作的是很重要的。清空数组是其中一个重要的概念，所以这里是我知道的几个方法。

![](img/38c4955dddaee0a0f30d98ec74e1a228.png)

# 1)使用长度属性

属性返回数组中元素的数量。如果我们把这个等于 0，我们就可以清空数组元素。这种方法很受欢迎，但不是完成工作的最快方法。

```
baratheon = ["Robert", "Renly", "Stannis"]baratheon.length = 0console.log(baratheon) //  expected result: []
console.log(baratheon.length) //  expected result: 0
```

# 2)将其分配给一个新的空数组

这是 ***最快的*** 清空阵列的方式。如果你没有从其他地方到原始数组的任何引用，这是完美的。如果你这样做，这些引用将不会被更新，这些地方将继续使用旧的数组。

```
baratheon = ["Robert", "Renly", "Stannis"]baratheon = []console.log(baratheon.length) //  expected result: 0
console.log(baratheon) //  expected result: []
```

# 3)使用数组方法拼接()

这可以使用 *JavaScript 数组方法*列表中的`**splice()**`方法来完成。`**splice()**`方法将*索引(*从该处开始拼接)和要移除的*项目数*作为参数，拼接元素。

我们必须将 ***0*** 作为索引*(第一个元素)*和数组的*长度作为参数传递，最终清空整个数组。该方法的性能几乎与分配新数组方法一样快。*

```
baratheon = ["Robert", "Renly", "Stannis"]baratheon.splice(0, baratheon.length)console.log(baratheon.length) // expected result: 0
console.log(baratheon) //  expected result: []
```

这就是总结！

如果你知道任何其他清空数组的方法，请在下面评论。

谢谢:)