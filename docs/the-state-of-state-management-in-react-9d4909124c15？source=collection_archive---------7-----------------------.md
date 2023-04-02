# React 中的状态管理状态

> 原文：<https://javascript.plainenglish.io/the-state-of-state-management-in-react-9d4909124c15?source=collection_archive---------7----------------------->

![](img/13f4d482aa2e21c9e2884d25755373d9.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

如今，随着 React 挂钩的引入，状态管理变得更加简单，改变了开发人员在他们的 React 组件中管理状态的方式。

以前，为特定组件定义状态很复杂(我将在下面讨论)，但是现在，创建一个像 useUserLogin 这样的抽象钩子来跨组件共享状态逻辑就很容易了。

我们将在 hooks 之前和使用 hooks 的时候看一下状态管理，为什么你可能想要使用状态管理库，以及今天一些常见的状态管理库。

# 在反应钩子之前

在 React hooks 之前，状态管理是相当二进制的。有两种选择:在组件中定义局部状态，或者使用状态管理框架来处理全局状态。

## 本地状态(挂钩前)

让我们看看钩子之前的本地状态。

```
export class TodoList extends Component {
    state = {
        isLoading: false,
        hasError: false,
        todos: []
    }

    searchTodos(searchValue) {
        this.setState({isLoading: true});

        api.get(`/todos?searchKey=${searchValue}`)
            .then((data) => this.setState({todos: data}))
            .catch(() => this.setState({hasError: true}))
            .finally(() => this.setState({loading: false}));
    }

    render() {
        if (this.state.isLoading) {
            // render loading spinner
        }

        if (this.state.hasError) {
            // render error message
        }

        return (
            <div>
                <input onChange={(event) => searchTodos(event.target.value)} />
            </div>
        )

    }
}
```

在这种情况下，我们可以通过`this.setState`属性非常有效地处理本地状态。

然而，假设您不希望 state 上的`loading`属性被约束到这个组件。现在，您必须引入一个像 Redux 这样的库来处理跨各种组件的全局状态。

现在这种方法可行了，但是这种方法有几个问题:

1.  更多样板代码
2.  复杂/错综复杂的流程
3.  全球状态无处不在，可能会产生奇怪的副作用

如果您是一名在 React hooks 之前从事代码库工作的开发人员，您应该知道 Redux 和全局状态在大型应用程序中会变得多么复杂，导致大量样板代码，使您的项目更难调试。

# 反应钩

在 React 16.8 中引入了钩子。现在，在组件之间共享状态行为变得容易多了，让我们看看如何在上面的组件中实现钩子:

```
import {useState} from 'react';

const useRequestHandler = () => {
    const [isLoading, setLoading] = useState(false);
    const [hasError, setError] = useState(false);
    const [data, setData] = useState(null);

    const handleRequest = (request) => {
        setLoading(true);
        setError(false);

        return api.get(request)
            .then(setData)
            .catch(() => setError(true))
            .finally(() => setLoading(false))
    };

    return {isLoading, hasError, data, handleRequest};
};

const UserList = () => {
    const {data, isLoading, hasError, handleRequest} = useRequestHandler();

    const searchUsers = (value) => handleRequest(`/todos?searchKey=${value}`);

    return (
        <React.Fragment>
            {data.map(u => <p>{u.name}</p>)}
        </React.Fragment>
    )
```

现在，加载和获取数据行为被抽象出来，允许您在应用程序中重用该功能。

然而，我们仍然有一个问题。将状态保存在钩子中并不意味着它成为一个单体，状态只绑定到一个组件。在某些情况下，我们可能只想保存一个状态实例(例如，只获取一次用户信息)。这就是状态管理框架证明其价值的地方。

# 如何决定状态保存在哪里

既然可以跨组件共享状态逻辑，那么我们如何决定是将状态保存在组件中(本地)还是全局呢？这取决于您是否希望在组件之间共享状态，比如跨多个组件本地跟踪用户的待办事项。

![](img/7ca36afe1772b74a340d92f8085d104b.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 全球国家管理框架

虽然我不会对这些框架进行全面的深入研究，但我会链接每个框架的网站。

1.  [React-Redux](https://react-redux.js.org/)
2.  [Mobx](https://github.com/mobxjs/mobx-react)
3.  [通量](https://facebook.github.io/flux/)
4.  [未说明](https://github.com/jamiebuilds/unstated)
5.  [反冲](https://recoiljs.org/)

# 结论

尽管 React 挂钩改变了 React 开发人员处理状态的方式，但它们并不是灵丹妙药。这并不意味着我们需要全局地保留每个状态——大多数情况下，最好将其保留在组件级别，但是有大量的解决方案可以使管理全局状态变得更容易。

# 保持联络

有很多内容，我很感谢你读我的。我是一名年轻的企业家，我写的是软件开发以及我经营和发展公司的经历。你可以在这里注册我的简讯

请随时联系我，在 Linkedin 或 Twitter 上与我联系。