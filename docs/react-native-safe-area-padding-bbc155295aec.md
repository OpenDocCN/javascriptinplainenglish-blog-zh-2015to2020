# 如何让您的 React Native 应用程序在有缺口的手机上看起来仍然不错

> 原文：<https://javascript.plainenglish.io/react-native-safe-area-padding-bbc155295aec?source=collection_archive---------5----------------------->

![](img/6dccd6d953441fec15ce5e2831d46566.png)

Photo by [Bagus Hernawan](https://unsplash.com/@bhaguz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/iphone-x?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

“新的”苹果手机 X 和所有其他现在有缺口的新手机看起来都很棒，他们肯定做了正确的事情，否则我们就不会和他们一起走了。

不幸的是，你的移动应用需要的填充对每部手机来说都是不同的。Android 不同，iPhone X 不同于 iPhone 8 等等。幸运的是，有一个软件包可以帮助您。

反应原生社区的[反应原生安全区视图](https://github.com/react-native-community/react-native-safe-area-view)是一个了不起的反应原生社区建立的伟大的包。它是开源的，并且仍然不时更新。

# 建立以博览会回应本土

好吧，我将通过使用 [Expo](https://expo.io/) 构建一个示例来解释这一点，这是一个您可以使用的工具，也是 React Native 的团队实际上推荐的工具。

*提示:如果您已经知道如何使用 Expo 建立项目，您可以跳到下一个大标题。名为“添加安全视图”*

所以继续打开你最喜欢的工具。(我的是 Jetbrains 的网络风暴。很高兴成为一名学生。)并打开命令行。我假设你已经安装了 Expo，否则你可以按照这里的操作。

在控制台中运行以下代码，使用 Expo 初始化一个项目。当然，你可以把名字从`reactNativeSafeAreaExample` 改成你喜欢的任何名字。

```
expo init reactNativeSafeAreaExample && cd reactNativeSafeAreaExample
```

系统将提示您选择一个模板。用箭头键选择默认(空白)模板，然后按 enter 键。世博会现在将启动这个项目。

确保你的命令行也进入了新的文件夹，或者使用`cd`命令。

使用`yarn start`运行项目，并按照给定的说明在手机上打开应用程序。请确保在您的手机上安装 Expo 应用程序。您可以在[世博会](https://expo.io/)网站上找到使用说明。

## 删除样式

在这种情况下，Hello World 应用程序的默认样式是居中的文本。打开您的 App.js，在底部应该有一些如下所示的代码:

App.js

这是造型发生的地方。继续移除所有的造型。或者至少是`alignItems` 和`justifyContent`。如果你保存 app.js，你的手机或模拟器上的应用程序应该会更新，你的文本应该移到左上角的某个地方，没有填充。在 iPhone X 或类似产品上，它可能很难阅读，因为它在凹槽下面，在应该是透明的状态栏下面。

# 添加安全区域视图

在 mac 上按`ctrl + c`或`cmd + c`停止命令行。在项目文件夹中运行以下命令。

`expo install react-native-safe-area-view react-native-safe-area-context`

这将安装您需要的软件包。之后，您可以使用`yarn start`再次运行应用程序。

当软件包安装完成后，您需要将它添加到您的项目中。通过在顶部添加这一行来更新您的 App.js:

`import { SafeAreaProvider } from ‘react-native-safe-area-context’;`

你现在可以用新获得的`<SafeAreaProvider>`包围你的`<View>`。你的 App.js 看起来应该有点像这样:(不包括样式和其他导入)

App.js

接下来，您还需要通过在顶部添加这一行来导入`SafeAreaView`:

`import SafeAreaView from ‘react-native-safe-area-view’;`

导入后，您可以在 App.js 中更新您的 return 语句，以包含 SafeAreaView。

App.js

如果你现在在你的手机或模拟器上刷新你的应用程序，你会看到安全区域视图已经被应用，你的文本将再次可读。

## 在组件上使用安全区域视图

当然，你可能不希望在整个应用程序上应用 SafeAreaView。你可能需要一个背景颜色或其他特征，你仍然需要访问那个“不安全”的区域。这很容易做到。您可以创建一个新组件，而不是在 App.js 中包围主视图:

CoolText.js

我还在文本上添加了一些样式，但这是可选的。

接下来，我们需要导入安全区域视图并用`<SafeAreaView>`组件包围`<Text>`。这里不需要 safeAreaProvider。

CoolText.js

接下来，更新您的 App.js 以包含 CoolText 组件，并移除 SafeAreaView

App.js

现在你知道了。一个应用程序，使用安全区域视图和安全区域提供者。

# 结论

如果你想要更深入的信息，你可以查看这个网站的官方网站，或者如果你想看完整个例子，你可以在我的 GitHub 上查看。

我希望这是有帮助的，非常感谢您的阅读。