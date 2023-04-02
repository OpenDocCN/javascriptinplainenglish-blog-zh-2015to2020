# 让我们清理一下:JavaScript 中丑陋的尝试陷阱！

> 原文：<https://javascript.plainenglish.io/lets-clean-up-ugly-try-catches-e4e1a53de500?source=collection_archive---------0----------------------->

![](img/e0deb3f6ae898770fd3745501e5a57b9.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们都去过那里。我们都使用了`await`对`async`的方法，忘记了把它们包装在`try-catch`里，只是为了被告知一个`Unhandled Error`😱

但不仅仅是这些`aysnc`方法会抛出。也许我们使用了抛出的第三方库，或者我们设计我们的代码库的方式使得我们有意抛出错误以将错误处理提升几层。

因此，我们继续将我们的调用包装到我们的`try-catch`块中的可抛出方法中。

太好了。😍

🤔还是真的？

在代码库中工作，你可以期望方法`throw`会导致你的逻辑被包裹在`try-catch`块中的情况。它还会导致其他代码设计问题。

看看下面的例子:

如上所述，`myVar`变量不存在于`try`块之外。但是我们需要这种价值来延续我们的逻辑！！

所以现在我们需要做一些不同的事情。
怎么样:

# 🤮!

不。我们不要这样做。

现在我们所有的逻辑都封装在`try`块中。太丑了。
此外，如果任何后续的方法调用抛出，我们确定要以同样的方式处理它们吗！？可能，但最有可能不是！

好吧，让我们试试别的…

这有点好，但仍不完美。它仍然是丑陋的，因为`myVar`必须被声明，然后在不同的范围内几乎立即被初始化。当`myThrowableMethod`真的抛出时也会出现问题，因为执行将继续并尝试使用`myVar`！

🐛警惕！

我可以继续说下去，给出更多这些`try-catch`块会带来代码设计、可读性和可维护性问题的情况。

相反，我会给你一个解决方案！

# 解决方案🚀

我写了一个小图书馆来正面解决这个问题:

# 让我们欢迎[不要尝试](https://github.com/coly010/notry.git)到现场。🎉

# 什么是`no-try`？😱

`no-try`是一个很小的库，它去掉了代码中的`try-catch`，提高了代码的可读性和可维护性，同时也有助于改进代码设计。

它公开了两个功能。`noTry`、`noTryAsync`后者解析并返回承诺结果。

不相信我？让我们更详细地看看它。

要安装它，只需运行`npm i --save no-try`

然后将其添加到您的文件中:

在打字稿中:

`import {noTry, noTryAsync} from "no-try";`

在 JavaScript 中:

```
const noTry = require('no-try').noTry;
const noTryAsync = require('no-try').noTryAsync;
```

现在，让我们重构上面的例子，使用`no-try`。

如果你有一个标准的错误处理函数，你可以把它提供给`noTry`，如果有错误发生，它会为你调用它！

就是这样！

我们已经从代码中移除了`try-catch`块，防止了与块作用域变量相关的问题，同时也让我们的代码更具可读性，而不会牺牲我们想要的处理错误的灵活性。

你可以在 [GitHub](https://github.com/coly010/notry.git) 上阅读更多关于`no-try`的内容。

现在去清理你的代码吧！

如有任何问题，欢迎在下方提问或在 Twitter 上联系我: [@FerryColum](https://twitter.com/FerryColum) 。

*原发布于 2019 年 11 月 30 日*[*https://dev . to*](https://dev.to/coly010/let-s-clean-up-ugly-try-catches-5d4c)*。*