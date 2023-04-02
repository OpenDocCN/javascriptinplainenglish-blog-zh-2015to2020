# 通过深层链接轻松测试世博会应用

> 原文：<https://javascript.plainenglish.io/making-testing-expo-apps-easy-with-deep-links-1c3af12dcd74?source=collection_archive---------2----------------------->

![](img/f86eebe1396a9d28ff2438e92acc2d7a.png)

Photo by [Aaron Lau](https://unsplash.com/@cloud_symmetry?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[我花了七个月的时间用 Expo 构建了一个 React 原生应用，我真的很享受选择 Expo 带给我的灵活性。](https://medium.com/swlh/building-my-first-app-jiffycv-53192b24b386)

构建服务尤其为我节省了大量时间，让我可以花更多时间处理单调的应用商店提交，每次我这样做时，事情似乎越来越复杂。

虽然深层链接不是 Expo 独有的，但当你的应用程序在 Expo 客户端运行或作为独立应用程序运行时，你会面临一些额外的复杂性，两者都有不同的深度链接结构。

# 什么是深度链接？

深层链接(也称为自定义 URL 方案)是一种统一资源标识符(URI)，当在支持它们的设备上打开时，会打开相应的应用程序来侦听该特定的“方案”。然后，该应用程序可以使用 URI 将用户直接带到应用程序中的屏幕，或者设置应用程序状态，以便配置所需的行为。

例如，当在安装了脸书应用程序并支持深层链接的设备上访问深度链接`fb://profile/6661337`时，将为用户打开脸书应用程序，并使用提供的 ID 将他们带到个人资料页面。

# 为什么要使用深层链接进行测试？

除了能让用户在他们的设备上点击另一个应用程序的链接时启动你的应用程序，深度链接还有很多好处。

它们允许更简化的 UX，因为启动您的应用程序通常比等待链接加载到网页上更快，尤其是在用户需要进行身份验证并且他们已经登录到您的应用程序的情况下。

它们允许更快地进入特定的应用状态，例如将产品添加到购物篮或访问特定的帖子，而不需要用户自己执行进入该状态所需的操作，这可能意味着更高的参与度和转化率。

后一个方面是对测试最有用的方面，因为能够将应用程序设置为满足测试用例先决条件的状态可以节省时间并产生更一致的结果。

深层链接还允许定义路径和查询字符串参数，因此可以基于以这种方式传递给应用程序的值动态创建状态。

# 处理世博会中的深层链接

为了给你的 Expo 应用建立深层链接，你首先需要决定一个方案名称。

这可以是你的应用程序名称或另一个字符串，如果你需要分解字符串，可以包含`.`、`+`或`-`，尽管我会避免这些以确保最大的兼容性。

一旦你决定了一个方案，你就把它添加到你的`expo.scheme`属性下的`app.json`文件中。

The scheme determines the protocol part of the Deep Link URI, for instance exampleapp://

这将向应用程序的独立版本添加适当的条目，以便它与使用该方案的 URIs 相关联。

然后在你的应用程序代码中，你可以使用`expo-linking`库来调用`getInitialURL()`函数，当应用程序通过深度链接启动时，它将返回深度链接 URI 的一个承诺。

当应用程序在前台运行时，为了检测深层链接，你可以使用`addEventListener('url', callback)`为 URIs 设置一个监听器。

前台事件侦听器对于处理身份验证流非常有用，在这种情况下，您会将用户发送到网站进行登录，并通过深度链接将他们重定向到应用程序。它对于测试也很有用，因为您不需要关闭并重新启动应用程序来更改应用程序的状态。

一旦你有了深度链接 URI，你可以使用`parse()`从中提取不同的组件，这也将解释世博会客户端和独立 URIs 之间的差异。

我发现使用`getInitialURL()`的最好地方是 Expo `AppLoading`组件的`startAsync`函数内部。

The AppLoading component allows for async operations to be completed within the main entry point, this is useful as this entry point cannot be an async function

这允许解析深度链接，并允许您在应用程序仍在加载时设置应用程序的状态，例如，如果您想要使用深层链接将应用程序置于特定状态以进行测试或演示其功能，这将非常有用。

## 独立应用程序和世博客户端应用程序在 URI 的差异

在开发过程中，您可能会使用 Expo 客户端应用程序运行您的应用程序。Expo 客户端让你在开发的时候测试你的应用变得非常容易，但是它会让你很难使用深层链接。

Expo 客户端不会使用您在`app.json`中定义的深度链接方案，因为它有自己的模式`exp://`，并要求您的路径以 Expo 服务器的主机名为前缀，以便从和`--/` 中获取应用程序文件。

对于在本地 Expo 服务器上运行的应用程序，您可以在二维码上方的 Expo 开发者工具页面或运行该流程的终端中找到要使用的主机名，同样在二维码上方。

对于在本地运行的应用程序，生成的 URI 看起来像`exp://[HOSTNAME AND PORT]/--/testing/reset-db`,对于发布到 Expo 的应用程序，看起来像`exp://exp.host/@[USERNAME]/[APP SLUG]/--/testing/reset-db`。

独立应用程序将使用在`app.json`中定义的方案，既不需要主机名也不需要`--/`前缀，因此上面的示例是针对独立应用程序的`[APP SCHEME]://testing/reset-db`。

为了在我的基于 Appium 的自动化测试中处理这个问题，我传入了一个环境变量，在针对 Expo 客户端和独立应用程序运行测试套件时，我可以用它来创建适当的深度链接。

In getDeepLinkUri optional behaviours can be passed in to create Deep Links that set the app state into a specific configuration

# 触发深层链接进行测试

有很多方法可以触发深层链接，有些更适合在设备上手动测试，有些更适合自动化测试。

## 使用网络浏览器

深度链接旨在弥合应用程序和外部服务之间的差距，所以你会认为从网络浏览器内部启动深度链接是很自然的事情。

Android Chrome 不允许在地址栏中直接输入自定义方案，这样做只是执行谷歌搜索。相反，您需要创建一个带有锚标记(`<a>`)的网页，并将深度链接作为`href`属性。

在 iOS 上，你可以在 Safari 的地址栏中输入深度链接 URI，它会询问你是否要打开该应用程序。与 Android 类似，这在 iOS 版 Chrome 上也不起作用。

## 二维码

这是我在使用实际设备时触发深层链接的首选方法，尤其是在设备没有插入计算机进行调试的情况下。

这个过程是使用像 [QR 码生成器](https://www.qr-code-generator.com/)这样的工具为深度链接 URI 生成一个 QR 码，然后使用设备上的相机应用程序扫描它。

在 iOS 上，你可以使用内置的相机应用程序，当扫描二维码时，它会问你是否要打开应用程序，在 Android 上，如果你的相机应用程序不支持二维码，你可能需要下载二维码扫描应用程序。

我喜欢这种方法，因为二维码易于创建、共享和扫描，而且你不用在复杂的 URIs 中输入，你不太可能犯错误，这加快了测试过程。

## Android 调试桥(ADB)

如果你在本地 Android 模拟器上运行你的应用，或者你的 Android 设备已经接入，并且开发者模式已经激活，你可以使用`adb`为你触发深度链接。

你需要将`adb`安装到你的电脑上，并在你的路径中设置好，这样做是在模拟器上触发深度链接最简单的方法。

为了触发深度链接，你需要在终端上运行`adb shell am start -d [DEEP LINK URI]`，设备会立即打开你的应用程序(不像 iOS 会问你是否要打开应用程序)。

## xcrun 命令

对于 iOS 来说，相当于`adb`的是`xcrun`，以类似的方式，你可以用它来触发深层链接。

`xcrun`是在您安装 Xcode 命令行工具时安装的，并且仅在 Mac 上可用。像`adb`一样，这种方法只对本地模拟器和插入式设备有用。

为了触发深度链接，你需要在你的终端上运行`xcrun simctl openurl booted [DEEP LINK URI]`，设备会询问你是否要打开你的应用程序。

## Appium

Appium 是我选择的自动化框架，它有一个快捷方式，可以很容易地在 Android 上启动深层链接。

不幸的是，iOS 就不一样了。令人欣慰的是，一位 [ppium Pro 发表了一篇关于深度链接和 Appium](https://appiumpro.com/editions/84-reliably-opening-deep-links-across-platforms-and-devices) 的文章，这对理解你的选择非常有用。

要使用 Appium 在 Android 上启动深度链接，您可以使用`client.execute`命令`mobile:deeplink`，并提供一个带有 URI 和包的选项对象:

mobile:deepLink only works on Android

要使用 Appium 在 iOS 上启动深度链接，您需要使用 Safari 在地址栏中输入深度链接，并接受提示以启动应用程序(您可以在 Appium 上设置自动接受提示的功能以加快速度)。

如果不是因为 iOS 倾向于在版本之间重命名元素，这将是非常容易的，因此针对最新的 iOS 版本运行自动化可能需要一些努力来更新这个过程。

I find it easier to wrap the behaviours needed for each iOS version in a function

## 世博会客户历史

如果您正在使用 Expo 客户端测试您的应用程序，并且您之前已经使用深度链接启动了该应用程序，它会将 URI 存储在其“最近打开”列表中。

按下这些列表项中的一项将启动与之前相同的 URI 应用程序，但是值得注意的是，当测试运行在本地网络上的 Expo 服务器或通过“隧道”选项时，您可能会发现主机名已改变，这些链接将不起作用。

如果你正在测试一个使用前台深度链接监听方法的应用程序，这可能是一个改变应用程序状态的非常简单的方法，因为你可以调出 Expo 客户端菜单，返回 Expo 客户端主屏幕，从列表中选择一个链接，然后返回应用程序，根据需要设置状态。

# 摘要

深度链接是一个非常有用的测试工具，因为它们允许你轻松地配置应用程序并启动到一个设置的应用程序状态，或者如果使用前台方法，甚至更新应用程序状态而无需重新启动应用程序。

除了在 Expo 客户端和独立应用程序上使用深度链接的不同，Expo 使得实现和使用深度链接变得非常容易，这使得让你的应用程序可测试变得非常简单。