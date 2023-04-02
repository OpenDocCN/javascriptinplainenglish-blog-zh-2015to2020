# 我的第一个全栈 Web 应用

> 原文：<https://javascript.plainenglish.io/my-first-full-stack-web-application-8ac82db61b10?source=collection_archive---------3----------------------->

![](img/81db96e63787e1f7cdcc01518a4c0992.png)

Share My Bike homepage image

在 600 多节课、实验室和 4 个文件夹项目之后，现在是时候谈谈我最后的文件夹项目了。当报名参加[熨斗学校](https://flatironschool.com/)的在线自定进度软件工程课程时，我知道要完成五个文件夹项目才能毕业。对我来说，最终项目似乎是一件遥远的事情，需要很多我还不具备的技能。但它就在那里，准备提交审查。

对于这个最终的组合项目，目标是构建一个风格化的**单页面应用程序**，它由一个 [React.js](https://reactjs.org/) 和 [Redux.js](http://Redux.js) 前端组成，调用 Ruby on Rails API。

## 单页应用程序

那么首先，什么是**单页应用**？它是一个网站或网络应用程序，通过动态地重写当前页面来响应用户的动作或行为，而不是从服务器重新加载新页面。对此有两种方法:

*   要么只在一个页面加载中检索所有必要的内容，但根据应用程序的复杂性，这可能会花费太长时间并损害用户体验。
*   或者例如在用户**事件**之后，每个适当的内容被**请求**到服务器。常见的用户事件包括点击按钮、向下滚动页面、悬停在某个元素上、按下键盘上的某个键…

第二种方法对于复杂的应用程序来说是最常见的。事实上，让我们记住，单页应用程序的原因是一个更流畅的用户体验，不会被全页重载打断。

就代码而言，这意味着整个应用程序只有一个 HTML 文件——通常命名为*index.html*。

## 构建应用程序结构

应用程序分为两部分:前端和后端。**前端**是用户与之交互的部分:界面。**后端**管理服务器和用户界面之间的连接。要构建应用程序，有两个选项:

*   第一个选择是为后端和前端构建一个代码库——比如在 Github 上。
*   第二个选择是构建两个代码库:一个用于后端，一个用于前端。这有几个好处。其中两个是，一方面，它允许后端——在我们的例子中是一个 API 被**重用**多次，我们想要多少前端就重用多少次，另一方面，它允许**较小的目录**在我们的文本编辑器中管理。

选择没有对错之分。我上面给出的支持两个独立存储库的两个原因是我选择在两个独立的存储库中构建我的应用程序的主要原因。

我构建的第一个存储库是后端存储库。在我的**终端**中，我使用下面的命令创建了一个作为 API 的 **Rails** 应用程序，没有任何视图。这就像创建一个带有额外的**标志**的常规 Rails 应用程序一样。

```
rails new my_app_backend --api
```

对于前端，我使用了 **create-react-app** 生成器来开始。

```
npx create-react-app my_app_frontend
```

这两个命令为我提供了开始工作所需的所有文件。

## 关于组件的类型

这个特定项目的技术要求是至少有 2 个**容器**组件和 5 个**无状态**组件。React 中的组件是接口的构建块。它可以从父组件(称为**道具**)接收**输入**，并且可以根据需要多次**重复使用**。

为了更详细地了解这些容器组件和无状态组件，让我们先用几句话解释一下什么是 ***状态*** 。状态是我们导入的数据，并且会受到**更改**。在*状态*改变的众多原因中，有两个可能:它所来自的数据库已经被更新，所以*状态*将相应地改变或者用户修改了它。

容器组件也称为**有状态**组件，无状态组件也称为**表示性**组件。容器和表示组件之间的区别不是严格的，而是任意的，每个开发者都可以自由地组织他们的组件。但是一般来说，容器组件**会有*状态*** ，并且能够跟踪它的**变化**，而表示组件**不会有*状态*** ，它或者**显示通过*道具*传入的**内容，或者总是显示相同的内容。

对于我们在本文中讨论的应用程序，我决定进行基本的拆分。我为 API 中的每个**模型**构建了一个**容器**组件。随着我的前进，我删除了一些不必要的，我增加了其他的。主要是我的**有状态**组件是**形式**。最好的例子是注册表单和登录表单。在 React 的一个表单中，对于每个用户**输入**，都有一个**改变**处于*状态*，无论是本地*状态*还是 **Redux** 存储*状态*(我们稍后会稍微讨论 Redux)。无状态组件的一个例子是 *BicyclesList.js* 下的**自行车列表**。该组件作为 ***道具*** 接收来自*城市容器*组件的城市列表，并且与*状态*没有任何联系。

## React 中的路由

由于我们构建了一个没有完全重新加载页面的单页面应用程序，我们可能想知道**路由**是如何完成的。在 web 应用程序路由器中，您决定当用户访问某个**页面**时**应该发生什么**。因为我们在单个页面应用程序中只有**一个视图**，所以我们不能像以前在 Rails 应用程序中那样，当用户点击链接时**将用户重定向到**新视图**。**

一如既往，我们的问题总有解决的办法。下面是 ***反应路由器*库**。除其他外，它使我们能够确保:

*   我们有一个显示用户正在做什么的 URL，而不仅仅是基本的根 URL
*   用户可以使用浏览器的前进和后退按钮**和**
*   用户可以**在地址栏中输入**一个 URL，然后**导航**查看页面

为了解释我在路线中使用的模式，我将使用 CitiesContainer 作为示例:

```
*App.js***import** React **from** 'react';
**import** { Route } **from** 'react-router-dom';
**import** CitiesContainer **from** './containers/CitiesContainer';**class** CitiesContainer **extends** React.Component {
  render() {
    **return** (
      <Route path='/cities' component={CitiesContainer} />
    )
  }
}**export default** App;
```

通过使用*路径*而不是*精确路径*，所有包含“/城市”的路径都将被考虑在内。CitiesContainer 组件内部:

```
*containers/CitiesContainer.js* **import** React **from** 'react';
**import** { connect } **from** 'react-redux';
**import** { Route } **from** 'react-router-dom';
**import** { fetchCities } **from** '../actions/fetchCities';
**import** CitiesList **from** '../components/CitiesList';
**import** CityPage **from** '../components/CityPage';
**import** BicyclesList **from** '../components/BicyclesList';**class** CitiesContainer **extends** React.Component {
  componentDidMount() {
    **this**.props.fetchCities()
  } 

  render() {
    **return** (
      <div>
        <Route exact path='/cities' render={() => <CitiesList 
         cities={this.props.cities} />} />
        <Route exact path='/cities/:id' render={(routerProps) => 
         <CityPage {...routerProps} cities={this.props.cities} 
         />}/>
        <Route path='/cities/:id/bicycles' render={(routerProps) => 
         <BicyclesList {...routerProps} cities={this.props.cities} 
         />}/>
      </div>
    )
  }
}**const** mapStateToProps = state => {
  **return** {
    cities: state.cities
  }
}**export** **default** **connect**(mapStateToProps, { fetchCities })(CitiesContainer)
```

*routerProps* 是由 *react-router-dom* 包提供给美国开发者的。他们允许我们在*道具*中获取 URL 的内容作为**参数**。在我们的例子中，它允许我们在 props 中获得适当城市的 **id** ，通过它我们可以**过滤**所有城市——由 *props* 给出——并隔离出我们需要的城市。

## 使用 Redux

所以，我们终于到了 Redux。你会问 Redux 是什么。我记得问过我自己——和谷歌——同样的问题。我在 Youtube 上输入“Redux 是什么？”时发现了这个视频我觉得很清楚。我希望这对你也有用。 [Redux 文档](https://redux.js.org/)告诉我们 Redux 是 JavaScript 应用程序的**可预测状态容器**。文档坚持 Redux 的 4 个方面:

*   **可预测的**:它有助于编写在不同环境下运行的**一致的**应用程序，并且易于测试。
*   **集中化**:将应用程序的状态和逻辑集中化允许强大的功能，如 ***状态*持久性**。
*   **可调试的**:Redux dev tools 使得跟踪应用状态**何时、何地、为何以及如何改变**变得容易。
*   **灵活** : Redux 适用于任何 UI 层。

在我们的应用程序中，Redux 在很多方面都非常方便，但是在这个应用程序中真正重要的一点是拥有关于**当前用户**的信息——如果有一个登录的用户——可以在任何地方访问。事实上，我们将这些信息存储在 **Redux store** 中，这样我们就可以在应用程序的任何地方访问这些信息，而不仅仅是在组件中作为*道具*传递。事实上，任何子组件都可以 ***将*** 连接到商店，并且:

*   有**状态的内容为*道具*** ，但没有**引用**存放在我们的组件中，使用 *mapStateToProps()* 。
*   或者使用 *mapDispatchToProps()* 能够 ***分派*** 动作，而不需要**引用**我们组件中的存储。

这允许在管理的*状态*和显示的*状态*的**之间分离关注点。但这并不是我们用 Redux 所能做的全部。在构建**异步**动作创建器时，我们使用了 ***thunk* 中间件**。**

一、什么是中间件？维基百科上说它*是一种计算机软件，它向* ***操作系统*** *之外的软件应用程序提供* ***服务*** *。可谓'*软件胶水' '。所以 thunk 让我们可以做一些我们不能做的事情。一个 *thunk* 函数将 *dispatch* 作为一个参数，这样它就可以在函数内部**使用，在我们的例子中是在动作创建者内部。由于*分派*可以在函数内部使用，这使得当**我们的*获取*请求是**完成**时，我们能够*只分派*。这就是在下面的示例中所做的，我们只在获取所有城市的获取请求完成时进行调度:

```
*actions/fetchCities.j****s*****export** **const** fetchCities = () => {
  **return** (dispatch) => {
    fetch('[http://localhost:3000/api/v1/cities'](http://localhost:3000/api/v1/cities'))
    .then(response => response.json())
    .then(cities => {
      dispatch({
        **type**: 'FETCH_CITIES',
        **payload**: cities
      })
    })
  }
}
```

## 使用“提取”的数据持久性

使用*获取*，我们能够**从服务器获取**数据，以及**将**数据发送到同一服务器。从服务器获取数据的例子可以在我谈到路由时提到的 CitiesContainer 例子中找到。我们在下面的动作控制器中使用 fetch 来**获取**所有城市:

```
*actions/fetchCities.j****s*****export** **const** fetchCities = () => {
  **return** (dispatch) => {
    fetch('[http://localhost:3000/api/v1/cities'](http://localhost:3000/api/v1/cities'))
    .then(response => response.json())
    .then(cities => {
      dispatch({
        **type**: 'FETCH_CITIES',
        **payload**: cities
      })
    })
  }
}
```

在认证部分，登录动作创建者是一个向服务器发送 **POST** 请求的好例子。

```
*actions/auth.js***export** **const** login = credentials => {
  **return** (dispatch) => {
    fetch('[http://localhost:3000/api/v1/login'](http://localhost:3000/api/v1/login'), {
      **credentials**: 'include',
      **method**: 'POST',
      **headers**: {
        'Content-Type': 'application/json'
      },
      **body**: JSON.stringify(credentials)
    })
      .then(response => response.json())
      .then(user => {
        if (user.error) {
          alert(user.error)
        } else {
          dispatch(setCurrentUser(user))
          dispatch(resetLoginForm())
        }
      })
      .catch(console.log)
  }
}**export** **const** setCurrentUser = user => {
  **return** {
    **type**: 'SET_CURRENT_USER',
    **payload**: user
  }
}**export** **const** resetLoginForm = () => {
  **return** {
    **type**: 'RESET_LOGIN_FORM'
  }
}
```

至于**造型**，我最初打算用**引导**来反应，因为这是我主要听说过的一个。由于对样式非常陌生，并且在**导航**栏元素方面没有找到我要找的东西，我搜索了一下我可以在 React 中使用的其他样式**框架**。阅读[这篇文章](https://medium.com/@zeolearn/6-best-reactjs-based-ui-frameworks-9c780b96236c)，我检查了**语义 UI** React 的菜单组件，找到了我最初寻找的设置。由于从未使用过其他风格框架，我无法比较语义 UI 与其他相比使用起来有多简单，但是我必须说让**习惯**并使用起来是相当容易的。

就像应用程序的其余部分一样，样式并不完全是我此刻想要的样子，但我计划让这个应用程序在各个方面都是一个正在进行的工作。既然已经有了**的结构**和基本特性，增加**的新特性**会比从头开始容易。我期待增强应用程序的**功能**。