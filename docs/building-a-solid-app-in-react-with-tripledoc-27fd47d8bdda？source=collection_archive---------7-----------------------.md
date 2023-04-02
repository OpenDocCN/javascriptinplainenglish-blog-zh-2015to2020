# 使用 Tripledoc 构建可靠的应用程序

> 原文：<https://javascript.plainenglish.io/building-a-solid-app-in-react-with-tripledoc-27fd47d8bdda?source=collection_archive---------7----------------------->

![](img/03903c365aa4a31ae27ed6e305b52d25.png)

Photo by [Andrej Lišakov](https://unsplash.com/@lishakov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在互联网上，你的数据很可能是你最有价值的资产。有了它，公司可以推断出驱动你的所有方面，并有针对性地进行广告宣传，把你变成他们产品的消费者，或者改变你对他们关心的主题的看法。

但是你并不拥有这些数据。它存在于您使用的各种服务、网站和应用程序中的大量孤岛中，虽然您可以通过 GDPR(或非欧盟等同机构)的请求访问这些数据，但您不会获得公司掌握的所有数据，只是属于公司法律义务范围内的数据。

有许多项目和服务可以用来夺回用户对其数据的一些控制权，并建立一个去中心化的服务、网站和应用程序版本，为用户提供他们喜欢的相同体验，同时更容易控制他们的数据去向和他们看到的信息。

乳齿象可能是这些分散服务中最著名的。任何人都可以加入一个乳齿象实例(或者运行他们自己的实例),并从“Fe diversity”发布和接收帖子，这是来自服务器维护者为他们的社区管理的其他乳齿象实例的联合更新流。

支持乳齿象的技术被称为 [ActivityPub，这是一个 W3C 标准](https://activitypub.rocks/)，用于在社交网络上联合内容。它在这个目的上确实很好，但是如果你想要一个更个人化的东西的分散版本，比如你的应用程序使用的数据呢？这是 Solid 正在寻求解决的问题。

# 固体的快速介绍

[Solid 由蒂姆·伯纳斯·李](https://solidproject.org/)(你可能知道他是发明万维网的人)领导，旨在分散用户数据的存储。

这种去中心化不仅仅局限于数据存储的位置，还让用户可以允许多个应用程序访问相同的数据，让用户在如何存储和使用数据方面有更高的自由度。

用户的数据存储在 [*pod*](https://solidproject.org/faqs#pod) 中，通过 [*WebID* ，](https://solidproject.org/faqs#what-is-a-webid)访问该 pod，本质上是一个保存用户信息及其共享偏好的文档。

存储用户数据的 pod 和 WebID 可以是独立的服务(为了更多的去中心化收益)，但是如果你第一次使用 Solid，很可能你会为两者使用同一个提供商[。](https://solidproject.org/use-solid/#how-to-pick-a-provider)

当用户使用 Solid 应用程序时，他们使用自己的 WebID 登录，并授予应用程序访问用户数据的权限(不同的访问权限可用)，如果应用程序能够共享该用户的数据。

该应用程序一旦被授权访问用户 pod 中的数据，就可以使用 RESTful 接口读取、写入和删除 pod 中的数据(取决于授予的访问级别)。

## Solid 如何存储数据

pod 中的数据存储为关联数据(Solid 是社交关联数据的组合)，诚然，我对关联数据和 RDF 了解有限，但这是一种很容易掌握的格式。

Solid 中应用程序使用的资源存储在 Turtle 文档中，这些文档有一个或多个主题。

文档中的主题有定义主题的属性(在 Turtle 规范中称为文字)。这些属性可以是简单的属性，如名称，也可以是相同或多个文档中不同主题之间的关系。

定义两个主题的格式是一个`subject, predicate, object`三元组，例如，如果主题被定义为`:colin a foaf:Person`，这意味着主题是一个由朋友的朋友模式定义的人。

主题的属性是以`property value`的格式定义的，例如`foaf:name Colin`，起初可能会觉得有点奇怪，但是有一些特定的属性类别使得基于图形的计算更容易，例如那些使用`foaf:knows`属性生成主题认识的人的列表的属性。

# 使用 Solid 构建应用程序

当我第一次发现 Solid 时，我真的很高兴看到有许多应用程序发布了它们的源代码，这为理解认证流程以及如何使用 Solid 创建、读取、更新和删除(CRUD)资源提供了一个很好的起点。

甚至有一组 React 组件和挂钩可以让像我这样的人更容易开发，他们知道 React，但对链接数据几乎一无所知。

然而，我发现`[@solid/react](https://www.npmjs.com/package/@solid/react)` [库](https://www.npmjs.com/package/@solid/react)的实现有点令人困惑，因为它使用了 JavaScript 代理和 [LDFlex 表达式](https://github.com/solid/query-ldflex)(我努力使用自定义对象的一些东西)，这感觉更适合于只是读取文档，而不是读取和写入文档及其主题。

我最终找到了 [Tripledoc](https://vincenttunru.gitlab.io/tripledoc/) ，我发现它使用起来非常简单，因为它遵循了 document - > subject - >属性结构，该结构允许在层次结构的每一层进行 CRUD 操作。

我已经使用 Solid 创建了一个演示项目，您可以使用它来更好地理解 Solid 和 Tripledoc，我将在下面进行更详细的介绍。

[](https://gitlab.com/colinfwren/solid-doggo-tracker) [## 科林·雷恩/固体狗跟踪器

### 基于 React 的基本固体应用程序跟踪你的狗狗

gitlab.com](https://gitlab.com/colinfwren/solid-doggo-tracker) 

## 证明

Solid 应用程序的身份验证流程是要求用户使用其 WebID 登录，方法是输入 WebID 提供商的主机，并将他们重定向到该提供商进行登录，如果用户是第一次使用该应用程序，则要求他们配置应用程序的访问权限；之后，他们将作为经过身份验证的用户被带回应用程序。

该认证流程通常在 web 浏览器的弹出窗口中执行。Solid 提供了一个优秀的库，用于以这种方式认证用户，称为`[solid-auth-client](https://www.npmjs.com/package/solid-auth-client)` [](https://www.npmjs.com/package/solid-auth-client)，它有一个方便的 CLI 工具，用于生成一个 HTML 文件来显示用户这样做。

Once the user has logged in the WebID for the session can be used

一旦用户通过身份验证，您就可以通过登录时存储的 cookie 来访问他们的 pod 资源。

## 使用文档

使用`tripledoc`读取文件由`fetchDocument`功能处理。

这个函数获取一个资源的路径(例如 https://host.tld/public/file ),如果找到了就返回文档。

如果没有找到文档，它将抛出一个错误，所以我使用`tripledoc`的`createDocument`函数返回一个相同路径的空文档，如果没有文档存在的话。

Grab the document from the user’s pod and create a new document if unable to find and existing one

没有办法删除`tripledoc`库中的文档，但是您可以向资源 URI 发送一个 HTTP DELETE 请求来删除资源。

## 使用文档的主题

一旦你有了一个文档，你就可以用许多不同的方法让一个主题工作。

如果您想获得文档中某个特定对象类的所有主题，您可以使用 document 的`getAllSubjectsOfType`方法，该方法将返回一个您可以迭代的主题列表。

Getting a list of subjects from a Solid document

如果您知道主题的标识符，您可以使用文档的`getSubject`方法，该方法接受包含#前缀的标识符。

Getting a subject via it’s identifier

如果您想使用一个基于图的谓词来查找主题(比如查找知道给定主题的主题)，您可以分别使用`findSubject`和`findSubjects`方法来实现。

您可以使用文档的`addSubject`方法创建一个主题，该方法采用一个 options 对象，要设置的最重要的属性是`identifier`属性，因为这将决定新主题存储在哪个标识符下。如果没有传递标识符，主题将使用随机 UUID 作为标识符。

Adding a new subject with data

与文档不同，您可以使用文档的`removeSubject`函数使用`tripledoc`从文档中删除主题，该函数将主题标识符(带#前缀)作为参数。

Removing a subject from a document

为了将任何基于主题的更改推送到用户的 pod，您需要调用文档的`save`方法。

## 使用主题的属性

为了访问主题的属性，您需要知道属性将返回什么数据类型以及属性存储在哪个谓词下。

为了处理一个主题的属性，将谓词视为一个键是很有用的，您将使用它来读取、写入和删除属性值。

subject 对象支持以下数据类型:

*   日期时间
*   小数
*   整数
*   本地化
*   裁判员
*   线

以及每种数据类型的以下操作

*   **add** —为所提供的谓词创建该数据类型的新实例
*   **get** —在提供的谓词下获取该数据类型的实例
*   **移除** —移除所提供谓词下该数据类型的实例
*   **set** —删除所提供谓词下该数据类型的现有实例，并添加一个新实例

与主题一样，对这些属性的更改不会被推送到用户的 pod，直到文档上的`save`方法被调用。

An example of removing and adding properties to a subject

## 在 React 应用程序中使用 Tripledoc

这只是我如何在我的 React 应用程序中使用`tripledoc`的一个例子，你可能会发现`[tripledoc-react](https://www.npmjs.com/package/tripledoc-react)`库更容易使用，但我喜欢将我的逻辑功能与组件分开，所以我选择只使用`tripledoc`。

在我构建的演示应用程序中，我有两个元素来跟踪状态——用户的 WebID 和我的应用程序使用的资源中的主题。

我通过一个 React 上下文提供程序来处理这些，因为这允许我访问状态，并从应用程序的组件中调度操作来更新状态。

为了处理认证，我使用了`solid-auth-client`的`popupLogin`函数来加载库提供给我的`popup.html`，通过使用它的 CLI 命令。在用户登录和用户的 WebID 被检索时，它被存储在应用程序的状态中。

用户登录后，将从资源中获取主题列表，并读取用于在应用程序中呈现项目的属性，该对象列表也存储在应用程序的状态中。

为了在应用程序中创建、编辑或删除项目，我从 solid pod 中抓取文档(以确保我正在使用最新的数据)，并在保存该文档之前对主题或主题的属性执行相应的操作。

在保存文档时，我会发送并更新应用程序状态中存储的主题。

多亏了 Tripledoc，在 React 应用程序中使用 Solid pod 与使用任何其他 REST api 没有什么不同。

开发流程的唯一区别是:

*   你可能想注册一个基于云的 pod 提供商的 pod，而不是在本地运行你自己的 pod，你需要跳过一堆网络和安全环来让本地应用程序与本地 pod 一起工作
*   你需要提前了解数据的数据类型和本体，因为在基于 JSON 的商店上构建应用程序使用的数据需要付出更多的努力

# 摘要

我认为，去中心化将是未来 10 年网络和应用开发的一个日益增长的主题，因为立法最终会赶上技术，我们会看到像脸书这样的公司被迫将数据存储在用户的领地。

像 Solid 这样的技术意味着应用程序开发人员不需要担心存储数据和适用于它的数据隐私法律，相反，他们只需要担心处理信息的法律。

我对 Solid 为用户提供的数据可移植性非常感兴趣，因为我一直认为开放 API 供开发者使用可以让生态系统围绕产品涌现(这是我认为 Pokemon Go 首次推出时真正错过的事情之一)。

然而，我确实担心使用相同资源的多个应用程序将如何工作，因为我可以想象应用程序之间存在不兼容性，并且对应用程序开发人员来说，让其他应用程序无法使用资源并不太难，或者更糟糕的情况是，甚至删除其他应用程序的数据。

我还认为 Solid 提供的访问控制可能有点过于宽松，因为你实际上是在授权一个应用程序访问 pod 上的所有文档。可以提出一个论点，人们将使用不同的 pod 来分离私人和公共信息，但这对用户来说是不方便的(因此不太可能发生)，并且这最终会被坏人利用。

如果作为一名应用程序开发人员，我可以从其他应用程序以及我自己的应用程序中访问你的所有数据，我可能会开发一个应用程序，提供一些琐碎的功能，比如让你能够保存食谱，同时还可以在后台读取你的健康信息、社交互动以及其他任何可以让我建立良好的个人资料的东西，我可以将这些资料卖给广告商甚至更糟。

我们已经在应用商店中看到了这种类型的行为,[开发者将构建一个“火炬”应用](https://www.theguardian.com/technology/2013/dec/06/android-app-50m-downloads-sent-data-advertisers),该应用需要开发者可以请求访问的每一个权限，然后收集数据并发送出去出售。

如果 Solid 能够为其存储的文档提供这种细粒度的访问控制，并在应用程序的基础上授予这些权限，这将使我更加确信使用 Solid 时我的数据是安全的。

我认为，虽然在技术上可以托管自己的固体服务器，但我们很可能会看到一两个主要的固体提供商，最终将使这些服务器成为黑客的目标，他们不仅可以访问每个用户的一组数据，还可以访问多组数据，这使其更值得努力。

抛开安全担忧，我真的认为 Solid 是一项伟大的技术，我真的很期待看到它在未来十年如何发展和被采用，以及它如何塑造互联网。