# 使用 TypeScript 在 React & Redux 中进行类型检查

> 原文：<https://javascript.plainenglish.io/type-checking-in-react-redux-with-typescript-834cd75cb7d2?source=collection_archive---------5----------------------->

![](img/563bfcf77af004cd9b9bc96ce0dc9d50.png)

TypeScript 在 JS 社区中的地位越来越强，许多受欢迎的公司开始在基于 react 的前端使用它进行类型检查。让我们看看如何将 TypeScript 与 React 和 Redux 一起使用。

# **向现有的 create-react-app 项目添加 TypeScript**

如果要将 TypeScript 添加到现有应用程序中，请安装 TypeScript 和其他必需的类型

```
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

然后我们需要将文件重命名为`.ts`或`.tsx`，然后启动服务器。这将自动生成`tsconfig.json`文件。

# **使用带动作的类型脚本**

```
export interface IStartFetchPostsAction extends Action<’StartFetchPosts’> {}export interface IFetchPostsSuccessAction extends Action<’FetchPostsSuccess’> {
 posts: IPost[];
}export type PostActions =
 | IStartFetchPostsAction
 | IFetchPostsSuccessAction
```

这些动作类型扩展了 Redux 库附带的通用`Action`类型。这确保了我们在使用代码中的动作时正确地设置了`type`属性。

正如你在上面看到的，我们有引用 2 个动作的`PostsActions`联合类型。我们稍后将在 Posts reducer 中使用它来确保我们使用正确的操作。

你可能会问，我们从哪里获得 IPost[]。嗯，有一个单独的文件，我们在那里为帖子创建了一个类似这样的界面。

```
export interface IPost {
 id: number;
 title: string;
 description: string;
...
}
```

至于从后端获取帖子应该是异步操作，这就是 Redux-Thunk 解决我们问题的地方。然而，在这里使用 TypeScript 会更有挑战性。

```
export const fetchPostsActionCreator: ActionCreator<
 ThunkAction<
 Promise<IFetchPostsSuccessAction>, 
 >
> = () => {
 return async (dispatch: Dispatch) => {
 const startFetchPostsAction: IStartFetchPostsAction = {
 type: ‘StartFetchPosts’,
 };
 dispatch(startFetchPostsAction);
 const posts = await fetch("API URL");
 const fetchPostsSuccessAction: IFetchPostsSuccessAction = {
 posts,
 type: ‘FetchPostsSuccess’,
 };
 return dispatch(fetchPostsSuccessAction);
 };
};
```

`ActionCreator`是 Redux 库中的一个通用类型，它接受动作创建者返回的类型。上面的动作创建器返回一个函数，该函数将返回`IFetchPostsSuccessAction`。第一次，你可能会感到陌生。但是它是 Redux-Thunk 库附带的一个泛型。

# **使用带减速器的打字稿**

让我们看看当我们实现 TypeScript 时，我们的 Reducer 会是什么样子:

```
import { Reducer } from "redux";
import {
  IPostsState,
  PostsActions,
} from "./PostsTypes";const initialPostState: IPostsState = {
  posts: [],
  isLoading: false
};const postsReducer: Reducer<IPostsState, PostsActions> = (
 state = initialPostsState,
 action,
) => {
 switch (action.type) {
 case ‘StartFetchPosts’: {
 return {
 …state,
 isloading: true,
 };
 }
 case ‘FetchPostsSuccess’: {
 return {
 …state,
 posts: action.posts,
 isloading: false,
 };
 }
 }
return state;
};
```

我们还需要一个减根器

```
import { combineReducers } from 'redux';
import PostsReducer from './reducers/PostsReducer'const rootReducer = combineReducers<IAppState>({
 Posts: postsReducer
});export default rootReducer;
```

我们使用 Redux 库中的泛型`Reducer`类型，并通过`PostsActions`联合类型传入我们的状态类型。

`switch`语句的每个 case 中的`action`参数的类型被缩小到与特定 case 相关的特定动作。

# **使用 TypeScript 和 Store**

我们正在使用 Redux 库中的通用类型`Store`,并传入我们的应用程序状态的类型，在这个项目中是`IAppState`。让我们在我们的 **store.js** 内部做出改变

```
export function configureStore(): Store<IAppState> {
 const store = createStore(rootReducer, undefined, applyMiddleware(thunk));
 return store;
}
```

# 在组件内部使用 TypeScript

让我们看看当我们在组件内部实现 TypeScript 时会是什么样子。

```
import * as React from "react";
import { connect } from "react-redux";
import { RouteComponentProps } from "react-router-dom";
import { fetchPosts } from "./PostsActions";
import { IPost } from "./PostsData";
import PostsList from "./PostsList";
import { IAppState } from "./Store";interface IProps extends RouteComponentProps {
  loading: boolean;
  posts: IPost[];
}class PostsPage extends React.Component<IProps> {
  public componentDidMount() {
    this.props.fetchPosts();
  }public render() {
  ...
    return (
      <div className="page-container">

        <PostsList
          posts={this.props.posts}
          loading={this.props.loading}
        />
      </div>
    );
  }
}const mapStateToProps = (store: IAppState) => {
  return {
    loading: store.posts.isLoading,
    posts: store.posts.posts
  };
};const mapDispatchToProps = (dispatch: any) => {
  return {
    fetchPosts: () => dispatch(fetchPosts())
  };
};export default connect(
  mapStateToProps,
  mapDispatchToProps
)(PostsPage);
```

`mapStateToProps`函数使用我们的`IAppState`类型，因此对商店状态的引用是强类型的。

**收尾**

在现有的 React 项目内部集成 TypeScript 并没有人们想象的那么可怕。当然，当你开始创业公司的开发过程时，最好生成新项目，并在最早阶段使用 TypeScript，但无论如何，现在你已经准备好接受挑战了:)