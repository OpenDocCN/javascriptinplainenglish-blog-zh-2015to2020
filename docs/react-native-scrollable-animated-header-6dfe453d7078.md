# 反应原生可滚动动画标题

> 原文：<https://javascript.plainenglish.io/react-native-scrollable-animated-header-6dfe453d7078?source=collection_archive---------1----------------------->

## 平滑动画标题

![](img/e06bfac20f219092783b34b11bf9693f.png)

Photo by [Franck V.](https://unsplash.com/@franckinjapan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/future?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如今，动画标题是当今应用程序中最常见的设计模式。动画是移动应用的重要组成部分。因此，我想在我的 React 本地应用程序中使用一个令人惊叹的动画标题。

我决定写这篇短文来展示如何构建一个带有 ScrollView 和动画 API 的标题。让我们一起做吧。

# 最终动画标题

![](img/a76ee99f16951fd1d848513feddc232d.png)

Scrollable Animated Header

如果你想检查所有代码，这里有 [Github](https://github.com/Gapur/react-native-scrollable-animated-header) 的链接。

# 设置项目

让我们创建一个名为`react_native_scrollable_animated_header`的新 React 原生项目:

```
react-native init react_native_scrollable_animated_header
cd react_native_scrollable_animated_header
npm run ios
```

现在，我们已经成功地创建了 React 本地项目。我们准备好编码了！

# 制作魔法代码

首先，我们需要为动画标题定义一些常量，这些常量将用于插入滚动位置值。

我将使用`faker.js`为`ScrollView`生成用户数据。`faker.js`是一个非常棒的生成虚假数据的库。您应该通过以下命令行安装它:

```
npm install faker --save
```

接下来，我将使用`ScrollView`组件显示用户数据。所以我们应该这样修改`App.js`文件:

之后，我们需要在`ScrollView`下创建标题。我们会用`Animated.View`。最终，它将被动画化。

# 魔幻动画

React Native 为动画提供动画 API。动画 API 侧重于输入和输出之间的声明性关系，中间有可配置的转换，以及控制基于时间的动画执行的启动/停止方法。

我们将使用`Animated.ScrollView`来制作滚动视图，并附加一个回调来在`onScroll`事件发生变化时监听它。然后，使用插值来映射 y 轴和不透明度之间的值。`The interpolation`将输入范围映射到输出范围，通常使用线性插值，但也支持缓动功能。

让我们用下面几行代码更新我们的`App.js`文件:

上面，我们使用`useRef`来保存动画值。`useRef`返回一个可变的 ref 对象，其`.current`属性被初始化为传递的参数。

接下来，我们需要在滚动`ScrollView`时更新动画值。我们将在`Animated.event`前捕捉事件。它直接映射到动画值`scrollY`，并在我们平移或滚动时更新它。

我们可以通过在动画配置中指定`useNativeDriver: true`来使用本地驱动程序。

让我们用下面的代码来制作我们组件的动画:

此外，我们将`scrollEventThrottle` 道具设置为 16，这样事件发送的频率就足够让动画流畅。

最后，我们的`App.js`:

# 结论

感谢阅读，希望这篇文章对你有用。编码快乐！

# 资源

[](https://medium.com/appandflow/react-native-scrollview-animated-header-10a18cb9469e) [## React 本机滚动视图动画标题

### 更新:更改了项目源以使用 Expo，并稍微更改了结构以使用 transforms 而不是…

medium.com](https://medium.com/appandflow/react-native-scrollview-animated-header-10a18cb9469e) [](https://medium.com/hackernoon/react-native-animated-header-using-animated-and-scrollview-9749255c149a) [## 使用动画和滚动视图的 React-Native 动画标题

### 动画是为移动应用创造良好用户体验的重要组成部分。这很关键…

medium.com](https://medium.com/hackernoon/react-native-animated-header-using-animated-and-scrollview-9749255c149a) [](https://www.skptricks.com/2019/06/react-native-scrollview-animated-header-example.html) [## React Native ScrollView 动画标题示例

### React 本机 ScrollView 动画标题示例。本教程解释了如何在滚动视图中创建动画标题…

www.skptricks.com](https://www.skptricks.com/2019/06/react-native-scrollview-animated-header-example.html) [](https://blog.codecentric.de/en/2019/07/react-native-animated-with-hooks/) [## 以代码为中心的 AG 博客

### 如何用 React 自带的 React 钩子的 React 原生动画动画系统制作动画？的目的是什么

blog.codecentric.de](https://blog.codecentric.de/en/2019/07/react-native-animated-with-hooks/) [](https://github.com/Gapur/react-native-scrollable-animated-header) [## gapur/react-原生-可滚动-动画-标题

### 用 ScrollView 反应原生动画标题。为 Gapur/react-native-scrollable-animated-header 开发做出贡献…

github.com](https://github.com/Gapur/react-native-scrollable-animated-header) 

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**