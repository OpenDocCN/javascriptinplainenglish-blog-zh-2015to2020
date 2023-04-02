# 使用更短的 ES6 setState 语法简化 React 代码

> 原文：<https://javascript.plainenglish.io/simplify-your-react-code-using-shorter-es6-setstate-syntax-8643432244bb?source=collection_archive---------5----------------------->

## 编写 setState 调用的简单而简短的方法

![](img/7235d84f7b921cf2d22d9b10f983e52d.png)

Photo by [Fernando Hernandez](https://unsplash.com/@_ferh97?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将看到如何使用新的 ES6 更短的语法，这将使`setState`调用更加简单。所以我们先从一些背景知识开始。

看看下面的箭头功能。

```
// Example 1const getName = () => {
 return "David";
}
```

上面的 arrow 函数只有一行代码。如果 arrow 函数中只有一行代码，我们可以隐式地返回它，而不添加 return 关键字，这样我们可以使上面的代码更短，如下所示

```
// Example 2
const getName = () => "David";
```

示例 1 和示例 2 将产生完全相同的输出。示例 2 只是示例 1 的一个较短的语法。

如您所知，如果函数中有多行代码，我们将这些语句写在花括号({})中，并显式指定 return 语句。

如果我们从一个函数返回一个对象，我们把它写成

```
const getObject = () => {
 return {
  name: "David",
  age: 30
 };
};
```

我们可以将上面的代码简化为

```
const getObject = () => ({
 name: "David",
 age: 30
});
```

在这里，我们直接将对象添加到圆括号( )中，这将隐式地返回对象，而不需要 return 语句。

现在让我们看看如何在`setState`呼叫中使用它。

现场演示:[https://codesandbox.io/s/twilight-sea-lnk6z](https://codesandbox.io/s/twilight-sea-lnk6z)

如果您想知道状态是如何直接写在组件内部而不是构造函数中的，以及为什么没有。为事件监听器添加绑定函数调用，查看我以前的文章[这里](https://medium.com/javascript-in-plain-english/how-to-write-clean-and-easy-to-understand-react-code-using-class-properties-syntax-5b375b0618d3?source=friends_link&sk=c170992cab9025fddb7b34b8894ea993)详细解释了它。

看看上面代码中的`handleAddOne`方法中的`setState`方法

```
this.setState(prevState => {
 return {
  counter: prevState.counter + 1
 };
});
```

如您所见，这个`setState`调用几乎占用了五行代码。也很容易忘记添加 return 关键字，比如

```
// without return keyword
this.setState(prevState => {
 {
  counter: prevState.counter + 1
 };
});
```

即使您错过了 return 语句，上面的`setState`调用 without return 关键字也不会抛出任何错误，但它不会像预期的那样工作。为了克服这些问题，我们可以将其简化为

```
this.setState(prevState => ({
 counter: prevState.counter + 1
}));
```

现在看起来更短了。

我们可以进一步缩短它，因为函数中只有一条语句

```
this.setState(prevState => ({ counter: prevState.counter + 1 }) );
```

哇，我们已经将`setState`通话从五行转换成了一行。

如果我们需要设置不止一个状态属性，那么把它放在不止一行是完全可以的

```
this.setState(prevState => ({
  counter: prevState.counter + 1,
  name: 'Jane'
 })
);
```

现在，假设我们在状态中有一个数字数组，我们想要删除数组中的所有元素，我们使用下面的`setState`调用

```
this.setState(() => {
 return {
  numbers: []
 }
});
```

一如既往，我们可以将其简化为

```
this.setState(() => ({ numbers: [] }) );
```

使用这种更短的返回对象的语法，不需要任何配置更改或任何安装。它是 ES6 语法，所以默认情况下在 create-react-all 中可用。

看看我最近出版的[精通 Redux](https://master-redux.yogeshchavan.dev/) 课程。

在本课程中，您将构建 3 个应用程序以及一个点餐应用程序，您将了解:

*   基本和高级冗余
*   如何管理数组和对象的复杂状态
*   如何使用多个减速器管理复杂的冗余状态
*   如何调试 Redux 应用程序
*   如何在 React 中使用 Redux 使用 react-redux 库让你的 app 反应性。
*   如何使用 redux-thunk 库处理异步 API 调用等等

最后，我们将从头开始构建一个完整的[订餐应用](https://www.youtube.com/watch?v=2zaPDfCKAvM)，集成 stripe 以接受支付，并将其部署到生产中。

**别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周简讯，里面有惊人的技巧、窍门和文章。**