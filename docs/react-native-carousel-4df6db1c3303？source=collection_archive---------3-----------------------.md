# 反应本机旋转木马

> 原文：<https://javascript.plainenglish.io/react-native-carousel-4df6db1c3303?source=collection_archive---------3----------------------->

## 制作响应式水平旋转木马

![](img/6747baa53dcef3640034f99bc2796a1f.png)

Photo by [ckturistando](https://unsplash.com/@ckturistando?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/carousel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我将在 React Native 上创建一个漂亮而有效的旋转木马。如果你不知道什么是`Carousel`，它是一个水平顺序的动态滚动元素列表，其中上一个和下一个元素是部分可见的。

乍一看，这似乎很复杂，幸运的是，我会告诉你如何一步一步地容易。让我们一起变魔术吧。

# 设置项目

在我们开始编写 carousel 逻辑之前，我们必须在 React Native 上创建一个应用程序。我们可以通过运行下面几行代码来创建应用程序:

```
react-native init react_native_carousel
cd react_native_carousel
npm run ios
```

太好了，我们已经成功创建了 React 本机应用程序。我们准备好编码了！💪

# 制作旋转木马

我将使用`[react-native-snap-carousel](https://github.com/archriss/react-native-snap-carousel)` lib 来制作水平响应转盘。这是 React Native 上的一个有用的库，具有预览、多种布局和对大量项目的高性能处理。兼容安卓& iOS。您可以使用以下命令安装它:

```
npm install --save react-native-snap-carousel
```

接下来，让我们为转盘定义常数:

```
// carousel slider width
export const SCREEN_WIDTH = Dimensions.get('window').width;
export const CAROUSEL_VERTICAL_OUTPUT = 56;
export const CAROUSEL_ITEM_WIDTH = SCREEN_WIDTH - CAROUSEL_VERTICAL_OUTPUT;
```

之后，我们将编写两个主要函数`renderItem`和`renderPagination`。我们将对轮播内容使用常量数据。这里有一个`[data of carousel](https://github.com/Gapur/react-native-carousel/blob/master/constants.js)`的链接。此外，我们将在本地状态中存储当前活动的幻灯片，并在转盘改变幻灯片时更新它。

# 更多动画

React Native 为动画提供动画 API。动画 API 侧重于输入和输出之间的声明性关系，中间有可配置的转换，以及控制基于时间的动画执行的启动/停止方法。

我们将使用`Animated.spring`为我们的应用制作动画。它跟踪速度状态，以在 to 值更新时创建流体运动。当转盘滑道改变时，我们将改变`backgroundColor`。

上面，我们使用`useRef`来保存动画值。`useRef`返回一个可变的 ref 对象，其`.current`属性被初始化为传递的参数。

我们应该通过在动画配置中指定`useNativeDriver: false`来使用本地驱动。因为本机驱动程序不支持 Animated.spring。

# 让我们演示一下我们的旋转木马

![](img/d77590fe942ad86272b724508afd5e48.png)

React Native Carousel

如果你想检查所有代码，这里有 [Github](https://github.com/Gapur/react-native-carousel) 的链接。

感谢阅读，希望这篇文章对你有用。编码快乐！

# 资源

[](https://medium.com/@rossbulat/react-native-carousels-with-horizontal-scroll-views-60b0587a670c) [## React Native:带有水平滚动视图的旋转木马

### 如何使用本地组件创建流畅且响应迅速的传送带

medium.com](https://medium.com/@rossbulat/react-native-carousels-with-horizontal-scroll-views-60b0587a670c) [](https://github.com/Gapur/react-native-carousel) [## Gapur/react-native-carousel

### 🤠React-Native dissolve GitHub 上的响应式水平旋转木马是超过 5000 万开发者共同工作的家园…

github.com](https://github.com/Gapur/react-native-carousel) [](https://github.com/archriss/react-native-snap-carousel) [## arch riss/react-native-snap-carousel

### 你想了解更多吗？这些是我们创建的大量使用插件的实时应用。不要害羞，分享一下…

github.com](https://github.com/archriss/react-native-snap-carousel)