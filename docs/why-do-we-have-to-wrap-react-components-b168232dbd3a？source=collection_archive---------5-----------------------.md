# 为什么我们必须包装 React 组件？

> 原文：<https://javascript.plainenglish.io/why-do-we-have-to-wrap-react-components-b168232dbd3a?source=collection_archive---------5----------------------->

![](img/06da0e307a75219adb015823c156ad4e.png)

[@ffstop](https://unsplash.com/@ffstop) Unsplashed.com

## 理解 React 应用程序中的 div 包装！

# 问题是

因此，当您第一次开始使用 react 时，您肯定会编写类似下面这样的代码

```
const App = () => {
    return(
      <p>Hello</p>
      <p>World</p>
     )
  } 
```

你可能会问这有什么错？

React 试图转换我们的 JSX，这个弹出

```
Failed to compile.
 ./src/App.js
Syntax error: Adjacent JSX elements must be wrapped in an enclosing tag (6:8)
```

在组件中编写的所有 JSX 语句的幕后，它们需要被包装到一个容器中。但是等等为什么有必要？

# 幕后的反应

要回答这个问题，我们必须考虑当 React 将我们的 JSX 最终变成我们在页面上看到的 HTML 时会发生什么。这个过程的第一步是将任何 JSX 语句转换成一个对象。在幕后，React 使用我们的 JSX，Babel 这样的 transpiler 将 JSX 的各个部分输入到`React.createElement`函数中。

如果我们看看 createElement 的 API

```
function createElement(elementType, props, …children) {}
```

参数被定义为

1.  `elementType` —您想要显示的 HTML 标签
2.  `props` —您想要传递的属性或样式等数据
3.  `children` —你想在最终的 HTML 标签之间传递的任何东西，最有可能是
    一些文本，但也可以是其他东西(见下文！)

这是我们从上面看到的 JSX 的例子

```
<p>Hello</p> // JSX
```

成为

```
React.createElement(<p>, null, 'Hello')
```

`React.createElement`函数只接受一个“元素类型”，也就是我们想要最终显示的 JSX 元素的`<p>`部分。

我们不能让两个 JSX 语句分别具有独立的`React.createElement`功能。这意味着两个 return 语句和 React 将抛出一个错误！在 JavaScript 中，每个函数只能返回一个值。

# 解决方案

那么这个难题的解决方案是什么呢？

我们将 JSX 包装成一个 div。

```
const App = () => {
    return( 
       <div>
         <p>Hello</p>
         <p>World</p>
      </div>
     )
  }
```

在幕后，这是它看起来的样子

```
Return (
     React.createElement('div', 'null',       
        [React.createElement('p','null','Hello'),
        React.createElement('p','null','Hello')]
        )
     )
```

App 函数只能返回一个值，通过使我们的 JSX 语句成为一个 div 的子语句，我们可以指定任意多的`createElement`来得到我们想要的输出。

现在这里的问题是，我们最终在最终的 HTML 中用无意义的 div 来膨胀 DOM。对于一个简单的组件来说，这可能不是问题，但是对于更复杂的组件来说，你可以想象这会如何影响组件和应用程序的最终运行。

您可能接触到的另一个问题是 React 应用程序中的布局。你可以想象一下，如果有 div 元素出现在不应该出现的地方，使用 FlexBox 来布局你的页面可能看起来不像你想要的样子。

如何处理这个问题，我们将在下一篇文章中看到！

# 其他文章

[](https://towardsdatascience.com/approach-to-learning-python-f1c9a02024f8) [## 学习 Python 的方法

### 今天如何最大限度地学习 python

towardsdatascience.com](https://towardsdatascience.com/approach-to-learning-python-f1c9a02024f8) [](https://towardsdatascience.com/efficient-web-scraping-with-scrapy-571694d52a6) [## 使用 Scrapy 进行有效的网页抓取

### Scrapy 的新功能使您的刮削效率

towardsdatascience.com](https://towardsdatascience.com/efficient-web-scraping-with-scrapy-571694d52a6) 

# 关于作者

我是一名执业医师和教育家，也是一名网站开发者。

请点击此处查看我的博客和其他帖子中关于项目的更多细节。

我将非常感谢任何评论，或者如果你想与 python 合作或需要帮助，请联系我。如果你想和我联系，请点击这里