# 如何使用 JavaScript 本地通知 API

> 原文：<https://javascript.plainenglish.io/javascript-native-notifications-b579229fcb2e?source=collection_archive---------8----------------------->

## 简短有用的 JavaScript 课程。让它变得简单

![](img/025ce6772ebb967a731c5a9f2b97c217.png)

A laptop with code

通知 API 允许网页或混合应用程序发送通知，并在系统级别的页面外显示通知。API 允许 web 应用向用户发送信息，即使应用在后台或处于空闲状态。

## 请求许可

首先，用户需要授予 current origin 显示系统通知的权限。Notification.requestPermission()返回一个承诺。

您可以选择允许或阻止来自该来源的通知。一旦您选择了一个选项，浏览器通常会持续当前会话。

```
const promise = Notification.requestPermission();promise.then(function(result) {
  console.log(result);//granted: The user has granted permission to display 
           notifications,after having been asked previously.//denied:  The user has explicitly declined permission to show 
           notifications.
});
```