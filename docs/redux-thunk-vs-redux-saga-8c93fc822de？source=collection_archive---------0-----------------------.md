# Redux-Thunk vs Redux-Saga

> 原文：<https://javascript.plainenglish.io/redux-thunk-vs-redux-saga-8c93fc822de?source=collection_archive---------0----------------------->

![](img/382a0f9d3a799096d65b71056258dae6.png)

如今，所有动态 web 应用程序都需要使用异步任务。作为一名 react 开发人员，您可能会在职业生涯中使用 Redux-Thunk，并对迁移另一个解决方案感到不舒服，例如最近几年成为趋势的 Redux-Saga。是的，它有不同的语法，当你第一次检查它时，它看起来很混乱，没有意义。

与 Redux-Thunk 相比，Redux-Saga 的好处是您可以更容易地测试您的异步数据流。

然而，Redux-Thunk 对于小型项目和刚刚进入 React 生态系统的开发人员来说非常棒。thunks 的逻辑都包含在函数内部。更重要的是，你不需要学习 Redux-Saga 附带的奇怪和不同的语法。然而，在本教程中，我将向您展示如何轻松地从 Redux-Thunk 迁移到 Redux-Saga，并将解释那个奇怪的语法:d。

让我们先安装一些依赖项。

```
npm i --save react-redux redux redux-logger redux-saga redux-thunk
```

接下来，我们需要在项目中设置一个 Redux。让我们创建一个 Redux 文件夹，然后在里面放入 **store.js** 文件。

```
import { createStore, applyMiddleware } from 'redux';
import logger from 'redux-logger';
import thunk from 'redux-thunk';import rootReducer from './root-reducer';const middlewares = [thunk];if (process.env.NODE_ENV === 'development') {
  middlewares.push(logger);
}export const store = createStore(rootReducer, applyMiddleware(...middlewares));export default store;
```

然后我们需要创建 **root-reducer.js**

```
import { combineReducers } from 'redux';
import fetchTasksReducer from './reducers/fetchTasksReducer'const rootReducer = combineReducers({
 tasks: fetchTasksReducer,
});export default rootReducer;
```

别忘了在 **App.js** 里面导入我们的商店

```
import React, { Component } from 'react';
import { BrowserRouter, Route } from 'react-router-dom';
import { Provider } from 'react-redux';
import Tasks from './components/tasks';
import './App.css';const store = require('./reducers').init();class App extends Component {
render() {
return (
<Provider store={store}>
<BrowserRouter>
<div className='App'>
<div className='container'>
<Route exact path='/' component={Tasks} />
</div>
</div>
</BrowserRouter>
</Provider>
);
}
}
export default App;
```

现在我们需要创建一个`fetchTasksReducer`。

```
...const INITIAL_STATE = {
  tasks: null,
  isFetching: false,
  errorMessage: undefined
};const fetchTasksReducer = (state = INITIAL_STATE, action) => {
  switch (action.type) {
    case "FETCH_TASKS_START":
      return {
        ...state,
        isFetching: true
      };
    case "FETCH_TASKS_SUCCESS":
      return {
        ...state,
        isFetching: false,
        tasks: action.payload
      };
    case "FETCH_TASKS_ERROR":
      return {
        ...state,
        isFetching: false,
        errorMessage: action.payload
      };
    default:
      return state;
  }
};export default fetchTasksReducer;
```

每当我们调度一个动作时，该动作会通过`fetchTasksReducer`并根据需要更新我们的状态。这三个条件是如何工作的:

*   `FETCH_TASKS_START` : HTTP 请求已经开始，这是展示一个 spinner 的好时机，比如让用户知道有一些进程正在运行。
*   `FETCH_TASKS_SUCCESS` : HTTP 请求成功，我们必须更新状态。
*   `FETCH_TASKS_ERROR` : HTTP 请求失败。您可以显示一个错误组件，让用户了解问题。

# 还原-thunk

目前，我们的应用程序并不像预期的那样工作，因为我们需要一个动作创建器来触发我们的 reducer 处理的动作。

```
export const fetchTasksStarted = () => ({
  type:  "FETCH_TASKS_START"
});export const fetchTasksSuccess = tasks => ({
  type: "FETCH_TASKS_SUCCESS",
  payload: tasks
});export const fetchTasksError = errorMessage => ({
  type: "FETCH_TASKS_ERROR",
  payload: errorMessage
});const fetchTasks =  () => async dispatch => {
    dispatch(fetchTasksStarted())
    try{
        const TaskResponse = await fetch("API URL")const task = await taskResponse.json()
        dispatch(fetchTasksSuccess(tasks))
    }catch(exc){
        dispatch(fetchTasksError(error.message))
    }
}
```

`fetchTasks`可能一开始看起来很奇怪，但它是一个返回另一个带有参数`dispatch`的函数的函数。一旦 dispatch 被调用，控制流将转移到 reducer 来决定做什么。在上面的例子中，如果请求成功，它只更新应用程序状态。

# 还原传奇

redux-saga 是一个 Redux 中间件，允许我们使用 Redux 轻松实现异步代码。是 Redux Thunk 最热门的竞争对手。

让我们开始吧。让我们假设我们有相同的项目，但是在 Redux-Thunk 实现之前。让我们在 **Store.js** 中实现 Redux-saga 中间件

```
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';
import logger from 'redux-logger';import rootReducer from './root-reducer';import { watchFetchTasksSaga } from './saga/fetchTasks.saga';const sagaMiddleware = createSagaMiddleware();const middlewares = [logger, sagaMiddleware];export const store = createStore(rootReducer, applyMiddleware(...middlewares));sagaMiddleware.run(watchFetchTasksSaga);export default store;
```

是时候创建**动作了:**

```
export const fetchTasksStarted = () => ({
  type:  "FETCH_TASKS_START"
});export const fetchTasksSuccess = tasks => ({
  type: "FETCH_TASKS_SUCCESS",
  payload: tasks
});export const fetchTasksError = errorMessage => ({
  type: "FETCH_TASKS_ERROR",
  payload: errorMessage
});
```

让我们创建一个 **saga** 文件夹，并在 **saga** 文件夹中创建一个名为`fetchTasks.saga`的新文件。

```
import { takeLatest, put } from "redux-saga/effects";function* fetchTasksSaga(){
 try {const taskResponse = yield fetch("API URL")const tasks = yield taskResponse.json()yield put(fetchTasksSuccess(tasks));} catch (error) {yield put(fetchTasksError(error.message));
  }
}export default function* watchFetchTasksSaga(){
    yield takeLatest("FETCH_TASKS_START", fetchTasksSaga)
}
```

那些东西叫做发生器函数。
你可以同时使用`takeLatest`和`takeEvery` ，但是我们用的是`takeLatest`

只要它只需要最后一个事件。

通过调用`put`函数，我们可以像使用`dispatch`一样触发动作。它允许调整我们的减速器来处理我们的行动。

```
...const INITIAL_STATE = {
  tasks: null,
  isFetching: false,
  errorMessage: undefined
};const fetchTasksReducer = (state = INITIAL_STATE, action) => {
  switch (action.type) {
    case "FETCH_TASKS_START":
      return {
        ...state,
        isFetching: true
      };
    case "FETCH_TASKS_SUCCESS":
      return {
        ...state,
        isFetching: false,
        tasks: action.payload
      };
    case "FETCH_TASKS_ERROR":
      return {
        ...state,
        isFetching: false,
        errorMessage: action.payload
      };
    default:
      return state;
  }
};export default fetchTasksReducer;
```

# 结论

就是这样。现在您已经熟悉了 React 和 Redux 中处理异步任务的两种方法。你可以根据你正在做的项目来决定选择哪一个。