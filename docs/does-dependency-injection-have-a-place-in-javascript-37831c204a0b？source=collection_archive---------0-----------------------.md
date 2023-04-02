# 依赖注入在 JavaScript 中有一席之地吗？

> 原文：<https://javascript.plainenglish.io/does-dependency-injection-have-a-place-in-javascript-37831c204a0b?source=collection_archive---------0----------------------->

## 如果是这样的话，在哪里使用效果最好？

![](img/09e4ac4deaeba0fd9d173e69655eadeb.png)

Photo by [Edvard Alexander Rølvaag](https://unsplash.com/@edvardr?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/hierarchy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

几年前，当测试我的应用程序意味着在浏览器中“点击一下”时，我记得曾和一些更有学问的同行讨论过依赖注入。一个叫约翰的家伙在火车上试图向我解释这个概念，但没有代码库来着重指出。这听起来非常聪明，但没有被理解。我决定做一些研究，因为我应该是我的小公司的首席开发人员，我觉得我应该知道这些东西。它对我来说仍然没有真正的意义，因为它是一个我没有的问题的解决方案。我没有编写单元测试(或任何类型的测试)，因此不需要模仿依赖关系，因此没有对接口进行编程，即使是在 C#中，接口实际上是存在的(不像 JavaScript)。

# 什么是依赖注入？

让我回顾一下，从广义上解释什么是依赖注入。如果我解释错了，请尽管无情地嘲笑我。

依赖注入(有时称为 DI、控制反转或 IoC)可以概括为将配置与实现分离的实践。这可能意味着文字配置，例如您想要访问的端点的 URL——与其将 URL 硬编码到与端点对话的类中，不如通过将 URL 传递给构造函数来“注入”该 URL。

或者你可能依赖于另一个类，比如有一个通过一个`Logger`类写日志消息的`ProductService`类。在这样的例子中，`Logger`可以通过传递给`ProductService`的构造函数作为依赖项注入。您可能希望以这种方式注入依赖项有很多原因:

*   `Logger`是可重用的，可以作为一个依赖注入到`CustomerService`中。这样做避免了存在于`Logger`中的重复代码。
*   为了只测试`ProductService`中的业务逻辑，我们现在可以很容易地在我们的单元测试中提供一个`Logger`的模拟实现。
*   如果我们就`Logger`的接口达成一致，那么当我们继续编写和测试`ProductService`时，其他人可以很容易地编写实现。以这种方式坚持单一责任允许我们更有效地并行工作。

其中一些点更与单一责任原则有关，而不是依赖注入。你可以让`Logger`在`ProductService`中实例化，而不是注入其中。然而，这将使`ProductService`更难测试。

我经常听到的依赖注入的理由之一是，它允许您编写接口，然后更容易地更改您的实现。这里的想法是，如果你有一个用于`Logger`的接口，那么你是将消息写入文件、数据库还是控制台都没有关系。重要的是`**Logger**` 定义了一个契约，如果你改变底层实现，它也不会改变。这对于像`Logger`这样的通用库很有用，在这里你可能想为一个应用程序写一个文件，但是为另一个应用程序写一个数据库。如果其他人提供了日志库，您可以轻松地切换实现。但是，除了为其他开发人员提供库之外，我很少在同一个项目中使用同一个接口的两个实现。当我在一个正在编写的应用程序中从使用 SQL Server 改为使用 MongoDB 时，我只是简单地改变了我的`DBService`的实现。

然而，仅仅是测试，就足以成为我想要某种依赖注入的理由。在像 C#或 Java 这样的语言中，编写接口一直是提供模拟的必要方式，尽管现在反射减轻了这种需要。尽管如此，编写一个接口通常被认为是一个好的实践，因为你可以很容易地通过更新你的配置来提供一个不同的实现。如果这种情况只发生在测试中，那就这样吧——对我来说已经足够好了。

# Java Script 语言

回到手头的事情。如果你相信我的话(我希望你相信)，那么我们已经确定依赖注入在严格语言中是有价值的，严格语言不允许你想什么时候改变类就什么时候改变。但是 JavaScript 在这方面有点快且松散——您可以通过覆盖其原型来轻松地更改任何类的实现。这段代码演示了重写现有类或实例的简便性:

```
class Logger {
  log(message) {
    console.log(message);
  }
}

const fakeLog = (message) => {
  console.log(`MOCK: ${message}`);
}

const x = new Logger();
x.log('Hello.');
x.log = fakeLog; // Change the behaviour of only this instance of the class
x.log('Hello again!'); // Should display "MOCK" prefix

// A new instance of the class still behaves as normal
const y = new Logger();
y.log('This is a new logger.');

// Change the implementation for every instance of this class
Logger.prototype.log = fakeLog; 
x.log('Goodbye.');
y.log('Farewell');
```

如果您在浏览器的 JavaScript 控制台中运行此代码，您应该会看到以下输出:

> 你好。
> 嘲弄:又见面了！这是一个新的记录器。
> 嘲弄:再见。
> 嘲弄:告别

覆盖一个类的原型的能力是一个强大的特性，这意味着你实际上不需要依赖注入来模仿你的依赖。您可以在运行测试之前简单地改变行为，如下例所示:

```
module.exports.Logger = class Logger {
    log(message) {
        console.log(message);
    }
}const Logger = require('./logger').Logger;

module.exports.ProductService = class ProductService {
    constructor() {
        this.logger = new Logger();
    }
    delete(id) {
        this.logger.log(`Deleting product ${id}`);
    }
}
```

如果您运行“node test.js ”,您会注意到使用了 Logger.log 的模拟实现，即使我们没有注入任何东西。这是因为我们已经更改了 Logger 类的实现，而不仅仅是 ProductService 中使用的实例。

从这个角度来看，在 JavaScript 中没有必要进行依赖注入。已经确定了依赖注入的最有价值的结果是测试(通过扩展，模仿)，我们现在展示了一种不注入依赖就模仿依赖的方法。

不过，这里有点味道。我们只能在类级别覆盖实现，这意味着我们的测试将不再被封装。如果我们一个接一个地运行我们的测试，这并不是太大的问题，因为我们可以在需要的地方改变行为，但是这给开发人员带来了在他们完成时将实现改变回来的负担——忽略这样做可能会导致后续测试中的意外行为(特别是对您刚刚嘲笑的类的测试)。

此外，您可能会决定并行运行您的测试，以便加速执行。如果两个测试同时覆盖同一个依赖项，那么奇怪的事情就会开始发生。如果您改为通过构造函数提供依赖，那么您可以模拟类的实例，而不是原型。更好的是，甚至不接触原始类，而是提供看起来像它的东西。例如，就`ProductService`而言，下面的对象看起来像我们之前示例中的`Logger`:

```
const mockLogger = {
  log: message => {
    console.log(`MOCK: ${message}`);
  }
};
```

*您可能想知道为什么要像这样覆盖实现，但实际上，您会使用像* [*Sinon*](https://sinonjs.org/) *这样的库来提供间谍，而不是您的真实实现，从而允许您断言您的 mock 上的特定方法是从您的测试主题中调用的。*

这种方法的好处是，您不会因为忘记模仿它的行为而意外地留下任何真正的实现。如果您的代码调用了一个您忘记模仿的方法，而不是执行真正的实现，您的测试将很快失败，这可能包括 API 调用或您不想在测试中出现的类似行为。

# **框架**

如果你必须注入依赖项，那么你基本上不能实例化任何你想要模仿的依赖项——它们必须从某个地方传入。某处通常是你的应用程序的入口点或主要方法。然而，手动创建依赖项可能很乏味，因为您可能最终会创建离您的入口点有几个步骤的类的实例。

比如你的入口方法是 A，它对 B 有依赖关系，对 C 有依赖关系，对 D 有依赖关系等等，你要在调用 A 的时候实例化 D，这样你就可以实例化 C，从而实例化 B，困惑？你不是唯一一个。这就是为什么有这么多依赖注入框架的原因。这背后的想法是在一个地方配置你所有的依赖项，然后当你需要一个类的实例时，你只需要让框架创建一个。它为您解决了依赖性。通常你只需要做一次，解决你的顶层依赖，这将会渗透到所有的子依赖中，这样你就不用担心了。这里的好处是，如果 D 后来开发了对 E 的依赖，您不必在每个实例化 D 的地方添加 E——您只需在一个地方配置它。

在 JavaScript 中，您有几个选项。如果你正在使用 [Angular](https://angular.io/) 框架，好消息是这些都内置在 Angular 的[模块系统](https://angular.io/api/core/NgModule)中。当你的应用程序启动时，你的根模块连接你在整个应用程序中需要的依赖关系，并允许你将一些依赖关系解析推迟到[功能模块](https://angular.io/guide/feature-modules)。在您的测试中，您用[测试床](https://angular.io/api/core/testing/TestBed)做同样的事情(它只是一个规模小得多的模块)，只提供您需要在这个文件/描述块中运行测试的依赖项。这是我喜欢 Angular 框架的原因之一，也是为什么我发现 React 中的测试令人沮丧(尽管我喜欢 React 的其他方面)。Jest 通过允许模拟导入解决了一些问题，但是它有一个主要的缺点，那就是每个 spec 文件只能提供一个实现。这意味着如果你有三个测试，每个都需要模拟不同的行为，你需要有三个规格文件，而不是只有一个。

如果你没有使用 Angular，有一个很棒的框架叫做 [Inversify](https://github.com/inversify/InversifyJS) ，它提供了一种不同的方式来配置你的依赖项，但是控制级别相同。当我写节点应用程序或脚本时，我倾向于使用 Inversify。

我不会深入这些框架的细节——这篇文章更多的是关于你是否需要依赖注入，而不是如何使用一个特定的库。但是用于 DI 的框架是一件好事，因为它们减少了您必须编写的样板文件的数量，并且使您的代码更容易维护。此外，可能还有我没有提到的其他选项，所以请在评论区大声说出你最喜欢的选项。

# 结论

JavaScript 中需要依赖注入吗？不。你应该考虑吗？是的。它在 JavaScript 中有一席之地吗？当然可以。

一如既往，这很大程度上只是我的观点。你可能不同意它的部分或全部，如果是这样，让我们来谈谈。