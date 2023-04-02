# Redux-Toolkit 中的 createAsyncThunk

> 原文：<https://javascript.plainenglish.io/createasyncthunk-in-redux-toolkit-4d8d2f0412d3?source=collection_archive---------0----------------------->

![](img/3700e0bf2a13116d1b01c8030bddce0c.png)

*分派异步操作从来没有看起来这么好*

所以，这是我的 *Redux-Toolkit* 系列的第三部分，你可以在下面找到第一和第二部分:

*   第一部分:介绍 [Redux-Toolkit](https://medium.com/datadriveninvestor/redux-toolkit-eb07f753649)
*   第 2 部分:在 Redux-Toolkit 中创建[切片](https://medium.com/datadriveninvestor/createslice-in-redux-toolkit-c5e5441b75d9)

当谈到管理 React 应用程序中的状态时，Redux 已经成为某种程度上的行业标准。现在，随着 Redux Toolkit 的推出，生活变得前所未有的简单。它功能强大，易于设置，并且您可以创建存储片段，以获得更好的代码可维护性和模块化。

Redux 的核心是同步的，所以我们需要添加像 [Redux-Thunk](https://github.com/reduxjs/redux-thunk) 或 Saga 这样的中间件来帮助我们处理异步位。有了 Redux-Toolki，我们就可以将 Thunk 集成为一个依赖项。

## ***createAsyncThunk***

根据官方文档:***createasynchunk***是一个函数，接受一个 Redux 动作类型字符串和一个应该返回一个承诺的回调函数。它根据您传入的操作类型前缀生成承诺生命周期操作类型，并返回一个 thunk 操作创建器，该创建器将运行承诺回调并根据返回的承诺调度生命周期操作。

让我们来分解一下:

```
export const fetchToDoList = createAsyncThunk(
 "todo/fetchList", async (_, { rejectWithValue },{condition:true}) => {
   try {
    const list = await getList();
    return list;
   } catch (err) {
    return rejectWithValue([], err);
   }
 });
```

**createasynchunk**接受三个参数:

1.  类型:*“todo/fetchList”。*遵循的一般命名约定是{reducerName}/{actionType}
2.  *payloadCreator* :是回调函数( *async (_，{ rejectWithValue })= > {}* )，第一个 param 是传递给回调的实参。第二个参数是 thunkApi(定义如下)。
3.  *options* :是一个有两个道具的对象， *condition* 是一个回调，它返回一个可以用来跳过执行的 bool，*dispatchConditionRejection*使用 *condition* 来调度动作。如果条件为假*dispatchConditionRejection*将不会分派任何动作。

**ThunkApi** 很重要，因为大多数时候你将依赖于其中定义的属性。它们是:

1.  *调度*:调度不同的动作。
2.  *getState* :从回调中访问 redux 存储
3.  *requestId* :这是 redux-toolkit 为每个请求生成的唯一 Id
4.  *信号*:该信号可用于[取消请求](https://medium.com/datadriveninvestor/aborting-cancelling-requests-with-fetch-or-axios-db2e93825a36)。
5.  *rejectWithValue* :是一个效用函数，在出错的情况下，可以返回给动作创建者一个定义好的有效载荷。
6.  *extra* :设置时给予 thunk 中间件的“额外参数”，如果可用的话

> ***承诺生命周期动作:***

我更喜欢使用*createasincthunk***，**的一个主要原因是它提供的生命周期动作。动作的三个生命周期如下:

1.  *待定*:在 *payloadCreator* 中调用回调之前
2.  履行了:关于成功执行死刑
3.  *拒绝*:错误时

```
[fetchToDoList.fulfilled]: (state, { meta, payload })=> {  
  state.todoList = payload;
 },
[fetchToDoList.pending]: (state, { meta })=>{
  state.loading = "pending";
},
[fetchToDoList.rejected]: (state,{meta,payload,error })=>{
  state.error = error;
}
```

每个生命周期都被传递了 reducer 状态(不是 store obj)和 thunk action creator，其中包含*有效负载(*返回值 *)* 已完成/已拒绝*，包含 requestId 和传递给 *payloadCreator 的参数的 meta* ，如果被拒绝*则为 error* 。*

让我们看一个简单的 todoList 切片来更好地理解:

todoSlice

这样，我们可以更方便地分派我们的异步操作，而不用将我们的逻辑分布到多个文件中。

你可以在这里找到 todo 应用[的 git repo。](https://github.com/devAbhimanyu/Redux-toolkit/tree/createAsyncThunk)

*更多内容请看*[*plain English . io*](http://plainenglish.io/)*。报名参加我们的* [*免费每周简讯*](http://newsletter.plainenglish.io/) *。在我们的* [*社区*](https://discord.gg/GtDtUAvyhW) *获得独家写作机会和建议。*