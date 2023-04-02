# 理解 D3.js 中的动画图形

> 原文：<https://javascript.plainenglish.io/understanding-animated-graphs-in-d3-js-67488accb467?source=collection_archive---------12----------------------->

[D3.js](https://d3js.org/) 是一个伟大的，在我看来非常优雅的库，用于在 web 浏览器中构建可视化。不幸的是，有些概念不太容易从文档中理解，尤其是在制作动画图表时。当我试图使用[官方资源](https://github.com/d3/d3/wiki)向我的学生教授这些概念时，我自己也有过这样的经历。

我确信 [ObservableHQ](https://observablehq.com/@d3/learn-d3) 是修补和学习某些特性的好方法，但是它的灵感来源于自动更新的电子表格，这使得它很难理解这些特性在现实世界的应用程序中是如何组合的。

因此，我将尝试用一个非常简单的例子来解释我(也可能是其他人)在学习 D3.js 时遇到的困难。这不会涵盖 D3.js 的很多不同特性，但会解释我觉得最难理解的部分。本指南在 D3.js 主页上的[介绍中增加了一些内容，这会让我更容易学习 D3.js。](https://d3js.org/)

*下面的例子假设* `*d3*` *变量和一个* `*<svg>*` *标签在作用域内。这可以通过使用 D3.js 文档* *中描述的脚本标签* [*来完成。*](https://github.com/d3/d3/wiki#installing)

```
<svg />
<script src="https://d3js.org/d3.v5.js"></script>
```

# 构建一个简单的图表

基本上 D3.js 允许你基于数据生成不同的标记(比如 HTML 或者 SVG)。让我们来看一个例子，它将数组中的数字可视化为不同大小的圆圈。

```
**const** data **=** [10, 30, 20];d3.select('svg').selectAll('circle')
    .data(data)
        .join(
            (enter) **=>** enter
                .append('circle')
                    .style('fill', 'red')
                    .attr('r', (d) **=>** d)
                    .attr('cx', (d, i) **=>** (i **+** 1) ***** 50)
                    .attr('cy', 50)
        )
```

`data`变量保存我们想要可视化的数据。为此，我们需要`[d3.select](https://github.com/d3/d3-selection#select)` [函数](https://github.com/d3/d3-selection#select)，它返回一个 [D3.js 选择](https://github.com/d3/d3-selection)。这样的选择充当 DOM 的包装器，它侧重于使用数据的大规模操作。`d3.select`函数返回一个选择，其中第一个元素匹配其传递的 CSS 选择器，因此是我们的 HTML 文档中唯一的`<svg>`标签。D3.js 利用了一个 [fluent 接口](https://www.martinfowler.com/bliki/FluentInterface.html)，使我们能够使用刚刚返回的选择，并进行另一个[函数调用](https://github.com/d3/d3-selection#selectAll) `[selectAll](https://github.com/d3/d3-selection#selectAll)`。这将返回所有的`[cirlce](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle)` [元素](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle)，它们是我们文档中第一个`<svg>`标签的后代。这感觉有点奇怪，因为这会导致一个空的选择，但是我们马上就会看到这一点。

所以使用`d3.select('svg').selectAll('circle')`调用，我们有一个空的选择，因为在`<svg>`标签中还没有圆。现在我们可以使用`[data](https://github.com/d3/d3-selection#selection_data)` [函数](https://github.com/d3/d3-selection#selection_data)给我们的`data`变量赋值。这将给我们一个数据选择，知道哪些元素我们必须添加到我们的可视化。这就是开始变得有趣的地方。

这样的数据选择提供了一个`[join](https://github.com/d3/d3-selection#selection_join)` [方法](https://github.com/d3/d3-selection#selection_join)，最多可以接受三个参数。在上面的例子中，只使用了一个参数，是一个接受`enter`选择的函数。这个选择保存了我们的数据集(`10`、`30`和`20`)中当前没有绑定到 SVG `circle`元素的所有值。因为还没有元素存在，所以我们的工作就是创建它们。这就是`[append](https://github.com/d3/d3-selection#selection_append)` [方法](https://github.com/d3/d3-selection#selection_append)的作用。我们用`'circle'`参数调用它，这意味着一旦调用结束，将有三个`circle`元素，每个元素对应一个丢失的数据点。

然后在这些圆上使用`[style](https://github.com/d3/d3-selection#style)`和`[attr](https://github.com/d3/d3-selection#selection_attr)`调用，以便以某种方式设计它们。查看 MDN 上的`[circle](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle)` [文档，以了解可用的属性。这些函数不仅接受简单的值；它们也接受函数作为参数。**如果一个函数通过了，这个函数将被当前数据点的值(** `**d**` **变量)和索引(** `**i**` **变量)调用。**这样我们可以将半径`r`设置为传递的值，并使用索引向右移动圆来计算`cx`属性。](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle)

通过这几行代码，我们已经创建了三个半径分别为 10、30 和 20 像素的红色圆圈。

# 更新当前选择

到目前为止，我们只有三个值作为圆的静态表示。到目前为止这并不有趣。我们可以自己简单地编写几行 SVG 来实现相同的结果。D3.js 的流行来自于它更高级的特性。让我们看看当一个新的数据集到达时，我们如何改变数据。开始时，我想知道这是如何工作的，因为从文档中看不出来。**基本上，诀窍是用不同的数据集再次调用完全相同的代码。然后，D3.js 将找出哪些元素已经存在，哪些必须被添加，以及是否有任何元素必须被删除。我们唯一要做的事情(除了再次调用该代码之外)是告诉 D3.js 它应该如何处理这些不同的元素集。**

我们通过向`join`方法传递更多的函数参数来做到这一点。第三个参数处理即将被移除的元素，第二个基于`selectAll`调用处理之前已经存在的元素，这解释了为什么它从一开始就在上面的代码中。我们将把上面的整个 D3.js 代码放到一个单独的函数中，当新数据到达时将再次调用这个函数。所以`selectAll`只会在第一次调用时返回一个空集，但是它的连续调用会返回旧的`circle`元素。

下面的代码展示了这个概念，并假设您的 HTML 中有一个`<button>`标签，当它被点击时将加载下一个数据集(为简洁起见，没有越界检查):

```
**const** data **=** [
    [10, 30, 20, 40],
    [20, 20, 20],
    [40, 10, 20, 10, 20],
    [20, 10, 20],
];**let** currentIndex **=** 0;document.getElementsByTagName('button')[0].addEventListener(
    'click',
    () **=>** update(**++**currentIndex)
);**function** update(index) {
    d3.select('svg').selectAll('circle')
        .data(data[index])
            .join(
                (enter) **=>** enter.append('circle'),
                (update) **=>** update,
                (exit) **=>** exit.remove(),
            )
                .style('fill', 'red')
                .attr('r', (d) **=>** d)
                .attr('cx', (d, i) **=>** (i **+** 1) ***** 50)
                .attr('cy', 50);
}update(0);
```

`data`变量现在包含一个数组的数组，因此内部数组被解释为一个数据集。我们有一个以`0`开头的索引变量，每次点击文档的第一个按钮都会调用`update`函数，增加索引的值。`update`函数包含 D3.js 特定代码，但也使用`index`参数访问当前数据集。如前所述，`join`方法与 3 个回调一起使用:

1.  `**enter**` **回调**用于当前未在可视化中表示的数据点。在这里，`append`方法用于为每个新数据点添加一个`circle` SVG 元素。
2.  对于已经在可视化中表示的数据点，调用`**update**` **回调**。目前我们没有对这些元素做任何特殊的处理，所以我们只是返回那个集合。
3.  对于在新数据集中不再有数据点的可视化中的元素，调用`**exit**` **回调**。这个例子使用`remove`方法立即删除这些元素。

*如果你熟悉*[*React*](https://reactjs.org/)*，你可以想到 D3.js 做着类似的工作:你只需要告诉它在每种情况下应该发生什么，D3.js 就会为你计算出何时调用哪个函数。*

因为除了为`enter`集合添加新的圆之外，我们经常想要以同样的方式处理`enter`和`update`集合，`join`方法将合并这两个集合并返回两者的组合。所以`join`方法的返回值可以用来设置这两个集合的样式和其他属性(我们也可以直接在`enter`和`update`回调中添加后续的方法调用，这通常是在这些集合之间的处理不同时完成的)。这部分代码是从第一个例子中复制过来的。

# 为更平滑的动画添加过渡

前面的例子改变了每一次按钮点击的可视化，但是它仍然不像一个复杂的可视化。D3.js 还支持过渡，这将使动画更加流畅。实现这一点的最重要的方法叫做`[transition](https://github.com/d3/d3-transition#selection_transition)`，它返回一个类似于 D3.js 选择的对象。**调用** `**transition**` **后，您可以使用前面描述的** `**attr**` **和** `**style**` **方法，但这些更改将会被激活，而不是立即应用。**为了控制动画的速度，这个类似选择的对象还包含了`[duration](https://github.com/d3/d3-transition#transition_duration)` [方法](https://github.com/d3/d3-transition#transition_duration)。为了延迟动画，可以使用`[delay](https://github.com/d3/d3-transition#transition_delay)` [方法](https://github.com/d3/d3-transition#transition_delay)。这两个函数都接受一个被解释为毫秒的参数。

有一个非常重要的旁注:**一个过渡对象，尽管相似，但不是 D3.js 选择。**如果传递的是过渡而不是选择，D3.js 可能会抛出错误。因此`[call](https://github.com/d3/d3-selection#selection_call)` [方法](https://github.com/d3/d3-selection#selection_call)存在。`call`方法将执行传递的函数，但不是返回传递的函数返回的值，而是总是返回执行`call`方法的选择。通过这种方式，方法链可以被分成几个部分，而不会创建使代码膨胀的独立变量。参见下面的例子，它只是显示了上面的`update`函数的新主体:

```
d3.select('svg').selectAll('circle')
    .data(data[index])
        .join(
            (enter) **=>** enter.append('circle')
                .attr('r', 0)
                .attr('cy', 50),
            (update) **=>** update,
            (exit) **=>** exit.call((exit) **=>** exit
                .transition()
                    .duration(500)
                        .attr('r', 0)
                        .remove()
            )
        )
            .transition()
                .duration(500)
                    .attr('r', (d) **=>** d)
                    .attr('cx', (d, i) **=>** (i **+** 1) ***** 50)
```

这几乎与上面的代码相同，但是为了使数据集之间的转换具有动画效果，做了三个关键的更改:

1.  **后称** `**transition**` **法为** `**join**` **法。**这样我们告诉`join`方法的`enter`和`update`集合的合并，后续的`attr`调用应该被动画化，持续时间为 500 毫秒
2.  **`**join**`**方法的** `**enter**` **集合立即得到一个** `**r**` **和** `**cy**` **值。**这些值是动画前的起始值。立即设置`cy`并保持不动将导致圆圈仅在 x 轴上移动。**
3.  ****`**join**`**方法的** `**exit**` **集合也使用** `**transition**` **和** `**duration**` **调用。**在`transition`方法后再次调用`attr`方法，这将使 cirlce 缩小直到不再可见。过渡的`remove`将在动画结束后移除元素(与标准选择的`remove`方法相反，后者会立即移除元素)。虽然对于`exit`设置来说不是必需的(因为它不会被进一步操作)，但是为了返回正确的选择而不是从第三次回调的转换，已经使用了`call`方法。****

# ****使用键识别数据点****

****这已经形成了一个相当光滑的可视化！但是另一个重要的部分不见了:圈子目前没有任何身份。这意味着，如果传递的数字比以前多，则差值被传递到`enter`集合，如果传递的数字少，则差值被传递到`exit`集合。但在许多情况下，这还不够。想象一下选举的可视化:可能有两个新的政党，一个政党已经不存在了，所以在`enter`和`exit`集合中应该有一些元素。为了使这成为可能，我们必须能够以某种方式识别数据。这也是为什么 `**data**`中的关键功能**方法被引入的原因。******

****我们将使用上面的代码来显示不同选举的结果。我知道条形图是更好的可视化方法，但是我们将再次使用圆圈，所以请耐心等待。为了让 D3.js 能够识别这个圆，并允许它移动现有的圆，而不是让它们消失和再次出现，我们将使用作为第二个参数传递给`data`方法的`key`函数。我们也不再使用普通数字作为数据，而是使用由一个`fill`和一个`value`属性组成的对象。`value`替换之前的数字，`fill`属性描述圆的颜色，作为圆的标识符。让我们看一下代码:****

```
****const** data **=** [
    [
        {fill: 'green', value: 30},
        {fill: 'red', value: 18},
        {fill: 'blue', value: 9},
    ],
    [
        {fill: 'black', value: 35},
        {fill: 'red', value: 27},
        {fill: 'green', value: 18},
        {fill: 'pink', value: 3},
    ],
    [
        {fill: 'red', value: 30},
        {fill: 'green', value: 15},
        {fill: 'black', value: 14},
        {fill: 'pink', value: 6},
    ],
];**let** currentIndex **=** 0;document.getElementsByTagName('button')[0].addEventListener(
    'click',
    () **=>** update(**++**currentIndex)
);**function** update(index) {
    d3.select('svg').selectAll('circle')
        .data(data[index], (d) **=>** d.fill)
            .join(
                (enter) **=>** enter.append('circle')
                    .style('fill', (d) **=>** d.fill)
                    .attr('r', 0)
                    .attr('cy', 50),
                (update) **=>** update,
                (exit) **=>** exit.call((exit) **=>** exit
                    .transition()
                        .duration(500)
                            .attr('r', 0)
                            .remove()
                )
            )
                .transition()
                    .duration(500)
                        .attr('r', (d) **=>** d.value)
                        .attr('cx', (d, i) **=>** (i **+** 1) ***** 50)
}update(0);**
```

****除了一些我想强调的小差异之外，它看起来与我们之前的产品非常相似:****

1.  ******`**data**`**变量如前所述已经改变。**它是数组的数组，内部数组包含颜色为`fill`和`value`的对象。选举的结果可能以类似的方式表示。******
2.  ******`**data**`**函数传递第二个参数，这个参数称为** `**key**` **函数。**我们返回`data`变量的`fill`值，该值将被用作圆的标识符。******
3.  ******中的** `**transition**` **后的** `**join**` **调用的** `**attr**` **调用的** `**r**` **必须进行改编**，因为数据点现在是一个对象而不是一个数字。****

****现在 D3.js 能够正确地将圆分配给`enter`、`update`和`exit`组。当点击不同的数据点时，圆圈现在应该保持它们的颜色并四处移动。如果操作不正确，圆圈会改变颜色，而不是移动正确的位置。这可能会很容易地挫败这种可视化的目的，因为数据发生了什么对于查看者来说是不清楚的。****

# ****结论****

****希望以上几点能帮助别人比我更快地理解 D3.js。**最后，我想说的是，在许多情况下，使用一个简单的图表库可能会更容易，但这些库并不强大，你可能很快就会走进死胡同。一旦你理解了 D3.js，你也可以很快地开发简单的条形图和折线图，同时如果需要的话，还可以将你的图表变得更加复杂。还有一个额外的好处:可视化数据很有趣！******

# ****简单英语的 JavaScript****

****喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！******