# 将世博会的照片上传到 Firebase

> 原文：<https://javascript.plainenglish.io/upload-photos-from-expo-to-firebase-3051c80c23eb?source=collection_archive---------2----------------------->

![](img/90c0f9dee95d4f630a3a04fa700a1df8.png)

Expo + Firebase

***将您的图片或照片从您的 Expo 客户端上传到您的 firebase 存储器。***

# 重要编辑

自从 expo 更新以来，有了一种新的更好的方法，下面是它如何工作的一个例子:[https://github . com/Expo/examples/blob/master/with-firebase-storage-upload/app . js](https://github.com/expo/examples/blob/master/with-firebase-storage-upload/App.js)

# 过时的方式

有一天，我需要将一张用 ImagePicker 从 Expo 拍摄的照片上传到我的 firebase 存储，并将下载 url 保存到 firestore，我发现的一个解决方案是创建一个上传 base 64 的 firebase 函数。

问题是你不能使用 putString，因为 firebase 并不是真正为 expo 设计的，所以我们需要做一些变通。

首先，让我们创建 firebase 函数，我假设你已经知道如何创建一个，这是我使用的函数:

我正在手动创建下载 url，但是如果需要对 url 的权限，并且它对您不起作用，您可以总是使用 [getDownloadUrl()](https://firebase.google.com/docs/storage/web/download-files) 从 ref 到图像。

现在，要将 base64 字符串发送到我们刚刚创建的 firebase 函数，您需要这样的东西

我不知道这是否是最好的方法，但 firebase 功能定价非常便宜，firebase 定价的问题应该是您的 firestore 使用情况，这可能不是最快的方法(上传图像需要几秒钟)，但它对我有效，我希望它对您有效。

感谢阅读💖