# 您的通知在 React Native 中不起作用的 5 个可能原因

> 原文：<https://javascript.plainenglish.io/5-possible-reasons-your-notifications-are-not-working-react-native-d5c5a35ae3f?source=collection_archive---------1----------------------->

## react-native-fire base-通知& & react-native-push-通知

![](img/1b98551b38e94636bc2bde3d49ff2412.png)

Photo by [Jamie Street](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 库配置

无论您使用 react-native-firebase 还是 react-native-push-notifications，您都需要为每个本机平台做一些预先配置，正如您可能预料的那样，如果做得不正确，事情将不会对您起作用。

这两个平台的配置都非常简单，所以你可以在这里查看 firebase 的[和 react-native-push-notification 的](https://v5.rnfirebase.io/docs/v5.x.x/notifications/android)[。如果你有任何问题，请在下面留下你的评论。](https://github.com/zo0r/react-native-push-notification)

## Firebase 控制台与项目配置不匹配

如果您正在管理多个 firebase 项目，请确保使用正确的控制台来测试正确的项目。例如，我发现自己将测试通知从一个项目的控制台推送到另一个项目的客户端，这让我头疼了好一阵子。只需确保您的 google-services.json 文件与您的 firebase 控制台或后端服务器配置相匹配。

## 尚未创建频道(客户端)

至少 Android 需要这个来正确处理不同声音和行为的不同类型通知。例如，渠道是你可以禁用“来自你认识的人的推文”的通知，而不用禁用 Android Twitter 的“直接消息”的通知的原因。

react-native-push-notifications 库创建了一个默认通道来处理这些通知，但可能不适合您的用例。因此，如果您发现您的背景通知没有弹出或使用您拥有的特殊声音，您可能需要创建一些自己的通道。

## 尚未指定推送通道(服务器)

一旦您创建了自定义频道，下一步就是确保将您的通知从您的服务器或控制台推送到这些频道，就像您将信件发送到收件人的邮箱一样。

从节点服务器来看，可能是这样的:

Firebase messaging push to client with channel_id

这些通知库的大量文档已经可以在官方回购和网站以及类似的文章中找到，但如果你想让我写一篇关于我如何设法使用其中一个库的文章，请在评论中告诉我，我会尽快处理，否则你应该可以从这一点开始:)