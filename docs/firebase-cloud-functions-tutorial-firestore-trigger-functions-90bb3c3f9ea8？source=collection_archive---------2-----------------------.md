# Firebase 云函数教程— Firestore 触发函数

> 原文：<https://javascript.plainenglish.io/firebase-cloud-functions-tutorial-firestore-trigger-functions-90bb3c3f9ea8?source=collection_archive---------2----------------------->

![](img/f02a294c56b687d448b4f827ab3e87bb.png)

Cloud Firestore Triggers

众所周知，Firebase 是我最喜欢的工具之一。他们的团队使得在创纪录的时间内开发一个应用程序变得如此容易。在我的上一篇文章中，我们介绍了如何使用 Firebase 函数创建一个 [REST API。虽然这是一个好的开始，但是还有很多事情要做。](https://medium.com/javascript-in-plain-english/firebase-cloud-functions-tutorial-creating-a-rest-api-8cbc51479f80)

在本教程中，我们将介绍 Firebase 函数触发器，以及如何使用它们来简化开发。有了它们，我们可以监听 Firestore 表、存储桶、身份验证请求等等。然后我们可以在特定事件发生时执行代码。

在本教程中，我们将介绍云 Firestore 触发功能。这些只是您可以采取的不同侦听器和操作的几个示例。显然，您还可以做更多的事情，但是这应该会让您对这些事情有一个很好的了解。

# 云 Firestore 触发器

云 Firestore 触发器允许您在编写、创建、更新或删除文档时执行代码。您需要做的就是指定文档路径和事件。那么当特定事件发生时，您将触发触发器。

# 视频教程

Cloud Firestore Triggers Video Tutorial

## onCreate

这将在首次写入文档时触发。我最喜欢在发送推送通知的时候使用它。您可以创建一个通知表，并在每次想要发送推送通知时写入该表。

**文档结构** 在通知集合内部，从具有以下结构的客户端或 API 创建文档:

```
{
  fcmToken: "1234",
  message: "Test push notification,
  title: "Test"
}
```

**onCreate Cloud Firestore 触发器** 在上述文档创建完成后，该触发器将被触发:

onCreate Cloud Firestore Trigger for Push Notifications

## onUpdate

Firestore 等 NoSQL 数据库最大的优点之一就是可伸缩性。但是，为了获得可伸缩性，您将复制数据。如果用户在一个表中更新他们的数据，而您还需要更新另一个表中的数据，这可能会变得很麻烦。

例如，如果我们有一个带有用户名的用户集合，但也有一个引用该用户名的评论集合。当用户在用户集合中更新用户名时，我们也需要在评论集合中更新用户名。

**用户文档**

```
{
  firstName: "Diligent",
  lastName: "Dev",
  userName: "TheDiligentDev"
}
```

**审查文件**

```
{
  userName: "TheDiligentDev",
  rating: 5,
  text: "Great Article!"
}
```

**onUpdate 云 Firestore 触发器**

onUpdate Cloud Firestore Trigger

## onDelete

onDelete Firebase 函数触发器是另一个清理数据的好工具。例如，假设您有一个帖子集合，每个帖子都有一个图像数组。如果一篇选中的文章被删除，你会想要删除所有相关的图片来清理你的存储桶。

**发文**

```
{
  title: "Example Post,
  content: "Example post content",
  images: ["test-1.png", "test-2.png"]
}
```

**onDelete Cloud Firestore 触发器**

在删除上面的文档时，这个触发器将触发并清除任何帖子图像。

onDelete Cloud Firestore Trigger

# 结论

这应该会让您对使用 Firebase 函数触发器可以自动化的一些任务有一个很好的了解。在以后的文章中，我将介绍您可以利用的其他触发因素。如果你有任何问题、意见或顾虑，请给我留言。下次见，编码快乐！

## 来自简明英语团队的说明

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****