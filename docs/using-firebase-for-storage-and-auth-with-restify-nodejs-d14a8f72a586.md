# 通过 Restify & NodeJS 使用 Firebase 进行存储和授权

> 原文：<https://javascript.plainenglish.io/using-firebase-for-storage-and-auth-with-restify-nodejs-d14a8f72a586?source=collection_archive---------5----------------------->

![](img/80b94ce419ad3648db3d2433b23232e5.png)

Photo by [Adam Wilson](https://unsplash.com/@fourcolourblack?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我最近一直在构建一个 API，为我将要构建的 CV 应用程序实现基本的创建、读取、更新、删除(CRUD)功能。

[我首先使用 Pact](https://medium.com/@colinwren/building-an-api-consumer-first-with-pact-88360a19bf0d) 构建 API 消费者，一旦我很高兴我的 API 满足了带有存根的消费者契约，我就开始实现用于存储和授权的[Firebase](https://firebase.google.com/)，使用消费者契约来确保我没有破坏任何东西。

这是我第一次使用 Firebase，因为如果我需要实现授权，我通常会退回到我的舒适区 Django 和 PostgreSQL，但我过去曾使用过 [Restify](http://restify.com/) ,并很喜欢它，所以决定尝试一下 Firebase，因为它有望轻松实现授权和存储。

# 使用 Firebase 添加身份验证

![](img/0f0f77f868978b3f89a87d8c74e4761e.png)

Photo by [Siora Photography](https://unsplash.com/@siora18?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

现成的 Firebase 支持相当多的身份验证后端，我认为您很难找到一个用户没有至少一个提供后端的帐户，在编写本文时，这些帐户包括:

*   电子邮件地址和密码
*   电子邮件地址和一次性链接
*   电话号码
*   脸谱网
*   推特
*   谷歌
*   微软
*   苹果

在下面的例子中，我使用电子邮件地址和密码。

## 创建新用户

在让用户登录之前，您需要让他们注册您的应用程序。

为了做到这一点，您可以使用`firebase.auth().createUserWithEmailAndPassword()`函数来创建帐户或抛出一个错误，然后使用`firebase.auth().currentUser.getIdToken()`函数来为该用户获取一个 auth 令牌。

Wrap a try/catch around this and you’ve got a means of signing the user up and returning their auth token to them

## 登录现有用户

一旦用户第一次登录到您的应用程序，他们就可以使用`firebase.auth().signInWithEmailAndPassword()`函数登录，类似于`createUserWithEmailAndPassword()`函数，但是需要调用`getIdToken()`来获得他们的认证令牌。

Wrap a try/catch around this and you’ve got a means of singing the user in and returning their auth token to them

## 从 JWT 获取用户

一旦用户成功注册或登录，并且获得了他们的 auth 令牌，他们就需要在随后对 API 的调用中使用该令牌来验证他们自己，这通常是通过在 Authorization 头中使用它作为载体令牌来完成的(例如`Authorization: Bearer [TOKEN]`)。

在 Restify 中，您可以将函数作为中间件应用于对服务器的所有调用，并在调用控制器之前执行任务。

我创建了一个函数来做这件事，然后使用`server.use()`函数来应用它，中间件函数采用与普通控制器相同的`(req, res, next)`参数。

Grab the JWT, get the user from it and set that on the request to be accessed by later controllers

为了从 JWT 获取用户记录，您需要调用`admin.auth().verifyIdToken()`，它将返回令牌中包含的信息。然后您可以使用带有`admin.auth().getUser()`的`sub`属性来加载用户记录。

一旦我获得了用户记录，我就使用 Restify 的`req.set()`函数来设置请求的用户对象，然后可以通过后续控制器中的`req.get()`来访问用户记录。

## 明白了—注销不会吊销令牌

[Firebase 文档](https://firebase.google.com/docs/reference/js/firebase.auth.Auth#signout)建议您调用`firebase.auth().signOut()`来结束用户的会话，但这实际上并没有撤销用户的授权令牌，这意味着用户仍然可以在他们使用的令牌创建后的一个小时内访问文档。

相反,`signOut()`告诉 Firebase 在用户令牌到期时不要更新它。我还没有找到在注销时完全撤销访问权的方法，所以在使用 Firebase 进行 auth 时要记住这一点。

# 使用 Firebase 作为存储

![](img/9534ab9c794eb43617544ef03c70d734.png)

Photo by [Danny](https://unsplash.com/@findthevision?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Firebase 使用文档存储结构。与大多数 NoSQL 解决方案类似，它没有绑定到模式，因此可以用于保存文档之间的不同数据。

我还没有机会深入了解 Firebase 的存储方面，但据我目前所见，它相对容易使用，但像大多数 NoSQL 存储一样，您需要提前计划如何横向扩展。

Firebase 使用`collections`作为分离文档类型的一种方式，所以如果你想处理一系列包含狗的数据的文档，你可以使用`admin.firestore().collection('dogs')`，然后对集合执行操作。

在集合中有一系列对文档的引用；这些引用对象是您创建、删除、更新和读取文档的交互方式。

## 设置事物

为了使用 Firebase 存储，您需要使用`admin.firestore()`初始化 Firestore，它会返回一个对象，您可以使用它来执行文档操作。

The db constant will then be used to perform any database operations.

## 创建文档

创建文档非常简单，只需使用`db.collection('dogs').doc()`在集合中创建一个新文档，它将返回对新创建文档的引用。

该文档引用有一个名为`set()`的方法，用于更新存储在该文档中的数据。

The document reference is really important for dealing with the record

## 搜索文档

一旦你创建了一个文档，你很可能想要在以后检索它并与之交互。

如果您仍然将 ref 存储在某个地方，那么这样做是很容易的，但通常情况下，您需要进行搜索来找到它，或者根据某个条件对多个文档进行搜索(比如用户 ID 来获取该用户的所有文档)。

为了搜索一个文档，你需要获得一个存储该文档的集合的引用，这个引用有一个叫做`where()`的方法，可以用来提供一个谓词来进行搜索。

一旦使用由`where()`提供的`get()`函数执行了搜索，结果对象将具有一个`docs`属性，这是一个返回文档的数组。

在下面的例子中，我搜索与用户相关的所有文档，然后向用户返回一个 id 数组。

The predicate can search in nested objects using dot syntax

有一点需要注意，因为 Firebase 是一个 NoSQL 数据库。您需要在您的模式中找到一种方法来存储您想要搜索的属性，例如`userId`，因为默认情况下不会提供这些属性，在我的应用程序中，我不得不扩展我的文档的元对象的模式，以包含`userId`来搜索它。

## 阅读文件

通过文档 ID 直接访问文档以便从中读取数据的管理方式类似于执行搜索，唯一的区别是为集合提供文档 ID 意味着不需要提供谓词。

您可以使用`db.collection('dogs').doc(DOCUMENT_ID)`获得对文档的引用，它返回对集合中文档的引用，类似于您需要调用`get()`来获得对文档本身的引用的搜索。

一旦获得了文档引用，就可以使用引用的`exists`属性来检查文档是否确实存在，并调用`data()`方法来读取文档数据本身。

I like to split the collection reference logic into a separate function to allow for re-use

## 更新文档

对文档的更新是在文档引用上执行的，并作为单个深度对象提供。

任何需要更新的嵌套对象属性都需要以点语法提供(例如，要更新`meta`对象的`userId`属性，您需要提供`meta.userId`作为更新对象中的键)。

当处理需要对嵌套对象使用点语法时，我发现`dotize`库很有用。

一旦获得了正确格式的更新数据，就可以调用文档引用的`update()`方法。

然而，对文档的更新并没有更新现有的文档引用，所以如果您希望将更新后的文档返回给用户，您需要再次获取它并从中读取数据。

Dotize is great for taking nested objects and converting them to dot syntax

## 删除文档

删除一个文档非常简单，只需在文档引用上调用`delete()`即可。

I return true or false as a means of knowing if the document existed but this isn’t necessary

# 测试用熄灭火源

![](img/34f885d40ada333679de28d93d6d1286.png)

Photo by [Nicolas Thomas](https://unsplash.com/@nicolasthomas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

正如本文开头提到的，[我在构建我的 API 实现时一直使用 Pact 来实践 TDD](https://medium.com/@colinwren/building-an-api-consumer-first-with-pact-88360a19bf0d)，由于 Pact 创建的 Pact 的静态特性，我需要在测试中使用存根值。

即使我没有使用 Pact，但是我仍然希望存根我的 Firebase 调用，因为这将允许我测试我的应用程序如何响应各种场景，而不需要有一个运行的 Firebase 来连接和操纵返回我需要验证的状态。

我使用 [Jest 作为我的测试运行程序，因为它有一个非常有用的自动模仿特性](https://jestjs.io/docs/en/manual-mocks)，这意味着将模块放在项目根目录下的`__mock__`目录中会将这些模块作为存根应用于测试代码中导入的实际模块。

这取代了使用像`proxyquire`和`Rewire`这样的库来控制被测代码将与之交互的库的需要。

## 嘲弄火焰基地-管理

`firebase-admin`是提供 JWT 阅读和 firestore 功能的模块，因此大多数 firebase 呼叫都是在这里进行的。

下面的模块需要在`__mocks__/firebase-admin.js`可用才能工作，你可以在任何`jest.fn()`调用中使用`mockImplementation`来实现你想要测试的行为。

This mock covers all the firebase-admin functionality used in this post

## 模拟燃烧基地

`firebase/app`是提供创建新用户帐户、登录和注销以及获取当前用户的功能的模块。T

既然如此，这是一个相对轻量级的模拟。

下面的模块需要在`__mocks__/firebase/app.js`可用才能工作，与上面类似，你可以使用`mockImplementation`来实现你想要测试的行为。

This mock covers all the firebase/app functionality used in this post

# 摘要

Firebase 无疑实现了它的承诺，即易于实现授权和存储，并且使用起来很愉快，尤其是在 Restify 中使用中间件和 Jest 在测试期间的自动模仿功能与模块化方法相结合时。

我还没有到需要担心 Firebase 的定价模式的地步，所以我还没有考虑到这一点，但我会向任何希望快速向项目添加授权和/或存储的开发人员推荐 Firebase。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****