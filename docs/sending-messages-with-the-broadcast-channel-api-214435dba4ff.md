# 如何使用广播通道 API 发送消息

> 原文：<https://javascript.plainenglish.io/sending-messages-with-the-broadcast-channel-api-214435dba4ff?source=collection_archive---------5----------------------->

![](img/d31b62f3f62bd00cefb32eaabc27002e.png)

Photo by [Ricardo Gomez Angel](https://unsplash.com/@ripato?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

要在相同来源的浏览上下文(如窗口、选项卡、框架或 iframe)之间发送消息，我们可以使用广播通道 API。

在本文中，我们将研究如何使用它来发送和接收消息。

# 广播频道 API

为了使用这个 API，我们必须创建一个`BroadcastChannel`对象来监听底层通道。

然后，我们就可以接收任何发布到它上面的消息。我们不需要维护 iframes 的引用或者我们想要与之交流的工人。

相同来源的任何东西都可以通过创建一个`BroadcasChannel`来订阅一个特定的频道，然后它们之间将进行全双工通信。

我们可以如下使用它:

```
const bc = new BroadcastChannel("test_channel");
bc.postMessage("test");
bc.onmessage = ev => {
  console.log(ev.data);
};
bc.close();
```

在上面的代码中，我们创建了一个`BroadcastChannel`实例，然后调用`postMessage`并设置`onmessage`属性来监听发送的消息。

使用`data`属性访问数据。

完成后，我们可以调用`close`来关闭通道。

我们可以发送任何类型的对象，而不仅仅是字符串。

# 消息事件

`onmessage`处理程序中的`ev`参数是`MessageEvent`对象。

它有几个我们可以访问的只读属性:

*   `data` —消息发送方发送的数据。这将是我们传递到`postMessage`中的内容
*   `origin` —表示发出的消息来源的字符串
*   `lastEventId` —表示事件的唯一 ID 的字符串
*   `source`—`MessageEventSource`对象，(可以是`WindowProxy`、`MessagePort`或`ServiceWorker`对象)，代表消息发送者
*   `ports`—`MessagePort`对象的数组，表示与发送消息的通道相关联的端口。

# 处理错误

为了处理发送消息的错误，我们可以为广播频道对象的`onmessageerror`属性设置一个事件处理程序。

例如，我们可以写:

```
bc.onmessageerror = e => console.log(e);
```

然后我们可以记录出现的任何错误。

![](img/ca72171436a9d0c6fb1d7e1758611b0a.png)

Photo by [Pawel Kadysz](https://unsplash.com/@pawelkadysz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 名字

我们可以用`name`属性得到频道的名称。

为此，我们可以写:

```
const bc = new BroadcastChannel("test_channel");
console.log(bc.name);
```

然后我们可以从`console.log`中看到我们的广播频道名称是`test_channel`。

# 结论

广播通道 API 是一个非常简单的 API，它让我们可以进行跨上下文通信。

它可用于检测同一站点原始环境中其他选项卡中的用户操作，如用户登录帐户时。

没有消息协议，不同上下文中的不同文档需要自己实现它。

同样，也没有协商，这也不是规范所要求的。