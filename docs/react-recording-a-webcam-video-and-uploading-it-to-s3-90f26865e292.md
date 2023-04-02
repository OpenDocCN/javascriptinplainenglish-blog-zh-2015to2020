# 反应:录制一段网络视频并上传到 S3

> 原文：<https://javascript.plainenglish.io/react-recording-a-webcam-video-and-uploading-it-to-s3-90f26865e292?source=collection_archive---------6----------------------->

## 简短但详细的指南。

![](img/3013b1ea4b6c1bce81c175fe6fac8466.png)

还记得 20 年前，在电子邮件上使用彩色字体被视为令人兴奋的事情吗？人们很快就习惯了电子邮件的“新常态”，颜色不再令人兴奋。所以人们开始加入图片，然后是你的名字，最近是个性化的内容来适应你。但这种循环一直在继续，我们现在已经习惯了在电子邮件中加入个性化内容。

那么下一步是什么？邮件里有什么感觉很新鲜？嵌入式视频！

**将涵盖哪些内容:**

1.  使用网络摄像头在 React 中录制视频
2.  从网站访问 AWS 资源
3.  上传视频到 S3 &获得公共链接

# **用 React 录制网络摄像头视频**

## 要求:

*   通过网络摄像头录制视频和音频
*   录制时获得视频的实时反馈
*   录制后有回放选项

[React Video Recorder](https://www.npmjs.com/package/react-video-recorder) 是一个高度可配置的轻量级皮肤，覆盖了[风格组件的](https://www.npmjs.com/package/styled-components)视频组件。它提供了一个实用工具工具箱，可以根据您的需求定制录制和 [blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob) ，例如 MimeType、回放选项和图像叠加。这些都记录在他们的 Github 上。

## **反应元件**

# **创建您的 AWS 资源**

## **创建 S3 存储桶**

从 [AWS S3 控制台](https://console.aws.amazon.com/s3/)中，您可以创建您的铲斗。要在 AWS 帐户之外公开和查看视频链接，您必须关闭默认选项`Block All Public Access`。

## 亚马逊认知

当在浏览器中使用 AWS SDK 时，最好使用 Amazon Cognito 身份池，因为它提供了临时凭证，您可以在其中指定用户可以访问哪些资源。用户可以作为来宾(未经身份验证)或经过身份验证的用户访问您的 AWS 资源。在这篇文章中，我们将关注客人，但是如果你想了解更多关于认证用户的信息，你可以[这里](https://docs.aws.amazon.com/cognito/latest/developerguide/switching-identities.html)。

您可以从[控制台](https://eu-west-1.console.aws.amazon.com/cognito/federated)创建您的 Amazon Cognito 身份池。创建身份池时，勾选框`Enable access to unauthenticated users`以允许用户无需登录即可访问资源。

下一页将允许您选择希望通过 IAM 策略授予用户访问权限的资源。默认情况下，用户有权访问 Cognito services & analytics，因此我们需要允许访问 S3。在每个角色下，单击策略文档上的编辑，并添加以下内容:

确保用您的铲斗名称替换`<bucketname>`。

我们添加到现有文档中的惟一内容是能够将对象添加到特定的 S3 存储桶中。良好的做法是只授予限制潜在攻击者造成的任何损害所需的最低访问权限。创建身份池后，您还可以通过 [IAM 控制台](https://console.aws.amazon.com/iam/home)添加此权限。

# **上传到 S3**

您首先必须将 [AWS SDK](https://www.npmjs.com/package/aws-sdk) 安装到 React 应用程序中。

## **认证**

现在，我们可以添加我们的身份池。从 [Amazon Cognito 身份池控制台](https://eu-west-1.console.aws.amazon.com/cognito/federated)点击你刚刚创建的池，然后进入`Sample Code`。这将为您提供一个类似于下面的代码片段，其中包含您的身份池的唯一标识符。

**配置 S3**

AWS SDK 默认为最新版本的 S3 客户端。虽然大多数时候这是您的首选，但是如果 API 发生变化，这可能会导致问题。为了避免这种情况，您可以将 S3 API 版本全局锁定到特定版本。

要访问您的 S3 存储桶，您需要创建它的一个实例，该实例将允许您使用身份池中的用户凭据执行 CRUD 操作。

## 将视频添加到 S3

这里有几个属性需要分解:

*   **键:**要保存到 S3 的文件的名称。该键还考虑使用正斜杠(/)的文件结构，因此要将其保存到特定文件夹，可以使用 folder1/folder2/video.mp4。
*   **正文:**要上传的内容。
*   **内容类型:**这告诉亚马逊内容是什么格式。当通过 S3 提供的公共 URL 访问视频时，播放时需要它。
*   **ACL:** 亚马逊的访问控制列表(ACL)指定了对象上设置的权限。就像创建 S3 存储桶一样，存储桶中的对象默认为非公共的。要允许公开访问 URL，需要将其设置为`public-read`。

# **全部放在一起**

添加后，您的 S3 存储桶将包含您的视频，其 URL 遵循可公开共享的格式`https://<bucketname>.<region>.amazonaws.com/<videoname>`。

## 需要考虑的要点

1.  如果你计划在真实的站点上使用它，这个域必须有一个 SSL 证书。这是浏览器上的设计，因为通过 HTTP 传输网络摄像头内容是危险的。
2.  如果你只想将公共视频发送给一个用户，并且不希望视频被轻易发现，我建议你至少在名字前加一个 [UUID](https://www.npmjs.com/package/uuid) 。

*更多内容请看*[***plain English . io***](http://plainenglish.io/)