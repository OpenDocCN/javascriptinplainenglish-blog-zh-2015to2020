# 将 GraphQL 与 React 一起使用

> 原文：<https://javascript.plainenglish.io/using-graphql-with-react-94c609cfbe52?source=collection_archive---------5----------------------->

![](img/b9c9c231f633bc616c940bf21baa716c.png)

在本教程中，我们将使用 GraphQL 和 React。GraphQL 是什么？GraphQL 是一种用于 API 的查询语言，也是完成这些查询的运行时。GraphQL 为您的 API 中的数据提供了一个完整而清晰的描述，为客户提供了要求他们所需要的东西的能力，仅此而已。它还使 API 随着时间的推移更容易发展，并激活强大的开发工具。然而，在本教程中，您将只学习如何在前端实现 GraphQL。让我们开始吧。

# 创建 React 项目

让我们创建一个电子商务应用程序

```
create-react-app ecommerce
```

接下来，导航到您的项目文件夹并启动本地开发服务器:

```
cd ecommerce
npm start
```

它将在`[http://localhost:3000](http://localhost:3000)`开始运行

# 安装 Apollo 客户端

Apolo client 是一个丰富的数据管理解决方案，包含了我们需要的所有基本东西。值得注意的是，Apolo 提供了缓存，使其成为应用程序中本地和远程数据的单一来源。听起来像 Redux 生态系统权利“单一来源的真理”？以下是我们将要使用的软件包:

*   [graph QL](https://www.npmjs.com/package/graphql):graph QL 的 JavaScript 参考实现
*   [react-apollo](https://www.npmjs.com/package/react-apollo) :这个包允许你从 GraphQL 服务器获取数据，并使用它来开发复杂的反应式用户界面。这个包主要用于 React。
*   Apollo Boost : Apollo Boost 是一种开始使用 Apollo 客户端的零配置方式。它包括一些合理的默认设置，比如我们推荐的`InMemoryCache`和`HttpLink`，它们是按照推荐的设置为您配置的。

打开一个新的终端，导航到您的项目文件夹，然后运行以下命令:

```
npm install graphql --save
npm install react-apollo --save
npm install apollo-boost --save
```

现在我们需要在我们的**电子商务**应用程序中配置它们。

让我们从 **index.js** 文件开始，添加以下代码:

```
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter } from "react-router-dom";
import { ApolloProvider } from "react-apollo";
import { createHttpLink } from "apollo-link-http";
import { InMemoryCache } from "apollo-cache-inmemory";
import { ApolloClient } from "apollo-boost";
import "./index.css";
import { default as App } from "./App/App.container";
import { resolvers, typeDefs } from "./graphql/resolvers";
import { default as data } from "./graphql/initial-data";const httpLink = createHttpLink({
  uri: "URL goes here"
});const cache = new InMemoryCache();const client = new ApolloClient({
  link: httpLink,
  cache,
  typeDefs,
  resolvers
});client.writeData({ data });ReactDOM.render(
  <ApolloProvider client={client}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </ApolloProvider>,
  document.getElementById("root")
);
```

我们已经在内存缓存中创建了**的实例，然后创建了 **HttpLink** 的实例，并传入了我们的 GraphQL API URI。然后我们创建了一个 **ApolloClient** 的实例，并提供了缓存和链接实例。**

# 将 Apollo 客户端连接到 React 组件

在创建了 **ApolloClient** 的实例之后，我们需要将它连接到我们的 React 组件。让我们从最简单的场景开始，我们需要获取数据。

**通过查询获取数据**

```
import React from "react";
import { Query } from "react-apollo";
import { gql } from "apollo-boost";
import CategoriesOverview from "./categories-overview.component";
import Spinner from "../spinner/spinner.component";
const GET_CATEGORIES = gql`
  {
    categories {
      id
      title
      items {
        id
        name
        price
        imageUrl
      }
    }
  }
`;
const CategoriesOverviewContainer = () => (
  <Query query={GET_CATEGORIES}>
    {({ loading, data }) => {
      if (loading) return <Spinner />;
      return <CategoriesOverview categories={data.categories} />;
    }}
  </Query>
);
export default CategoriesOverviewContainer;
```

我们只是使用查询来获取数据。如您所见，我们为想要显示的项目选择了特殊数据。并使用一个微调组件，该组件将一直显示，直到显示数据。

**变异数据**

是时候更进一步，看看我们如何改变数据了。让我们创建一个 graphql 文件夹并创建 2 个文件。

**InitData.js**

```
const INITIAL_DATA = {
  cartHidden: true,
  cartItems: [],
  itemCount: 0,
  cartTotal: 0
};
export default INITIAL_DATA;
```

在这里，我们可以为可变数据设置初始值。让我们创建另一个文件，该文件将包含一个用于向购物车添加商品的可重用函数。这种方法将使我们的代码更加整洁。

**utils.js**

```
export const addItemToCart = (cartItems, cartItemToAdd) => {
    const existingCartItem = cartItems.find(
        cartItem => cartItem.id === cartItemToAdd.id
    );
    if (existingCartItem) {
        return cartItems.map(cartItem =>
            cartItem.id === cartItemToAdd.id ? {…
                cartItem, quantity: cartItem.quantity + 1
            } : cartItem
        );
    }
    return […cartItems, {…
        cartItemToAdd, quantity: 1
    }];
};
```

如您所见，这是一个简单的函数，可用于向购物车添加新商品。是时候再次使用 GraphQL 了:
让我们创建另一个名为:

**resolvers.js**

```
import { gql } from "apollo-boost";
import { addItemToCart } from "./utils";
export const typeDefs = gql`
  extend type Item {
    quantity: Int
  }
  extend type Mutation {
    ToggleCartHidden: Boolean!
    AddItemToCart(item: Item!): [Item]!
  }
`;
const GET_CART_HIDDEN = gql`
  {
    cartHidden [@client](http://twitter.com/client)
  }
`;
const GET_CART_ITEMS = gql`
  {
    cartItems [@client](http://twitter.com/client)
  }
`;
const updateCartItemsRelatedQueries = (cache, newCartItems) => {
  cache.writeQuery({
    query: GET_CART_ITEMS,
    data: { cartItems: newCartItems }
  });
};
export const resolvers = {
  Mutation: {
    toggleCartHidden: (_root, _args, { cache }) => {
      const { cartHidden } = cache.readQuery({
        query: GET_CART_HIDDEN
      });
      cache.writeQuery({
        query: GET_CART_HIDDEN,
        data: { cartHidden: !cartHidden }
      });
      return !cartHidden;
    },
    addItemToCart: (_root, { item }, { cache }) => {
      const { cartItems } = cache.readQuery({
        query: GET_CART_ITEMS
      });
      const newCartItems = addItemToCart(cartItems, item);
      updateCartItemsRelatedQueries(cache, newCartItems);
      return newCartItems;
    }
  }
};
```

让我们看看这里发生了什么？
首先，我们定义了购物车商品和购物车本身的类型。就您所知，ToggleCartHidden 类型为布尔型，可以打开或关闭购物车下拉列表。你可能会认为这种语法很奇怪。并且对新术语感到困惑，可能会问什么是@client，什么是 resolver…？
**@client** 是一个指令，它告诉 Apollo 客户机在本地获取字段数据(从缓存或使用本地解析器)，而不是将其发送到我们的 GraphQL 服务器。

**解析器**提供将 GraphQL 操作(查询、变异或订阅)转换成数据的指令。它们要么返回我们在模式中指定的相同类型的数据，要么返回对该数据的承诺。

**readQuery** 方法使您能够直接在缓存上运行 GraphQL 查询。

如果您的缓存包含完成指定查询所需的所有数据，readQuery 将返回查询形状的数据对象，就像 GraphQL 服务器一样。

如果您的缓存没有包含完成指定查询所需的所有数据，readQuery 将抛出一个错误。它从不尝试从远程服务器获取数据。

**writeQuery** 顾名思义就是用来更新数据的。

因此，我们使用这种新方法向购物车添加新商品，并切换购物车本身。你可以在这里添加其他功能，从购物车中删除商品，更新购物车商品计数等等……
希望现在一切都清楚了。现在让我们看看如何将它与**购物车下拉列表**组件一起使用。

```
import React from "react";
import { Query, Mutation } from "react-apollo";
import { gql } from "apollo-boost";
import CartDropdown from "./cart-dropdown.component";const TOGGLE_CART_HIDDEN = gql`
  mutation ToggleCartHidden {
    toggleCartHidden [@client](http://twitter.com/client)
  }
`;
const GET_CART_ITEMS = gql`
  {
    cartItems [@client](http://twitter.com/client)
  }
`;
const CartDropdownContainer = () => (
  <Mutation mutation={TOGGLE_CART_HIDDEN}>
    {toggleCartHidden => (
      <Query query={GET_CART_ITEMS}>
        {({ data: { cartItems } }) => (
          <CartDropdown
            cartItems={cartItems}
            toggleCartHidden={toggleCartHidden}
          />
        )}
      </Query>
    )}
  </Mutation>
);
export default CartDropdownContainer;
```

正如您在这个组件中看到的，我们同时使用了变异和查询。就是这样。您可以尝试自己实现我们之前创建的另一个变体，用另一个组件向购物车添加新商品。如果这很难，那就给这个教程发个评论，我会为这个教程添加第二部分。

# 结论

GraphQL 现在非常流行，但是，您应该决定在客户端使用它作为 Redux 的替代，还是继续使用 Redux？