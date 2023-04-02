# 反应模式—小代码和林挺

> 原文：<https://javascript.plainenglish.io/react-patterns-small-code-and-linting-6c5bd629b9af?source=collection_archive---------11----------------------->

![](img/bf4886ad00ad443504636b0c402e14ee.png)

Photo by [William Moreland](https://unsplash.com/@relentlessjpg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将看看如何通过编写小组件和使用 linters 来清理 React 代码。

# 小部件

我们应该保持组件较小，这样我们就可以轻松地管理每个组件的逻辑。

例如，我们应该写:

```
const LoggedInMenu = () => {
 // JSX for user menu
}const LoggedOutMenu = () => {
 // JSX for admin menu
}
...
const App = () => {
  ...
  return (
    <div>
      {loggedIn && LoggedInMenu()}
      {!loggedIn && LoggedOutMenu()}
    </div>
  )
}
```

我们将组件分成更小的组件。

我们有一个用于`LoggedInMenu`和一个`LoggedOutMenu`。

# 埃斯林特

我们可以使用 ESLint 来检查语法问题。

它有很多规则来检查我们能想到的几乎任何东西。

任何不好的做法都会被标记出来，我们必须修正它们。

还有专门针对 React 应用的规则。

要安装它，我们可以运行:

```
npm install --global eslint
```

然后我们可以运行:

```
eslint app.js
```

检查语法是否有问题。

# 配置

我们可以在`.eslintrc`文件中配置规则。

每个规则有 3 个选项。

0 表示禁用，1 表示警告，2 表示错误。

例如，我们可以写:

```
{
  "rules": {
    "for-direction": [2]
  }
}
```

`for-direction`是规则的名称，2 代表错误。

为了使启用规则更容易，我们可以启用一整套规则。

例如，我们可以写:

```
{
  "extends": "eslint:recommended"
}
```

启用所有规则。

`extends`意味着我们使用了`eslint:recommended`列表中的所有规则。

此外，我们必须为 ES6 和 JSX 添加一些配置。

在`.eslintrc`中，我们写道:

```
"parserOptions": {
  "ecmaVersion": 6,
  "ecmaFeatures": {
    "jsx": true
  }
}
```

现在我们有林挺的 ES6 和 JSX 的。

通过配置 ESLint，我们可以实时获得对代码的反馈，这样我们就可以纠正任何不好的代码。

# React ESLint 插件

要使用 ESLint 对应用程序进行 lint React，我们可以通过运行以下命令为 ESLint 安装 React 插件:

```
npm install --global eslint-plugin-react
```

然后我们可以通过添加以下内容将其添加到`.eslinttc`:

```
"plugins": [
  "react"
]
```

并通过编写以下内容来更新`extends`关键字:

```
"extends": [
 "eslint:recommended",
 "plugin:react/recommended"
],
```

现在，ESLint 将检测 React 代码的问题。

要启用规则，我们可以编写:

```
"rules": {
  "react/jsx-indent": [2, 2]
}
```

第一个 2 表示规则被启用，第二个 2 表示我们想要缩进 2 个空格。

# Airbnb 配置

一个流行的 ESLint 插件是 Airbnb 配置插件。

它有自己的一套规则，类似于默认规则，但有一些额外的东西在里面。

要使用它，我们首先运行:

```
npm install --global eslint-config-airbnbeslint eslint-plugin-jsx-a11y eslint-plugin-import eslint-plugin-react
```

安装所有的依赖项。

然后在`.eslintrc`中，我们写道:

```
{
  "extends": "airbnb"
}
```

# 函数式编程

JavaScript 具有函数式编程特性，让我们可以创建具有这些特性的程序。

![](img/742783f10d85e4b52fbdb138dbb5ca19.png)

Photo by [Michael Anfang](https://unsplash.com/@manfang?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 一等品

JavaScript 包含的一个很大的函数式编程特性是，函数和其他任何东西一样都是对象。

这样，我们可以将它们设置为属性值，为它们分配变量，并将它们作为参数传入。

例如，如果我们有:

```
const add = (a, b) => a + b;
```

然后我们可以通过编写来创建一个函数:

```
const log = func => (...args) => {
  console.log(...args);
  return func(...args);
}
```

那么我们可以使用`log`如下:

```
const add = (a, b) => a + b;
const logAdd = log(add);
logAdd(1, 2);
```

我们可以使用`logAdd`来创建它。

`log`是高阶函数，因为它将函数作为参数。

它还返回一个可以用自己的参数调用的函数。

我们将`add`函数传递给`log`来返回一个函数。

然后我们可以通过向返回的函数传递参数来调用它。

# 结论

我们可以使用 ESLint 来检查 React 项目中的错误语法。

此外，函数式编程特性也经常在 React 中使用。