# 重构 JavaScript 逻辑以使用函数方法

> 原文：<https://javascript.plainenglish.io/refactoring-javascript-logic-to-use-a-functional-approach-fca3ae5e406?source=collection_archive---------11----------------------->

## 功能性或轻量级 JavaScript

![](img/5ebfea1cefb4cdaf39fb5e1f8d35e5ed.png)

See, it’s pretty simple…

几年前，一位非常敏锐的数学家变成了软件开发人员，他问我是否懂函数编程。“没有。”我回答。当他从座位上站起来走向白板，准备开始一个小时的即兴演讲时，我看到了他眼中的光芒。

他愤怒地在黑板上画了一串数学算式。数字上戴着尖尖的帽子，我认为是一个无穷大的符号进入了画面。有很多功能…很多功能。它们看起来都有点像这样

太神奇了！上个小时结束时，我对函数编程的了解和刚开始时差不多，但现在我非常确定自己不会在现实生活中尝试和使用它。看起来你需要一个与数学相关的四年制学位才能理解这里到底发生了什么。

“谢谢。”我回答。也许是我在整个演讲过程中尽职尽责的点头让他误以为我从中学到了什么。情况绝对不是这样。

这些年来，我会继续听到函数式编程及其好处。我觉得一定有很多数学家在写这些代码。最后，它开始点击，我在这里告诉你，你不需要成为一个函数编程纯粹主义者，以获得它的许多好处时，写 JavaScript。

虽然在人们谈论或撰写函数式编程时，可能会听到“一元数”、“一元函数”和“引用透明度”这样的术语被抛来抛去，但在编写相当容易理解和应用的代码时，有一些主要概念可以帮助您更好地处理函数式编程。

以函数式风格编写 JavaScript 的目标/好处是，代码变得更容易测试、更容易理解、隐藏 bug 的密度更小、更灵活，因此更容易重构和更可靠。

很好，你说。从何说起？

## 如果你使用了 Redux，那么你已经被介绍了函数编程背后的一个主要概念:不要变异数据！

如果你不熟悉 Redux，它是一个状态管理工具，经常与 reaction 一起使用。Redux 的一个典型使用情形是在两个组件之间共享数据，比如一个应用程序中可能需要的已登录用户信息，以显示不同的用户特定数据。

Redux 操作将触发 reducer 更新对象，但重要的是，它不会变异对象，而是返回具有更新值的新对象。

```
case 'UPDATE_USER':
const { updatedVals = {} } = action;
  return {
    ...state,
    ...updatedVals,
  };
```

突变数据的问题在于，可能还有其他逻辑依赖于数据保持其原始形状。也许你正在摄取一些实时数据，而某些小部件需要将这些信息转化为某种形状……那么，一旦这种变异的数据被其他小部件摄取，它就可能不再以它们期望的方式工作。这很糟糕。

使用 spread 操作符或`Object.assign`可以帮助你实现返回一个具有不同值和属性的对象或数组的新副本的目标。类似地，`map, reduce and filter`是返回一个带有更新值的新数组的好方法，而`forEach`通常用于更新数组中的实际值。

既然您已经避开了变化的数据，那么您需要有可靠的函数来管理您的原始数据。这些功能应该努力实现纯洁性。

## 纯函数是在给定相同输入的情况下产生相同输出的方法。

简单吧？你可能会惊讶，不遵守这条规则是多么容易，你可能会为此感到内疚。让我告诉你你的错误。

```
let shouldDisplayWelcomeMessage = false;const displayFooterElem = () => {
  return shouldDisplayWelcomMessage ? renderFooterWithMessge() : renderFooter();
}
```

我们的函数`displayFooterElem`并不纯粹，因为它依赖一个全局的、可重新赋值的变量`shouldDisplayWelcomeMessage`来确定它的输出。修复这个潜在的笨拙函数的最简单的方法是将`shouldDisplayWelcomeMessage`定义为一个常数。事实上，大多数函数式 JavaScript 只会使用常量、`Object.freeze`或`Object.seal`来确保变量、对象和数组是不可变的。

维护函数的纯洁性，也包括不产生像登录到控制台或改变数据这样的副作用，可能比看起来更困难。作为一个经验法则，保持你的功能小，并且只为一个目的负责。如果你有一个没有返回值的函数，那么这可能是一个线索，它是不纯的，因为它只会产生副作用或改变数据。

## 用你的小的、单一目的的方法，你终于准备好组合函数并达到函数编程的精通！

你可能使用过函数合成，甚至都不知道。JavaScript 将函数视为第一类成员，这意味着它们可以作为参数传递给其他函数。React 中的高阶组件利用这种能力创建灵活的组件，这些组件可能由许多更小的组件组成。类似地，如果您已经使用了带有一些异步逻辑的回调函数，那么您也已经涉足了函数合成。

函数组合基本上是将函数作为参数传递，以实现某种逻辑——我的数学朋友试图向我展示这一点，当时他在那块板上潦草地写下了`f(g(x))`。

让我们重构一些代码，这些代码检查一个`cart`对象中的商品，如果该商品不能打折，则返回一个警告，以使其更具功能性，并利用函数组合:

```
// procedural approachlet nonDiscountableItems = [];let discountableMsgs = [];const cart = {
  items: [
    {
      discountable: false,
      name: 'Item 2'
    },
    {
      discountable: false,
      name: 'Item 1'
    },
    {
      discountable: false,
      name: 'Item 3'
    }
  ]
}cart.items.forEach(item => {
  if (item.discountable == false) {
    nonDiscountableItems.push(item);
  }
});// create a list of messages for the items that are NOT discountableif (nonDiscountableItems) {
  nonDiscountableItems.map(item => {
    discountableMsgs.push(`${item.name} is not available for discount.`);
  });
}********************************************************************// more functional refactorconst isNotDiscountable = (item) => {
  return !item.discountable
}const nonDiscountMessage = (item) => {
  return `${item.name} is not available for discount.`
}const discountableMsgs = cart.items
  .filter(isNotDiscountable)
  .map(nonDiscountMessage);
```

我们重构的代码更加简洁，并且利用了一些小函数，这些小函数可以被组合来为购物车中不可打折的商品创建一个警告数组。我们减少了 bug 隐藏的表面区域，并使代码更能适应变化。

最终，当我们的项目经理基于购物车物品的一些其他属性要求警告消息时，我们可以简单地编写另一个小函数，可以与`isNotDiscountable.`交换

您可能会注意到，上面代码中编写的函数在其签名中只有一个参数。`Unary`函数是只有一个参数的函数，在函数式编程中很常见。通常，用这种约束编写一个函数可能，嗯…是有约束的。

Curried 函数可用于编写一系列一元函数，如下所示:

```
// non-curried function with multiple argumentsconst multiply = (num1, num2, num3) => {
  return num1 * num2 * num3
}********************************************************************// deliciously curried functionconst curriedMultiply = (num1) => {
  return (num2) => {
    return (num3) => {
      return num1 * num2 * num3;
    }
  }
}curriedMultiply(1)(2)(3);
//6
```

啊，令人满意。

## 您不需要成为函数式编程纯粹主义者来享受它在您的代码库中提供的好处。

不要被函数方法的教条所束缚，使用像函数组合、不变性和函数纯度这样的常识性技术来编写代码，使你的应用程序更容易阅读、测试和减少错误！