# 用 Jest 测试异步 Redux 操作

> 原文：<https://javascript.plainenglish.io/testing-async-redux-actions-with-jest-3bde5dd88607?source=collection_archive---------2----------------------->

大家好！！！我想写这个故事，是因为背后还有一个很搞笑的故事。所以，这要追溯到 2 年前，当我第一次被引入 React，然后为 actions 和 reducers 编写单元测试用例时，我差一点！！…我想说，在我用 Axios 测试一些异步调用之前，我几乎可以应付自如。我搜索了整个互联网，试图找到最好的方法来做到这一点，但它真的花了我很多时间来理解如何通过它。

所以今天我想分享我是如何做到的。

![](img/c5bd0729e83365889c1cff3ba2d56d3d.png)

让我们一步一步来:

> redux 中的异步动作是什么？

通过 Redux 使用异步动作的标准方式是使用 [Redux Thunk 中间件](https://github.com/gaearon/redux-thunk)。它在一个名为`redux-thunk`的独立包中。我们将在稍后解释中间件一般是如何工作的[；现在，您只需要知道一件重要的事情:通过使用这个特定的中间件，动作创建者可以返回一个函数，而不是一个动作对象。这样，动作创建者就变成了一个](https://redux.js.org/advanced/middleware) [thunk](https://en.wikipedia.org/wiki/Thunk) 。

**我们来举个例子:**

Async Action with redux

在上面的要点中，我们有一个从虚拟 api 返回一些数据的方法。在返回一个响应后，发送一个类型和一个有效载荷，我们将测试它。

让我们设置测试文件:

**步骤 1** 把你的动作和类型拿到测试文件中进行测试:

```
// action types
import { GET_ALL_ITEMS } from '../compare.types'; //action to mock
import { loadItemData } from '../compare.action';
```

**步骤:2** 导入 configureStore 和 thunk 来测试我们的异步方法:

```
// import configureStore to create a mock store where we will dispatch our actions
import configureStore from 'redux-mock-store';//import thunk middle to make our action asynchronous
import thunk from 'redux-thunk';
```

**步骤 3** 导入 MockAdapter 和 thunk 来测试我们的异步方法:

```
// important:  this i have used to mock the axios call
import MockAdapter from 'axios-mock-adapter';// import axios dependency
import axios from 'axios';
```

**步骤:4** 初始化存储、中间件、模拟存储和 thunk

```
// initialise middlewares
const middlewares = [thunk]; // initialise MockStore which is only the configureStore method which take middlewares as its parameters
const mockStore = configureStore(middlewares); //creating a mock instance from the MockAdapter of axios
const mock = new MockAdapter(axios); const store = mockStore({});
```

步骤 4: 启动测试套件

```
describe('Testing loadItemData()', () => { });
```

**步骤 5:** 清除商店中正在运行的任何操作

```
describe('Testing loadItemData()', () => { 
   beforeEach(() => { // Runs before each test in the suite             store.clearActions();    });
});
```

**第六步:**模仿你的 get call，调度你的行动

就是这样！！！希望你已经学会了如何用 Jest 和 Axios 测试异步 api 调用。

感谢阅读！如果你有任何问题，请随时联系 rajrock38@gmail.com，通过 LinkedIn 联系我，或者通过 Medium 和 Twitter 关注我。

如果你觉得这篇文章很有帮助，给它一些掌声会很有意义👏并分享出来帮别人找！并欢迎在下方发表评论。