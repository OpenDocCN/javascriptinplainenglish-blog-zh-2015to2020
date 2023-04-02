# 在 Redux 中创建纯不可变的 Reducers

> 原文：<https://javascript.plainenglish.io/redux-creating-pure-immutable-reducers-7a32e993ade0?source=collection_archive---------0----------------------->

## 这是一个关于如何将当前的 reducers 迁移为纯粹的和不可变的快速演示。

![](img/b7bb2a6d79b01d058f82447fcae50e83.png)

# 快速还原摘要

Redux 的核心实际上只是一个单向消息订阅系统，但有了它，我们可以创建惊人的可扩展状态管理。

在这个消息传递系统中，我们可以订阅(也称为“观察”和“监听”)事件，比如调用一个函数向 API 发出请求。有趣的是，在这个订阅**中，我们可以传递数据。**

## 运行时

*   订阅已设置
*   商店已申报。

## 数据流

1.  **数据调度动作** → 2。**通过数据通知用户** → 3。**全球商店中的数据集**

正如你所看到的，redux 非常模块化和简单。记住你的 Redux 实现应该是**可移植的、可替换的**、**并且几乎与应用程序分离。**

在本文中，我们将主要讨论优化**步骤 2。**

# 我们如何处理 Redux 中的数据？

如果你在网上搜索“**在 Redux 应用程序中哪里处理逻辑？**“你会得到一些**非常固执己见的**信息。有人说 reducer，有人说 actions，有人说不要在你的动作或 reducer 中包含任何逻辑。

我们要说的是**在你的归约器和动作中不应该有逻辑，它们应该是完全纯粹的和功能性的**。

# 那么我们把逻辑放在哪里呢？

我发现有两个选项是最具可伸缩性和最简洁的。

1.  **Redux 逻辑中间件**(个人推荐)
2.  **在**逻辑服务**中的 Redux** 之外，即**将数据传递给动作**

一旦您将您的逻辑移出了您的 reducer，您就可以开始进行增强了。我们将使我们的存储**不可变**和**纯**，这意味着无论它接收到什么状态，都是它将要设置的状态，没有副作用或函数调用(如果需要，您可以进行一些格式化，但是**不改变结构**)。记住你希望你的状态是**可预测的**和**确定的**，这是我们设置一个**初始状态**的原因之一。

通过遵循这些规则，我们可以利用 reducer 中的不变性库来只更新存储中已经更改的值。这非常重要，因为它允许在应用程序的其余部分进行进一步优化

**例如**:在 React 中有`PureComponents`根据传递给组件的状态差异进行优化。如果我们要完全克隆状态(**不要 _。每次在我们的减速器里克隆**，都会让`PureComponents`完全没用(**连丹阿布拉莫夫都同意**)。没有 T2，我们就不能恰当地纪念他。如你所见，使用不变性有一系列的好处。

## 这是你的减速器现在的样子(没有优化)

```
// **BAD IMPLEMENTATION**
export const **userReducer** = (state = {}, action = {}) => {

 **let newState = _.cloneDeep(state);** switch (action.type) { case GET_USER:
      newState.status = "runnning";
      return newState; 

    case USER_SUCCESS: {
 **if(newState.user.name === undefined){
        newState.user.name = "noname";
      }
      if(newState.user.time){
        newState.timestamp = Date.now();
        resetTimer(user.time)
      }** return newState;

     case USER_ERROR:
       newState.status = "error"
       return newState; default: {
       return newState;
     }
  }
};
```

在这个 ***未优化*** 的例子中，有几个**问题**。我们可以看到有**没有初始状态**设置，我们是**深度克隆**状态，有**副作用**，我们也是**有条件更新存储**。如前所述，这将影响优化的能力，并使我们的数据更难预测。

## 纯/不可变缩减器示例(优化)

```
import **immutable** from "immutability-helper";
import {**handleActions**} from "redux-actions";
import {ACTIONS} from './constants';const **initialState** = {
  status: "",
  user: {},
};export const **userReducer** = **handleActions**(
  {
    [ACTIONS.GET_USER]: state =>
      **immutable**(state, {
        status: {$set: "running"}
      }),
    [ACTIONS.USER_SUCCESS]: (state, {**data**}) =>
      **immutable**(state, {
        status: {$set: "success"},
        **user**: {$set: data.**user**},
      }),
    [ACTIONS.USER_ERROR]: state =>
      **immutable**(state, {
        status: {$set: "error"}
      })
  },
  **initialState**
);
```

在 ***优化的*** 示例中，我们减少了代码，我们的状态更加可预测。

## 好处:

1.  没有逻辑或副作用
2.  单一责任
3.  我们总是返回相同的存储结构
4.  定义初始状态是为了一致性/可预测性
5.  不变性有助于仅对已更改的存储值进行操作
6.  允许使用`PureComponents`和`Memoizing`