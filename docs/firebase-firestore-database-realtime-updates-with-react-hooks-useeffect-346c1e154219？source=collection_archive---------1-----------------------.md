# Firebase — Firestore 数据库实时更新与 React 挂钩—使用效果

> 原文：<https://javascript.plainenglish.io/firebase-firestore-database-realtime-updates-with-react-hooks-useeffect-346c1e154219?source=collection_archive---------1----------------------->

我按照 Robin Wieruch 的一个很棒的教程[来用 React 应用程序设置 Firebase。](https://www.robinwieruch.de/complete-firebase-authentication-react-tutorial#provide-firebase-in-react)

本教程设置用户验证并创建了一个`withFirebase`高阶组件(HOC)，提供 Firebase 配置和初始化。

唯一的问题是，教程中的所有内容都是用类编写的(写于 2018 年)，我想尝试用钩子设置订阅！

![](img/d955aa9db8f249231bf72c162814a59a.png)

[https://unsplash.com/photos/UKX_DwNKXSA](https://unsplash.com/photos/UKX_DwNKXSA)

## 使用 useEffect 订阅更新

传统上，通过`componentDidMount()`订阅实时更新，然后在`componentWillUnmount()`取消订阅。

使用 React 的 useEffect 钩子，我们可以设置对 Firebase Firestore 数据库的订阅，并通过取消订阅进行清理。

```
useEffect(() => {
  const unsubscribe = props.firebase
    .db.collection('myCollectionName')
    .onSnapshot(snapshot => {
      if (snapshot.size) {
        // we have something
        ** **Handle returned data ****
      } else {
        // it's empty
      }
    })return () => {
    unsubscribe()
  }
}, [props.firebase])
```

我们声明一个名为 unsubscribe 的常量，它调用 firebase prop，使用 [onSnapshot()](https://firebase.google.com/docs/firestore/query-data/listen?authuser=0) 订阅更新。

useEffect 中的`return`调用处理清理。这里我们称之为`unsubscribe()`，它在组件卸载时运行。

我们做的最后一件事是[优化挂钩](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)，仅在`[props.firebase]`改变时重新运行。也可以使用空数组`[]`在组件第一次挂载时只运行一次。

## 处理返回的数据

如果我们的 Firebase Firestore 系列有文档，那么`snapshot.size`将等同于`true`。

这里我们可以处理返回的结果。

例如，我们可以声明一个数组变量，遍历快照，将数据推送到数组，并将新的数据数组存储在 state:

```
let myDataArray = []snapshot.forEach(doc =>
  myDataArray.push({ ...doc.data() })
)setData(myDataArray)
```

## 否则…

如果`snapshot.size`等同于`false`，则提取出现了问题。

这可能只是因为集合是空的。

## 搬运装载

对于这个组件，我想显示一个加载微调器，直到我从 Firestore 返回一个结果。

为此，将加载变量存储在 state 中，并将其初始化为`true`——我们正在加载，直到 Firestore 告诉我们其他方法。

`const [loading, setLoading] = useState(true)`

在我们的组件内部，我们可以测试加载状态，显示一个`<Loader />`或`null`:

`{ loading ? <Loader /> : null }`

在我们的 useEffect 方法中，当我们从 Firebase 获得响应时，我们可以将 loading 设置为 false:

```
useEffect(() => {
  const unsubscribe = props.firebase
    .db.collection('myDbName')
    .onSnapshot(snapshot => {
      if (snapshot.size) {
        // we have something
        **setLoading(false)   **  
      } else {
        // it's empty
       ** setLoading(false)**
      }
    })return () => {
    unsubscribe()
  }
}, [props.firebase])
```

如果快照返回一些东西或者什么也不返回，无论哪种方式我们都已经完成了加载，所以我们使用`setLoading`钩子来设置它。

## …就是这样！

目前，Firebase HOC 是一个很好的设置，我觉得没有必要用钩子重写。

## 参考链接

*   用 React 设置 Firebase 教程，Robin wie ruch—[https://www . Robin wie ruch . de/complete-Firebase-authentic ation-React-Tutorial/# provide-fire base-in-React](https://www.robinwieruch.de/complete-firebase-authentication-react-tutorial/#provide-firebase-in-react)
*   Google Firebase Firestore Docs—[https://Firebase . Google . com/Docs/Firestore/query-data/listen？authuser=0](https://firebase.google.com/docs/firestore/query-data/listen?authuser=0)
*   反应使用效果文档—[https://reactjs.org/docs/hooks-effect.html](https://reactjs.org/docs/hooks-effect.html)