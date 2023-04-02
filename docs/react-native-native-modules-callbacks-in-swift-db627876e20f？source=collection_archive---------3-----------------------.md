# 反应本地 Swift 中的本地模块回调

> 原文：<https://javascript.plainenglish.io/react-native-native-modules-callbacks-in-swift-db627876e20f?source=collection_archive---------3----------------------->

![](img/0c9b607c5e4b3d6b212fc9706f9ac4f7.png)

# 介绍

回调是在运行本机函数后将数据发送回 React Native 的一种很好的方式。但这并不是使用它们的唯一原因。在我的例子中，我将扩展我之前关于本机模块的教程，我们将创建一个函数，通过使用 React Native **回调**返回一个布尔值，无论音频文件是否正在播放。

在前面的教程中，组件一安装，音乐就开始播放。现在，我们将创建一个按钮，当按下时调用回调。onPress 函数将调用一个 Swift 函数，返回播放器是否正在播放音频文件！

# 创建按钮

打开示例文件夹中的 **App.js** 文件。

在我们的渲染函数中，我们将创建一个按钮，当按下它时，将调用一个名为 isPlaying 的本地函数。

```
render() {
   return (
      <View style={styles.container}>
         <Text style={styles.welcome}>☆AudioHelper example☆</Text>                              <TouchableOpacity
            onPress={() => AudioHelper.isPlaying((error, data) => {
               if (error) { 
                  console.log(error);
               } else {
                  console.log(data);
               }
           })}
         > <Text style={{backgroundColor: "#DDD", padding: 5}}>
                 Is song playing?
            </Text>
         </TouchableOpacity>
      </View>);
}
```

不要忘记导入顶部的 **TouchableOpacity** ，如果文件:

```
import React, { Component } from 'react';
import { StyleSheet, Text, View, **TouchableOpacity** } from 'react-native';
import AudioHelper from 'react-native-audio-helper';
```

现在让我们转到原生 iOS 和 swift 部分。使用 XCode 打开示例项目。iOS 目录下有一个项目文件。

我们将编辑两个文件。AudioHelper.m 和 AudioHelper.swift .所以，我们要做的第一件事就是在 React Native 和 swift 之间建立桥梁。打开 **AudioHelper.m** 文件。

在**playSound RCT _ 外部 _ 方法** : `RCT_EXTERN_METHOD(isPlaying(RCTResponseSenderBlock)callback)`下面添加这一行

现在我们能够创建我们的 swift 函数来检查我们的音频文件是否正在播放。

在 **AudioHelper.swift** 文件中，我们将创建与上面的**RCT _ 外部 _ 方法**的定义相匹配的函数。

```
**@objc(isPlaying:)
func** isPlaying(**_** callback: RCTResponseSenderBlock) {
   **if** (player != **nil**) {
      callback([player!.isPlaying])
   }
}
```

这个函数首先检查音频播放器是否存在:

`if player != nil`

如果有播放器，调用 React 本机回调函数，返回一个包含布尔 isPlaying 的数组。

`callback([player!.isplaying])`

我从苹果文档[这里](https://developer.apple.com/documentation/avfoundation/avaudioplayer)得到了可用函数和属性的列表。这是我在回调函数[这里](https://developer.apple.com/documentation/avfoundation/avaudioplayer/1390139-isplaying)中返回的属性。

# 复试

React 本机桥被认为是异步的，只有两种方式将结果传递回 React 本机项目。

*   复试
*   事件发射器

回调是两者中比较容易的。它允许您的 React 本机应用程序运行本机 iOS / Android 功能，并期望返回某种数据。

## 包扎

这只是一个让你开始使用 React 本机回调的简单例子。如果你还没有看完第一个教程，我建议你从第一个[开始。它将为您创建一个 React 本机模块打下更多的基础。](https://medium.com/javascript-in-plain-english/react-native-native-modules-with-swift-6768ea03b3f)