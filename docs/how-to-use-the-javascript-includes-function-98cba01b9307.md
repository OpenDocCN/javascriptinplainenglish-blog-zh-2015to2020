# 如何使用 JavaScript includes()函数

> 原文：<https://javascript.plainenglish.io/how-to-use-the-javascript-includes-function-98cba01b9307?source=collection_archive---------9----------------------->

## 这非常简单

![](img/843b7e577810af40d269e34af99eae2c.png)

*Photo by* [*Clement Souchet*](https://unsplash.com/@windclems?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *on* [*Unsplash*](https://unsplash.com/s/photos/includes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 数组方法 includes()，像许多其他的[数组方法](https://jamiepittman.com/foreach-in-javascript-how-to-effectively-use-it/)一样，做它的名字所宣称的事情:它将检查一个数组，看它是否包含一个特定的值。这个数组方法很方便，而且非常容易使用！

# includes()是做什么的？

JavaScript 数组方法 includes()将检查一个数组，看它是否包含您提供的特定值。此数组方法返回一个布尔值:如果数组包含该值，则为 true 否则为 false。

# 我如何使用 includes？

```
array.includes(valueToSearchFor, fromIndex)
```

语法相当简单。

传递给 array 方法的第一个必需参数是要在数组中搜索的值。该值区分大小写，并且正在寻找完全匹配的值。这两点都要记住！

此外，您可以选择从 Index 传递一个名为*的参数，该参数指示您希望该方法从哪个索引位置开始搜索。以我的经验，在使用数组方法 includes()时，我很少使用 *fromIndex* 。*

# includes()发挥其魔力的一个例子

via [GIPHY](https://media.giphy.com/media/laIzn1CqsmSdO/giphy.gif)

让我们从一个与 2020 年非常相关的数组开始。

```
const outOfStock = ["toilet paper", "water", "paper towels", "hand sanitizer"];
```

我们想检查 outOfStock 数组，看看是否有我们需要的目前缺货的商品。我们想知道缺货数组是否包含卫生纸？我们如何使用数组方法 includes()进行检查？

```
outOfStock.includes("toilet paper");
```

我们有语法设置，但是如果我们回想一下，数组方法 includes()返回一个布尔值(true 或 false)。我们如何获取这一价值？

我们可以在控制台中记录该值，但是最好的方法是将该语句设置为一个变量。然后，您可以根据需要使用该变量。如果您只是独自工作，请随意在控制台中记录该值，如下所示，以验证您的答案。

```
const toiletPaper = outOfStock.includes("toilet paper"); console.log(toiletPaper);

//true
```

# 需要记住的关键事项包括()

*   您在数组中检查的值区分大小写。
*   您在数组中检查的值必须完全匹配。
*   includes()返回一个布尔值
*   将响应(真/假)存储为变量

我希望你发现这是有帮助的和直接的。这里有一些关于 includes 的要点，可以帮助你在野外使用它:

我错过什么了吗？关于 includes()，您还会包括哪些内容？

*原载于 2020 年 9 月 30 日 https://jamiepittman.com**[*。*](https://jamiepittman.com/javascript-includes/)*