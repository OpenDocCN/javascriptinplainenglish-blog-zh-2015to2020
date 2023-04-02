# 为什么我们仍然需要在 React 中使用状态管理库？

> 原文：<https://javascript.plainenglish.io/why-we-still-need-to-use-state-management-libraries-in-reactjs-1e53bd18dab5?source=collection_archive---------2----------------------->

## 为什么使用 just React 的状态机制不是一个好主意

![](img/35e159b00964fa9051b6348227e20be8.png)

Photo by [Guillaume Bolduc](https://unsplash.com/@guibolduc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

现在我们有了`hooks`和`Context API`，在 React 中开始一个新项目时放弃一个状态管理库似乎很有吸引力——我们应该屈服于这种诱惑吗？让我们通过查看一个简单的购物车示例来找出答案，从树的底部开始——item list 视图。

# 在底部

一个上下文消费者组件看起来像这样，当然，这是一个非常简单的例子，但是它很好地说明了主要的兴趣点。它只是呈现一个商品列表，可以在购物车中添加或删除商品。

```
**const** SimpleItemList = () **=>** {
    // Using the hook to gain access to the context
    **const** context = useContext(GlobalContext);

    return (
        <div>
        {context.items.map((item, index) **=>** {
            return (
                <Item
                    key={index}
                    item={item}
                    onAdd={() **=>** {
                        context.addToCart(item);
                    }}
                    onRemove={() **=>** {
                        context.removeFromCart(item);
                    }}
                />
            );
        })}
        </div>
    );
};
```

起初，它似乎很好，因为它不需要传入任何道具，只需要直接从上下文中获取我们需要的一切。

不过这也有一些缺点:

*   组件重用是有害的，因为它直接与上下文相关联
*   乍一看，很难推断这个组件需要什么变量，因为没有道具

我们可以传入上下文作为组件的道具，使其更具可重用性。

```
**const** context = useContext(props.context);
```

如果我们要使用 Typescript，我们可以指定组件的 props 扩展某种描述我们期望的上下文外观的基本接口，但我不建议这样做，因为这看起来像一个黑客，如果情况足够糟糕，这种选择是存在的。

# 在顶部

我们定义我们的应用程序状态，以及处理它所必需的函数，并使用`Context API`将它暴露给我们的其他组件。

为了使用`Context API`，我们需要用上下文提供者组件包装我们的应用程序，以便需要它的孩子可以访问我们的上下文。

```
**function** SimpleStateApp() {
    **const** addToCart = item **=>** {
        setState(prevState **=>** {
            **const** copy = { ...prevState };
            if (copy.cart[item.id]) {
                copy.cart[item.id].count++;
            } else {
                copy.cart[item.id] = { ...item, count: 1 };
            }
            return {
                ...copy
            };
        });
    };**const** removeFromCart = item **=>** {
        setState(prevState **=>** {
            **const** copy = { ...prevState };
            if (copy.cart[item.id].count === 1) {
                delete copy.cart[item.id];
            } else {
                copy.cart[item.id].count--;
            }
            return {
                ...copy
            };
        });
    };**const** [state, setState] = useState({
        addToCart: addToCart,
        removeFromCart: removeFromCart,
        items: [...ITEMS_ARRAY],
        cart: {}
    });return (
        <GlobalContext.Provider value={state}>
            <div className="App">
                <header className="App-header">
                    <div>This is the Simple barn</div>
                </header>
                <SimpleItemList />
                <SimpleCart />
            </div>
        </GlobalContext.Provider>
    );
}
```

它就像一个魔咒！尽管它暴露了一些事情:

*   这仍然是单个组件的局部状态，与其紧密耦合
*   增加业务逻辑的复杂性会增加根组件的复杂性

# 结论

我让它看起来比实际情况更糟，因为有很多方法可以避免将所有东西都直接放入根组件中，状态函数可以很容易地移动到单独的文件中，只需在根组件中导入，但我认为最好使用一个适当的状态管理库来将全局状态从任何单个组件中分离出来，使组件尽可能简单，只需要它们运行时的状态。

在这个 [repo](https://github.com/MustSeeMelons/react-state-party) 中，你可以查看同一个应用的多种状态管理方式。