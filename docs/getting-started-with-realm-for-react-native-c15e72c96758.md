# React Native 领域入门

> 原文：<https://javascript.plainenglish.io/getting-started-with-realm-for-react-native-c15e72c96758?source=collection_archive---------0----------------------->

[*Realm Database*](https://realm.io/docs/javascript/latest/)*让你的 React 原生 app 轻松保存和操作数据库中的数据。*

![](img/1be18be80dea2f7c36c0653dbb5efb97.png)

Photo by [Oskar Yildiz](https://unsplash.com/@oskaryil?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/app-development?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

Realm 是一个库，用于保存在重新加载和重新启动时持久保存的数据。以这种方式处理数据的其他方法是使用 AsyncStorage 或 SQLite。

使用 SQLite，您会发现自己在编写查询，而在 Realm 中，您可以像访问 JavaScript 中的对象一样访问数据。

在这篇博客中，我将尝试向您展示如何在您的下一个 React 本机应用程序中安装和使用 Realm 数据库。完成的代码将在 Github 上提供，并由我在我的三星 Galaxy S6 Edge 上进行测试。

*注意:Expo 不能和 Realm 一起使用。*

## 动机

当我第一次使用 Realm 时，我一直在努力让它工作。我看过的所有教程都包含了太多的信息。我希望这个指南更简洁，并鼓励其他人使用 Realm，因为在我看来它是 SQLite 的一个很好的替代品。

# 安装和设置

我将假设您以前已经用 React Native 制作了一个应用程序，并且您已经遵循了他们为 React Native CLI 设置的[开发环境。](https://reactnative.dev/docs/environment-setup)

打开你的 cmd，创建一个新的 React 原生应用。

```
npx react-native init RealmExample
```

进入新文件夹，运行你的应用程序，确保一切正常。我在连接到我电脑的安卓手机上运行它。这一步可能需要几分钟(或 10 分钟)。

```
cd RealmExample && npx react-native run-android
```

如果一切正常，进入 app.js 并删除所有默认元素，直到只剩下这个。

你现在应该在你的手机或模拟器上看到一些文本，没有其他内容。你可以随心所欲地设计应用程序。我会把它保持在最低限度。

## 安装领域

现在我们已经设置了我们的应用程序，是时候安装 Realm 了。

```
npm install realm --save
```

*注意:我过去在安装领域*[](https://docs.mongodb.com/realm/react-native/install#std-label-react-native-install)**时遇到过一些问题。这可能是因为 React Native 或 React 的版本已过期。请尝试更新它们，然后重试。**

# *制作图书模式*

*领域使用模式。为了简单起见，我们将创建一个包含所有领域代码的 Database.js 文件，从中我们可以导出函数以在组件和页面中使用。*

*让我们创建 Database.js 文件，并为一本书添加模式。*

*我们已经为 book 对象创建了一个模式。一本书包含所需的属性 title 和 pages，它们分别是 string 和 int 类型。我们还为图书版本添加了一个可选的(可以为空)属性。如果它不是 null，它必须是一个 int。*

*我们还用我们的模式创建我们的 Realm 对象，并导出它。请注意 schemaVersion 是如何设置为 1 的，稍后当我们添加一个用于向一本书添加作者的属性时，这将会改变。*

## *添加数据*

*现在我们要添加一些测试数据。我创建了一个小的 for 循环来创建三本书。*

*这个 for 循环被粘贴到 return 语句之前的 app.js 中。我让它跑了一次。之后我就把它注释掉了。如果它运行得更频繁，这没什么大不了的。无论如何，我们最终都会删除测试数据。*

## *查看数据*

*在数据库中拥有数据很有趣，但是我们如何才能看到它呢？有一种方法可以使用 Realm 的 Realm studio 并浏览 Android 中的文件，但如果你使用的是物理设备，那么让它工作起来就很麻烦了。*

*让我们添加一个函数来检索所有书籍并将其导出，以便我们可以在 app.js 中访问它。*

*将这几行添加到 Database.js 中。我们使用 realm.objects('Book ')检索书籍。这将返回一个我们可以查看的 JSON 对象。您可以添加 console.log()来查看控制台中的对象。*

*通过编辑导入和文本元素，您还可以看到它是如何工作的。像这样。*

*如果一切都是正确的，您将看到一个 JSON 字符串，其中包含一组书籍，有一个标题，400 页，edition 的值为空。*

*可以想象，您现在可以随心所欲地处理这些数据。您可以在书籍数组中循环显示一个漂亮的列表。让我们开始吧。*

## *制作一个可视列表*

*我们将使用 React Native 中的 [FlatList](https://reactnative.dev/docs/flatlist) 组件来正确显示我们的图书。*

*移除 app.js 中的<text>元素，用这个平面列表替换它。</text>*

**确保为制作此平面图导入适当的东西。**

*这段代码简单地从我们从 *getAllBooks* 函数获得的数据中创建了一个列表。它创建了一个包含三个文本的视图，每个文本包含图书数据的一个值。*

**如果您没有看到任何数据，请确保您的代码是正确的，并通过执行我们之前看到的代码再次尝试添加数据。**

## *添加和删除数据*

*现在我们可以看到数据，我们希望能够通过单击按钮来添加或删除它。*

*让我们在 Database.js 中添加两个函数来添加新记录和删除所有数据。*

*阅读这些函数，并试着理解它是如何工作的。*

*我们调用 **realm.write** 来准备 realm 改变一些东西。在 realm.write 中，我们根据我们的模式创建了一本新书。我们在函数中添加了一些参数，这样我们就可以添加包含我们想要的数据的书籍。*

*删除所有书籍的方法是首先获取所有书籍，然后删除数据。这个*可能*以后会更有意义。但是基本上您可以在一次写操作中修改数据库中的数据。但是你可以在后面读到更多。*

*让我们在平面列表上添加两个按钮！我将简单地添加两个带有 onPress 属性的文本组件和一些非常简单的样式。*

## *添加按钮*

*我添加了两个按钮，onPresses 和使用 useState 钩子添加状态。*

*如果到目前为止所有代码都工作正常，那么恭喜你。到目前为止，代码可以通过单击两个按钮来添加数据和删除所有记录。*

**嘶。如果你有困难，* [*这里是 app.js*](https://gist.github.com/mbvissers/915695269384085d98327c1b521c701c) *到目前为止，* [*这里是 Database.js*](https://gist.github.com/mbvissers/577c5200454b85f4d27b3d8aefc80520) *到目前为止。**

*显而易见，下一步是添加用于添加数据的输入字段。但是我说不。我很确定你知道如何改变参数和增加输入。*

*提示:如果你不理解大部分代码，花点时间通读一遍，试着理解它。理解它比复制粘贴代码更重要。*

## *更新数据*

*现在我们只需要看看如何更新数据。之后，我们可以在 book 模式中添加 author 属性。*

*让我们添加另一个按钮，将版本为 null 的所有记录更新为版本 1。*

*第一步是在我们的 Database.js 中创建一个函数，该函数将所有的空版本值更新为 1。*

*你可以看到，在 realm.write 函数中，我们得到所有的书，我们使用 [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 函数对它们进行循环，如果版本等于 null，它就将它设置为 1。*

*添加这个方法，在你的 app.js 里面像这样添加一个新的按钮。*

*如果所有版本为空，按钮会将它们设置为 1，然后重新加载我们的状态，这样我们的页面就会更新。就像其他按钮一样。*

*当您按下“新建”按钮时，记录应该全部更新为新的版本值。*

## *选择记录*

*我们拥有的当然是美好的。但是如果我们只想选择特定的书呢？让我们通过选择所有超过 400 页的书来尝试一下。*

*这是我们的新功能。你可以把它换出来，把这个函数放在初始状态加载，看看它的效果。不过要确保你有一本书符合你的条件。*

*你可以在这里阅读更多关于查询数据[的内容。在王国的官方文件中。但是我们稍后会讨论更多的问题。](https://realm.io/docs/javascript/latest/#queries)*

*[*这里是 app.js*](https://gist.github.com/mbvissers/9b9b2aa271fb12c078f1107b6fc55e8c) *到目前为止，这里的* [*是 Database.js*](https://gist.github.com/mbvissers/274ba8211b253c8e5d8abfc347ed1d6d) *到目前为止。**

*到目前为止，我们已经做了很多。去伸展一下。喝点水，休息一下。之后，让我们继续。*

# *更新模式*

*一本书通常有一个作者。但是我们目前不支持。让我们解决这个问题。*

*我们希望为我们的作者拥有一个单独的“表”，而一个新的“表”需要一个新的模式，所以让我们添加一个新的模式。我们还需要更新 Bookschema 来添加作者，并更新我们的 Realm 对象。*

*我们的 Database.js 现在包含了这段代码。这是一些更新的代码，和一些新的代码。仔细看。我们还更新了 schemaVersion。*

*我们还将作者值设置为 BookSchema 中的一个可选字段，这样我们就不会出现任何错误，因为我们现有的书籍没有任何作者。您还可以添加一个默认作者或删除所有现有书籍，这样模式差异就不是问题了。*

*我还增加了一个新栏，在我们的应用程序中显示我们书籍的作者姓名。该列被添加到平面列表中。*

*如果作者为空，则只显示“空”，否则将显示作者的全名。*

*为了利用我们新的作者字段，我们需要更新 *addBook* 来包含一个作者。我还创建了一个函数来获取所有作者。*

*现在，您可以通过调用更新的 addBook 函数并使用它添加一个 author 对象来添加一本书的作者。*

*新作者会自动按领域添加到作者“表”中。如果您添加另一个包含所有作者的平面列表，就可以看到这一点。*

*这里我们为作者添加了一个新的状态和一个新的列表。如果你添加了一本有作者的书，你会看到它会出现在第二个列表中。*

*如果你删除所有书籍，作者将保持可用。*

*您可能已经注意到，这将为每本书添加一个新的 author 对象，即使是同一作者。让我们解决这个问题。*

*我们需要函数来创建一个作者，一个函数来获取一个作者。您很可能希望通过 id 来检索它们。因此，让我们更新代码，为作者添加一个**主键** id。*

**如果您因为现有作者中没有 ID 而遇到问题，您现在可以删除所有数据或省略“primaryKey: id”。**

*添加一些独特的作者一起工作。我将通过使用一些随机参数和 id 的 0、1 和 2 调用 addAuthor 函数来创建三个。*

*也删除所有书籍。*

*现在让我们添加一些作者来自我们的作者表的书籍。*

*我们首先需要的是一个按 ID 获取作者的函数。*

*我们使用 JavaScript [模板文字](https://developer.mozilla.org/nl/docs/Web/JavaScript/Reference/Template_literals)将参数添加到字符串中。简单地将数字附加到字符串上也是可行的。*

*将该函数添加到您的导出中，并更新 addBook 中的 onPress 按钮，以添加带有作者 ID 的图书。*

*加几本书看看。不会有新作者了。*

*您可能还(没有)注意到，在 addBook 函数中，我们获取 getAuthorById 返回的数组的第一个元素。因为我们知道它应该只返回 1 个对象，所以我们可以非常安全地用[0]获得第一个元素。*

## *更新单个记录*

*更新单个记录与我们之前更新所有记录一样。*

*在 realm.write 中，您获得一条记录，并像普通的 JavaScript 对象一样对其进行更改。*

*恭喜你走到这一步。本教程到此结束。*

**嘶。* [*这里是要点*](https://gist.github.com/mbvissers/cba1a37a933d96d8ca873abc16cdaff5) *找到我们到目前为止所做的代码。**

*我希望它是有用的，并且您已经了解了开始学习所需的一切。*

*请继续阅读，找出你自己尝试的内容，并查看我使用所有这些代码完成的最终 GitHub 项目。和一些让你生活更轻松的小技巧。*

# *下一步是什么？*

*你可以更新这个项目，并尝试添加按钮，文本字段，也许新的模式，以在书中包括 id，添加一些章节，为流派添加一个新的模式，等等。玩得开心，祝你好运。*

# *技巧*

*A.自动增加 id 的一个简单方法是检索最大的 ID，并在 addAuthor 函数中给它加 1。*

*B.另一个技巧是，无论何时使用 Realm，都要尝试创建一个测试页面。以便您可以快速查看和添加数据。*

*C.阅读[文档](https://realm.io/docs/javascript/latest/)。有几种方法可以声明类型，类型比我之前介绍的多得多，要执行的查询也多得多。*

*D.“添加日期”列的日期对象很容易创建。将模式中的类型设置为“date”，并在 create 函数中将其设置为“new Date()”。*

# *GitHub 项目*

*为了可用性，我稍微更新了我们在这里做的项目。只是多了几个按钮和稍微更新的样式。你可以在这里找到[。](https://github.com/mbvissers/RealmExampleApp)*

*祝你学习顺利，玩得开心。感谢您的阅读。*