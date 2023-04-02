# 在 React Navigation 5 中创建嵌套导航器

> 原文：<https://javascript.plainenglish.io/create-nested-navigators-in-react-navigation-5-3b294a71fe29?source=collection_archive---------4----------------------->

## 集成标签栏和可扩展的堆栈导航

![](img/4e0abe462ead8510ec9d6fb92bce3c1a.png)

## 入门指南

我参与过两个有导航问题的项目。这些问题使得设计规格难以匹配。设计要求某些视图覆盖整个屏幕，包括底部标签栏。堆栈导航器嵌套在选项卡导航器中。以这种方式嵌套堆栈导航器并不能让您隐藏选项卡栏，因为每个导航器都有自己的导航历史。

让我们看看如何在不牺牲功能的情况下实现嵌套导航器。

## 安装 React 本机和依赖项

创建一个新的 React 本地项目。我在下面的官方[入门](https://reactnavigation.org/docs/getting-started)文档中列出了这些步骤:

`npx react-native init nested-stack-navigation`

项目创建后，我安装了所有与 React Navigation 5 相关的依赖项。

`npm install @react-navigation/native`

`npm install @react-navigation/bottom-tabs`

`npm install @react-navigation/stack`

在我们开始之前，确保项目可以运行:

`react-native run-ios`

您可能会得到以下错误消息: *null 不是一个对象(计算' RNGestureHandlerModule。方向’)*

如果是这样，下面的解决方案解决了它:

1.  将目录切换到`ios`文件夹；
2.  运行`pod install`，这将安装必要的软件包来消除错误。

*包 react-native-gesture-handler*没有正确安装，因此需要手动安装。我在 GitHub 上找到了同样问题的另一个解决方案，以防`pod install`不够用。

## 嵌套导航器的目的

嵌套导航器在 React 本机应用程序中结合了多种导航类型。您可以将堆栈导航器嵌套在一个抽屉中，并允许在该抽屉中显示其他屏幕。另一个例子是使用选项卡导航器显示应用程序主屏幕，然后使用堆栈导航器显示与每个选项卡相关的更多屏幕。

## 嵌套多个堆栈导航器

“嵌套导航器意味着在另一个导航器的屏幕内呈现一个导航器”( [React 导航文档](https://reactnavigation.org/docs/nesting-navigators/))

我工作过的一个项目将选项卡导航器作为最上面的导航器，如下所示:

*   标签。航海家
*   首页(堆栈。导航员)
*   设置(屏幕)
*   配置文件(堆栈。导航员)

主页、配置文件和设置屏幕是选项卡栏的一部分；每个都有自己的堆栈导航器。这使得每个标签都有自己的导航堆栈，可以在其中推送更多屏幕。这最终有一个缺陷，就是没有办法隐藏底部的标签栏。

当您希望屏幕覆盖所有元素(包括标签栏)时，上面的导航结构不起作用。反应导航文档说明每个导航器都有自己的私有选项参数。

当在子导航器中设置时，选项卡导航器不会接收到`tabBarVisible`参数。

为了解决这个问题，嵌套导航必须改变。

*   堆栈。航海家
*   主页(选项卡。导航器)
*   配置文件(屏幕)
*   设置(屏幕)

它可能看起来不太有条理，但是堆栈导航器需要是根导航器，以实现期望的全屏效果。

"把嵌套导航器看作是实现你想要的用户界面的一种方式，而不是组织你的代码的一种方式。"([反应导航文档](https://reactnavigation.org/docs/nesting-navigators))

## 嵌套导航器会影响行为

在嵌套导航器时，有一些事情需要记住。嵌套的导航器不会接收父事件，每个导航器都有自己的选项集，每个导航器都有自己的历史。

导航器有它们自己的私有选项参数，这就是为什么在子级中设置`tabBarVisible`时，堆栈导航器将不起作用。

## 创建基本屏幕组件

开始时，为每个屏幕创建基本组件。

```
import * as React from 'react';
import {Text, View} from 'react-native';

function HomeScreen({navigation}) {
  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <Text onPress={() => navigation.push('Profile')}>Go to Profile</Text>
    </View>
  );
}

function ProfileScreen({navigation}) {
  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <Text
        onPress={() => {
          navigation.goBack();
        }}>
        Go Back
      </Text>
    </View>
  );
}

function SettingsScreen({navigation}) {
  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <Text onPress={() => navigation.navigate('Profile')}>Settings</Text>
    </View>
  );
}
```

## 主堆栈屏幕

下一步是创建主堆栈。主堆栈应具有以下内容:

*   选项卡导航器
*   将成为选项卡栏一部分的所有屏幕

```
import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';

const HomeTab = createBottomTabNavigator();
function HomeTabs({navigation, route}) {
  return (
    <HomeTab.Navigator>
      <HomeTab.Screen name="Home" component={HomeStackScreen} />
      <HomeTab.Screen name="Settings" component={SettingsScreen} />
    </HomeTab.Navigator>
  );
}
```

## 创建堆栈导航器

堆栈是导航容器中的根导航器。主堆栈组件位于选项卡栏组件所在的位置。配置文件屏幕位于选项卡导航器之外，因此它可以完全覆盖整个屏幕。

```
import {
  NavigationContainer,
  getFocusedRouteNameFromRoute,
} from '@react-navigation/native';

const RootStack = createStackNavigator();
export default function App() {
  return (
    <NavigationContainer>
      <RootStack.Navigator>
        <RootStack.Screen
          name="Home"
          component={HomeTabs}
          options={({route}) => ({
            headerTitle: getHeaderTitle(route),
          })}
        />
        <HomeStack.Screen
          name="Profile"
          component={ProfileScreen}
          options={() => ({headerTitle: 'My settings'})}
        />
      </RootStack.Navigator>
    </NavigationContainer>
  );
}
```

## 结论

这是创建嵌套导航器的基础。嵌套导航器时，建议最多只嵌套 2 个堆栈导航器。只有在绝对必要的情况下，才能超出这个范围。请记住，嵌套导航器是实现您想要的用户界面的一种方式。