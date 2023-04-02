# 使用框架/内容加载器提高 React 应用的 UX

> 原文：<https://javascript.plainenglish.io/improving-the-ux-of-react-apps-with-skeleton-content-loader-f094e7ada4b6?source=collection_archive---------9----------------------->

## 在你的应用程序的某些地方去掉加载屏蔽，代之以内容加载器，以获得更好的用户体验。

现在许多应用程序更喜欢内容加载，以获得更好的用户体验。让我们先来看看什么是框架加载？

![](img/c269b33d733eb191b4f6877389b9530f.png)

Icons made by [https://www.flaticon.com/authors/freepik](https://www.flaticon.com/authors/freepik)

## 骨架/内容加载屏幕

框架屏幕是 UI 的一个版本，它不包含实际的内容/信息，但是它包含了实际内容显示时页面看起来的样子。这意味着当所有信息都加载到屏幕上时，它将显示布局的实际形状。

这些屏幕通常包含屏幕上不同类型元素的占位符，如文本、图像、视频等。

## 使用骨架屏幕的优势

框架屏幕有时会隐藏您的性能问题，而不是在页面中央显示一个老式的加载掩码，用户很难持续盯着加载程序🤪用户肯定会喜欢看加载屏幕。

*   内容加载器有助于在加载之前模拟您的 web/移动应用程序屏幕的外观
*   您可以根据您的内容布局显示不同类型元素的加载，如图像、文本、视频或任何其他部分(许多科技公司在加载内容时显示骨架屏幕，以提高 UX。)
*   目标是设计一种减少等待时间的感觉。如果用户已经知道菜单中的下一步是什么

现在，为了让您对中的加载程序有更多的了解，让我们看看下面的例子

![](img/10b6c2f7ef98c433ea78a74c1cedb1f2.png)

Skeleton screen of different apps

## 如何使用 React 实现这一点？

好消息是，有许多现成的软件包可供您开箱即用。以下是您可以在应用程序中尝试的一些软件包

*   [react-content-loader](https://danilowoz.com/create-content-loader/)(136，118 周下载量，9K🌟)
*   [反应-加载-骨架](https://github.com/dvtng/react-loading-skeleton)(周下载 54298，1.3K🌟)
*   [react-placeholder](https://github.com/buildo/react-placeholder)(22808 周下载量，1.2K🌟)

# **代码时间⚡️**

现在我们知道了可以用来实现这种内容加载的库，让我们开始查看需要添加到应用程序中的代码

## 装置

```
npm i react-content-loader — save OR yarn add react-content-loader
```

## 使用

1.使用现成的预置

```
import ContentLoader, { Facebook } from 'react-content-loader'

const MyLoader = () => <ContentLoader />
const MyFacebookLoader = () => <Facebook />
```

2.自定义模式，参见[在线工具](https://danilowoz.github.io/create-react-content-loader/)

```
const MyLoader = () => (
 <ContentLoader viewBox=”0 0 380 70">
 {/* Only SVG shapes */} 
 <rect x=”0" y=”0" rx=”5" ry=”5" width=”70" height=”70" />
 <rect x=”80" y=”17" rx=”4" ry=”4" width=”300" height=”13" />
 <rect x=”80" y=”40" rx=”3" ry=”3" width=”250" height=”10" />
 </ContentLoader>
)
```

## 演示时间🧨

沙盒信用—[https://codesandbox.io/u/shaunwallace](https://codesandbox.io/u/shaunwallace)

链接到 code sandbox—[https://codesandbox.io/s/2wp1zoj09j](https://codesandbox.io/s/2wp1zoj09j)

快乐学习！😄 💻