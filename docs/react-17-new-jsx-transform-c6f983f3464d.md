# 反应 17:新 JSX 变换

> 原文：<https://javascript.plainenglish.io/react-17-new-jsx-transform-c6f983f3464d?source=collection_archive---------3----------------------->

## 新 JSX 变换集成指南

![](img/737191f756fc9458651bd0732fbc6d23.png)

Photo by [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 我的旅程

任何依赖项的每个主要发布都会导致“哦，天哪，我希望这个主要升级不会太痛苦”的想法。当我看到 React 已经在`npm outdated`发布的时候，我的反应也没什么不同。但是在看了他们的 [React 17 发布博客](https://reactjs.org/blog/2020/10/20/react-v17.html)后，我松了一口气——没有突破性的变化或新功能。但是，它提到了一个有趣的可选变化:新的 JSX 变换。

# JSX 变换

JSX 变换是必要的，这样你的源代码可以被转换成浏览器可以理解的代码。这就是 Babel 或 TypeScript 等编译器的用武之地——它们将开发人员友好的代码转换成浏览器友好的代码。旧的 JSX 转换需要你导入 React，因为这会告诉你的编译器这个文件需要一些 React JSX 魔法。否则，编译将会失败。这就是为什么 ESLint 插件`[eslint-plugin-react](https://www.npmjs.com/package/eslint-plugin-react)`包括并推荐了`react/react-in-jsx-scope`和`react/jsx-uses-react`规则。

然而，新的 JSX 变换不再需要 React 导入。编译仍然可以工作，不需要对你的 JSX 代码做任何改动！在内部，转换魔术的工作方式不同，但最终结果是相同的——您的代码在浏览器中的工作方式与之前完全一样。

# 反应效用

虽然您不再需要导入 React，但是如果您使用 React 实用程序、挂钩和其他工具，您可能仍然需要导入它们。这些应该通过它们的命名导入来导入。

```
import { Component, lazy, Suspense, useState } from 'react';
```

# 埃斯林特

这个新的转换将与一些 ESLint 规则相冲突。如果你正在使用`react/react-in-jsx-scope`或`react/jsx-uses-react`，无论是显式的还是通过普通配置(比如 [Airbnb 的 ESLint 配置](https://www.npmjs.com/package/eslint-config-airbnb)或`eslint-plugin-react`的推荐)，它们都需要被禁用。

```
// .eslintrc.js
module.exports = {
  rules: {
    'react/react-in-jsx-scope': 'off',
    'react/jsx-uses-react': 'off'
  }
}
```

# 配置

不过这个特性确实需要一点配置。如果你用的是 Create React App 的`react-scripts`，那就不得不不幸的咬紧牙关升级到 4.0。

如果您正在手动设置 Babel，支持这种转换至少需要 Babel 7.9 和在您的 Babel 配置文件中使用自动运行时的`@babel/preset-react`或`@babel/plugin-transform-react-jsx`。

```
// for @babel/preset-react
{
  "presets": [
    ["@babel/preset-react", {
      "runtime": "automatic"
    }]
  ]
}// for @babel/plugin-transform-react-jsx
{
  "plugins": [
    ["@babel/plugin-transform-react-jsx", {
      "runtime": "automatic"
    }]
  ]
}
```

# Codemod

为了支持这种迁移，React 团队幸运地为我们提供了一个 codemod:

```
npx react-codemod update-react-imports
```

在对您的环境提出一些问题之后，如果您使用任何 React 实用程序，这个脚本应该会自动删除默认的 React 导入并添加命名导入。当然，验证这些变化，因为这个 codemod 可能并不完美。

注意:我个人遇到了一个 MODULE_NOT_FOUND ( *错误:找不到模块' @ babel/runtime-corejs 3/helpers/interopRequireDefault '*)的问题，至今还没有找到问题的根本原因。但是我确实找到了一个临时删除(或注释掉)babel 配置文件的解决方法。

# 支持的版本

除了 React 17 之外，React 团队还将该功能向后移植到 React 16.14、React 15.7 和 React 0.14.10，这样您就可以利用该功能，而无需进行重大升级。但是如果你在 React 16 上，升级到 React 17 是没有痛苦的。

# 最后的想法

这样有什么好处吗？不完全是。但是我喜欢这种改变吗？是的，绝对的。我总是觉得很愚蠢，我会导入一些没有(明确地)使用的东西，就好像它是一个未使用的导入或变量。由于新的 JSX 变换只需要很少的配置更改，并且有一个 codemod 来自动重构一切，所以您也可以开始使用它。

# 资源

*   [JSX 转型官方博文](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)