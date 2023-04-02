# 如何在 React 中循环数组

> 原文：<https://javascript.plainenglish.io/how-to-loop-through-arrays-in-react-3eaa8a14445?source=collection_archive---------1----------------------->

![](img/8933e4fa3854e49902a096a7f2866351.png)

在构建任何类型的 web 应用程序时，通常都需要处理一组数据。在本文中，让我向您展示如何使用 React 最佳实践遍历一组数据，使用您可以在自己的 web 应用程序中使用的真实示例。

## 你以前用过地图功能吗？

**map()** 方法创建一个新数组，其结果是对调用数组中的每个元素调用一个提供的函数。

所以在 ES2015 中 JavaScript 引入了地图功能。它极大地简化了循环过程，不再需要使用简单的 for 循环或 forEach 函数。现在，for 循环、forEach 和 map 之间有一些区别，但我们不会在这里深入讨论。React 文档强烈鼓励使用 map 函数，不仅仅是因为它简单，还因为它从数据中创建一个新数组，而不是试图改变/覆盖现有数据。

第二点至关重要。如果你不确定如何使用地图功能，我建议你花一点时间阅读这篇关于使用[地图，过滤和减少](https://medium.com/javascript-in-plain-english/learn-how-to-use-map-filter-and-reduce-in-5-minutes-fbf5c5540b2f)的文章。

*喜欢这篇文章吗？如果有，获取更多类似内容通过* [***订阅解码，我的 YouTube 频道***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) ***！***

## 遍历购物车中的一组商品

让我们想象一下，我们正在建立一个销售服装的电子商务网站。我们的应用程序可能会有一个购物车，商品被添加到其中。首先，我们的购物车看起来会像这样。

`let shoppingCart = [];`

但是在添加了一些项目之后，它可能看起来像这样:

```
let shoppingCart = [{id: 35, item: 'jumper', color: 'red', size: 'medium', price: 20},{id: 42, item: 'shirt', color: 'blue', size: 'medium', price: 15},{id: 71, item: 'socks', color: 'black', size: 'all', price: 5},]
```

现在，在我们的购物车页面上，我们将希望输出这些商品，以便用户可以看到他们订购了什么。让我们来看两种非常相似的处理方式。

在我们的 App 类构造函数中，我们将创建 this.items 并将其分配给`this.state.cart`(记住，这是我们存储添加到购物车中的项目的地方)。然后，我们使用 ES2015 map 函数遍历 this.state.cart 中的每个项目。

我们向映射函数传递两个参数。第一个是 item，它简单地对应于我们的`this.state.cart`数组中的一个条目。第二个是一个键，React 使用它来帮助渲染器跟踪每个单独的项目。正如您可能已经注意到的，我们没有在我们的`this.state.cart`中设置一个`key`，我们只是通过引用我们的 map 函数中的`item.id`来传递项目的 id 作为键。无论如何，我们输出一个带有`{item.name}`的`<li>` 标签。

让我们快速地看一下这段完整的代码:

```
**this**.items = **this**.state.cart.map((item) =>
    <li key={item.id}>{item.name}</li>
);
```

然后在稍后的代码中，当我们想要使用它时，我们简单地创建一个外部的`<ul>`标签，并将 this.items 放入其中，就像这样:

```
<ul>
    {this.items}
</ul>
```

就像这样，我们的页面会将购物车中的每个商品呈现为一个唯一的`<li>`商品。

我们处理这个问题的第二种方式非常相似，可能感觉更熟悉一些。

我们可以简单地将 this.item 代码块放在 **render()** 函数的顶部，而不是放在类构造函数中，就像这样:

```
render() {
    **const** items = **this**.state.cart.map((item) =>
        <li key={item.id}>{item.name}</li>
    );
```

这允许我们使用更传统的 const 语法(或者 var 或 let，如果您愿意的话)。

如果我们这样做，也意味着我们可以这样写:

```
<ul>
    {items}
</ul>
```

现在有些人会争辩说，你不应该污染`render()`函数，但是当编写一些相当简单并且不复杂得令人难以置信的东西时(我在这里说的是成千上万行代码)，这真的无关紧要。所以我建议你选择你觉得更舒服的方式。随着 React 的进步，您将会找到最适合您的项目的风格。

好了，我们知道了如何使用循环来呈现列表。但是，如果我们想通过数据循环输出大量的子组件呢？我们如何做到这一点？

嗯，这实际上很简单，与我们已经在做的事情非常相似。

让我们假设我们的商店有以下商品:

```
shop: [
    {id: 35, name: 'jumper', color: 'red', price: 20},
    {id: 42, name: 'shirt', color: 'blue', price: 15},
    {id: 56, name: 'pants', color: 'green', price: 25},
    {id: 71, name: 'socks', color: 'black', price: 5},
    {id: 72, name: 'socks', color: 'white', price: 5},
]
```

为了让你看得更清楚，这个商店就在这个州的一个建筑商里面。看起来都是这样的:

```
**class** App **extends** Component {
    constructor(props) {
        **super**(props);
        **this**.state = {
            cart: [],
            shop: [
                {id: 35, name: 'jumper', color: 'red', price: 20},
                {id: 42, name: 'shirt', color: 'blue', price: 15},
                {id: 56, name: 'pants', color: 'green', price: 25},
                {id: 71, name: 'socks', color: 'black', price: 5},
                {id: 72, name: 'socks', color: 'white', price: 5},
            ]
        }
    }
```

所以我们有了我们的商店项目，现在我们想创建一个子组件，我们称之为 **Item.js** 。整个组件文件如下所示:

```
**import** React, {Component} **from** 'react';

**class** Item **extends** Component {
    render() {
        **return** (
            <div>
                <p>{**this**.props.item.name}</p>
                <p>{**this**.props.item.color}</p>
                <p>${**this**.props.item.price}</p>
            </div>
        );
    }
}

**export default** Item;
```

如果您不确定，我们调用`this.props.item`,因为当我们在父组件内部调用 this.state.item 时，我们会将其传递给子组件。这听起来可能有点奇怪，但是请耐心等待，一会儿就会明白了！

所以回到循环，这就是我们要写的在商店中循环商品的代码:

```
<h3>Items for sale</h3>{**this**.state.shop.map((item) =>
    <Item key={item.id} item={item}/>
)}
```

如您所见，这与我们之前使用的概念非常相似。这里唯一的不同是，我们现在输出的不是`<li>`标签，而是`<Item />`标签——这就是我们刚才提到的 **Item.js** 组件！就这么简单！

所以当我们在 **Item.js** 组件内部调用 this.props.item 时，是对`<Item key={item.id} item={item}/>`的引用，更确切的说是对`item={item}`的引用。因此，如果我们称之为`product={item}`，我们将改为`this.props.product`。有道理吗？

您可能已经注意到，我们不得不将全部功能放在花括号{}中。这是必需的，以便 React 能够理解您试图表达一个函数，而不仅仅是呈现纯文本。

这里还要注意的另一点是，我们循环数据的三种不同方式都可以互换。因此，如果您想遍历子组件的数组，但更喜欢我们向您展示的第一种方法，那么您可以这样做！说到底都是正规的 JavaScript！

现在，我们已经向您展示了使用 React 中最新的 JavaScript 技术遍历数组的三种强大方法。现在去使用这些新发现的知识来制作一些令人惊奇的 web 应用程序吧！

*更多内容敬请关注*[***plain English . io***](http://plainenglish.io)**和上**[**我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！****

**[](https://newsletter.plainenglish.io/) [## 上周简明英语杂志

### 《上周简明英语》——科技世界的每周综述，包含我们认为你会…

时事通讯. plainenglish.io](https://newsletter.plainenglish.io/)**