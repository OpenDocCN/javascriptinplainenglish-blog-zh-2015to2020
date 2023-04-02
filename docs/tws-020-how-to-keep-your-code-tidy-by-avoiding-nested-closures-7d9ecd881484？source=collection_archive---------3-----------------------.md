# 如何通过避免嵌套闭包来保持代码整洁

> 原文：<https://javascript.plainenglish.io/tws-020-how-to-keep-your-code-tidy-by-avoiding-nested-closures-7d9ecd881484?source=collection_archive---------3----------------------->

![](img/6bd4ef9da1c69344924e19d8aa6bdfc9.png)

## JavaScript 使用“绑定”进行干净回调的模式

对于初学者来说，JavaScript 的“this”可能是个问题，闭包也带来了自己的一系列问题。ES2015 中引入的“绑定”功能解决了这些问题，并使您的代码更加清晰。在这一集里，Antoní分享了一个被遗忘的处理 ECMAScript 的“这个”的模式。

Ivana 刚刚修复了一个在`rwserve`库中发现的模糊错误。而且时间也创下了记录—从结账到拉取请求只需两个小时。她自己也很惊讶。在所有的测试用例中，没有任何问题发生，代码评审也没有任何评论就通过了。

诚然，当吉拉问题被分配给她时，她有点紧张。代码库很大，其中有许多她从未使用过的特性。此外，这将是她第一次实际处理代码。

她像往常一样开始了。重现 bug。。。找到可能是它来源的模块。。。将调试器设置为在进入模块时中断。。。逐句通过输入失败的代码。。。检查可能的变量。。。注意意外的状态变化。所有标准故障排除步骤。

就在那里。几分钟之内她就知道了问题所在，并且在不到一个小时的时间里就想出了纠正的方法。

测试。提交。拉取请求。代码审查。准备部署。

当她四处闲逛时，她注意到几乎每个模块都是由安东尼创作的。因为她之前发现他很平易近人，所以她去了他的办公室聊天。

她试探性地说，“所以我第一次承诺了一个`rwserve`安全模块，”然后在门口等着看他是否会参与进来。

安东尼抬起头，“嘿，伊万娜。太酷了。我不知道你被分配了维护问题。你需要我帮忙吗？”

“没有。我已经完成了，它将在周四部署。”

“恭喜你！”

“谢谢。所以我想提一下，我发现弄清楚发生了什么真的很容易。你的代码一点也不像迪文或肯的。关于它有一些东西让它清楚地知道发生了什么。我不确定这是什么，但我能够在不迷路的情况下读懂它。

“哦，这一定是我的普利策奖获奖评论，”他开玩笑说。

“嗯，*是这样的，*”她配合道，“但是我想我很惊讶你是如何能够在没有嵌套回调的情况下构建这么大的库，并且从来没有对`this`感到困惑。我讨厌`this`，并试图避免它，因为它似乎是给我带来这么多麻烦的根本原因。”

![](img/a81f429409404d6466376a53e26e5a4c.png)

onConnect, onStream, onTimeout

“哦，那个，”他回答道，“我想你可能没有领会到秘制酱料。这是我使用`bind`的方式。

他走到白板前，擦掉白板上红色和黑色的涂鸦。“让我向您展示如何设置 SessionMonitor 模块。它做什么并不重要，重要的是它如何做。”

“这就是我要说的。模块定义了一个类，所有的东西都在它的构造函数中初始化，就像这样——“

```
constructor(session, sessionId) {
    this.session       = session;
    this.sessionId     = sessionId;
    this.streamId      = 0;
    this.remoteAddress = null; this.boundConnect  = this.onConnect.bind(this);
    this.boundStream   = this.onStream.bind(this);
    this.boundTimeout  = this.onTimeout.bind(this);
    this.boundUncaught = this.onUncaughtException.bind(this); session.on('connect',           this.boundConnect);
    session.on('stream',            this.boundStream);
    session.on('timeout',           this.boundTimeout);
    session.on('uncaughtException', this.boundUncaught);
}
```

“这种模式很容易发现。首先，任何将在*回调*角色中被触发的函数都被绑定到类实例。然后，将捕获这些回调的每个侦听器都使用这些绑定的函数名进行注册。

这样，当回调发生时，绑定函数的`this`总是 SessionMonitor 的类实例。

下面是类方法的快照。不完全是，但足以抓住要点。

```
onConnect(serverHttp2Session, tlsSocket) {
    this.remoteAddress = tlsSocket.remoteAddress;
    serverHttp2Session.setTimeout(30*1000);
}onStream(stream, incomingRequestHeaders) {
    log.trace(`SID=${this.sessionId}`);
    this.streamId++;
    ...
}onTimeout() {
    this.session.close();
}onUncaughtException(error) {
    log.trace(`RA=${this.remoteAddress}`);
    log.trace(`SID=${this.sessionId}`);
    log.trace(`STRM=${this.streamId}`);
    log.trace(`ERR=${error.message}`);
}
```

“看一下`onConnect()`哪个是收到的第一个回调。它在建立会话超时之前将套接字的远程地址保存到类实例中。稍后，任何需要它的方法都可以访问这个远程地址。

当`onStream()`被触发时，它可以访问当前会话的`sessionId`，因为它是类的一个属性，并且在构造函数中被初始化。它还可以访问并递增下一个可用的`streamId`，因为它也是在构造函数中初始化的。

类似地，当`onTimeout()`被触发时，它可以访问服务器`session`，并适当地关闭它。

"如果有什么疯狂的事情发生，`onUncaughtException()`回调将使所有实例属性随时可用于诊断目的."

“这不是什么新花样。它已经存在很久了。”

“我猜是因为缺少闭包，所以可读性很强，”Ivana 评论道。

“没错，”Antoní笑着说，“闭包在 Node.js 社区中不知何故被提升为一种规范模式——但是没有明显的原因。考虑一下:闭包*拓宽了函数*的范围，允许函数访问其包含范围的变量。这与我们都宣称的政策背道而驰——我们应该将变量本地化，而不是全球化。

![](img/0c5bd3a42a46ac1ce5c101fee074f240.png)

“定位范围是`strict`、`var`、`let`、`export`和`import`等事物背后的全部目的。我们希望避免污染外部名称空间，这样本地名称空间就不会发生意外冲突。

当你给一个类添加属性时，你明确地告诉编译器那些属性只能被实例化的对象访问。对于该类的方法，这意味着，例如，类似于`this.remoteAddress`的东西。对于类外的函数，其中`sm`是 SessionMonitor 的一个实例，这意味着`sm.remoteAddress`或`sm[remoteAddress]`。

“回调*本身*并不新鲜。后端的 Node.js 和前端的 DOM 都是事件驱动的，遵循的是已经存在了几十年的架构模式。认为 JavaScript 是单线程的，以及认为*和*在某种程度上使其独一无二的观点是目光短浅的。

“我记得 Windows 刚推出的时候。当时它是一个运行在 DOS 操作系统中的单线程 16 位图形用户界面。事实上，它使用消息，这意味着我们必须找出一种方法来构造我们的代码，以发送和接收这些消息。我刚才展示的模式是基于微软多年前开发的模式。

“DOS！?"Ivana 对此表示怀疑，“那时 JavaScript 还没有出现呢！”

“图案，我说。记住，模式超越语言。所以不管是 C、C++还是 JavaScript 都无关紧要”

伊万娜几乎脸红了。她知道的。稍微转移一下话题，她问道，“为什么我在这么多其他代码库中看到习语`var that = this`

这是 ECMAScript 2015 中添加`bind`之前的遗留问题。当使用闭包时，这是一种保存外部作用域对`this` 的引用的方法，这样内部函数就可以访问它。这是必要的，因为内部函数有自己的`this`指向完全不同的东西。当内部函数需要访问外部作用域上的属性时(让我们再次使用`remoteAddress`，它使用语法`that.remoteAddress`。

“所以如果你使用`bind(this)`，这是一种反模式，”Ivana 总结道。

“没错，”安东尼笑了笑——他已经明白了。

“很好，”她咕哝着，主要是自言自语，想着如何将他教她的东西应用到自己的代码中。

Ivana 不完全确定她能记住所有的东西，所以她给白板拍了一张快照作为提醒。

Antoní回到了 brotli 压缩的工作中。但是他走神了。他已经忘了当一名新工程师时，情况在不断变化是什么感觉。新人应该如何识别好的模式和过时的习语之间的区别呢？如何阻止他们简单地模仿他们在其他人的代码中看到的东西？他们怎么会知道有更干净的方法来做事呢？

![](img/6590b063061023d370a57d1bc5ffd813.png)

*没有* [*迷你图人物*](https://readwritetools.com/meet-the-team.blue) *在制作这一段纠结的网络服务插曲中受到伤害。*