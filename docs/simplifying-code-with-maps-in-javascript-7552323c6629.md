# 用 JavaScript 中的地图简化代码

> 原文：<https://javascript.plainenglish.io/simplifying-code-with-maps-in-javascript-7552323c6629?source=collection_archive---------15----------------------->

![](img/5f672a132f68c42c4a13110f758d4f42.png)

开发人员经常发现自己处于需要根据各种条件返回不同结果的情况。经常发生这种情况的一个具体例子是，当我们想要根据一些可以切换的状态变量在组件内部呈现不同的 JSX 时。

因此，代码通常看起来像这样:

这里有一个简单的例子，我们有一个数据卡，作为一些分析仪表板的一部分，具有预定义的样式和布局。该卡允许在`sessions`和`post`数据之间切换。唯一改变的元素是卡片图标和标题，所以引入`cardType`布尔函数是有意义的，基于它可以呈现适当的图标和标题。此外，正确类型的数据将基于此切换显示。

除了代码重复之外，这种方法还有另一个问题。让我们假设我们的组件现在有一个额外的数据类型要显示— `pageViews`。此时，作为第一步，我们需要将切换按钮重构为可用类型的下拉列表。接下来，我们可以引入一个`switch`语句来代替冗长的`if/else`条件。因此，更新后的组件将如下所示:

代码看起来不那么重复，如果我们需要显示更多类型的数据，很容易添加新的`case`和下拉选项。然而，我们仍然可以做得更好。如果我们可以根据`dataType`的值从某种配置对象中获取`title`和`Icon`会怎么样？听起来我们需要数据类型和组件变量之间的某种映射。这就是我们可以使用数据结构的地方。

[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) 是 ES6 的加法，只是一个键值对的集合。历史上在 JS 中，对象被用于存储这样的字典对，然而 Map 比对象有一些优势。Map 通过插入键来保持键的顺序，而对于对象则不是这样，因为对象的顺序没有保证。
2。Map 可以有任何值作为它的键，而对于对象，它只有字符串和符号。
3。映射可以直接迭代，而对象在大多数情况下需要在此之前进行某种转换(例如使用`Object.keys`、`Object.values`或`Object.entries`)。
4。同样，使用`size`道具可以轻松确定地图的大小。必须使用上述方法之一将对象转换为数组。
5。在频繁添加/删除操作的情况下，Map 具有一定的性能优势。

现在我们已经熟悉了地图，让我们重构我们的组件来利用这个数据结构。

请注意，在将`switch`重构为地图之后，组件变得更加精简了。起初，这个地图可能看起来有点奇怪，看起来像一个多维数组。第一个元素是键，第二个元素是值。因为键和值可以是任何东西，所以我们将数据类型映射到数组，其中第一个元素是 title，第二个元素是 icon 组件。通常从这个嵌套数组中获取这两个值需要一点工作，但是[析构赋值](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)语法使它变得很容易。这种语法的另一个好处是，我们可以给我们的变量起任何名字，这在我们想把`title`或`Icon`改名为其他名字时很方便，而不用修改映射本身。贴图是在组件外部声明的，因此不会在每次渲染时不必要地重新创建。

既然我们这样做了，为什么不把下拉选项的数组也重构为一个地图呢？选项只是值和标签之间的映射，这是一个完美的映射用例！

由于 Map 没有`map`方法，所以需要先转换成数组。这可以通过使用[阵列排列](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Spread_in_array_literals)或[阵列来完成。这里我们再次受益于析构赋值，所以我们可以很容易地在 map 方法的回调中访问`label`和`value`，然后用这些键和它们的值创建一个对象。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

最终的结果看起来非常简洁和易于维护，我们只需要对地图做一些修改，以防添加更多的日期类型。

*最初发表于*[*https://claritydev.net*](https://claritydev.net/blog/simplifying-code-with-maps-in-javascript/)*。*