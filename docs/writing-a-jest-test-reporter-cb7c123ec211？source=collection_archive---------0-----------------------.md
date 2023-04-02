# 写一个笑话测试记者

> 原文：<https://javascript.plainenglish.io/writing-a-jest-test-reporter-cb7c123ec211?source=collection_archive---------0----------------------->

最近在工作中，我一直在构建一种方法，通过泽法将我们端到端测试的试运行数据上传到[吉拉。](https://marketplace.atlassian.com/apps/1014681/zephyr-for-jira-test-management?hosting=cloud&tab=overview)

通常集成测试运行结果的方法是将它们导出为 JUnit 兼容的文档，并将它们上传到支持该格式的工具中。

泽法有一个 API，其他团队中的一个已经建立了一个与之交互的库，所以我决定利用它在测试运行结束时上传结果。

像这样使用泽法的好处之一是，我可以获得环境变量，并将其作为测试运行元数据的一部分，以帮助跟踪每个环境中的每个测试运行。

# 什么是测试记者？

![](img/227500d7146d5016f2ab5e6dda6b98ad.png)

Photo by [rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

测试报告程序是测试运行程序的挂钩，它允许在测试运行的开始和结束时执行代码。

有许多测试记者可以开玩笑；处理不同数据格式、与不同系统集成或者只是帮助通知的那些。

要使用一个测试报告器，你首先通过`npm install`安装这个包，然后在你的`jest.config.js`中把它的名字添加到`reports`数组中。

Example jest.config.js to include a customer reporter (in this case it upserts test run data to Couchbase)

# 测试记者可以进入的阶段

![](img/819dcfd28579a68e00acbf4a90ab934e.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

测试记者参与了两个事件:

`onRunStart`这个阶段是在测试运行开始之前，但是测试套件的数量(`describe`块)已经被计算出来，所以如果你愿意，你可以使用这个值为结果创建占位符。

`onTestResult`该阶段在测试套件结束时启动，可用于处理测试套件的结果以及更新测试运行本身的进度。

如果你想在测试运行时可视化测试进度，这将是最好的选择。

`onRunComplete`此阶段随后启动，可用于处理所有测试运行后的测试结果(或在测试运行因错误而中止的情况下)。

如果你想批量上传所有测试的结果，这将是最好的选择。

# 各阶段提供的数据

我没有找到太多关于传递到每个阶段的数据的文档，所以我决定在 https://gitlab.com/colinfwren/containerised-jest 运行测试，并收集每个阶段传递的参数。

该过程的输出将是一个 JavaScript 对象，每个位置参数都由它在函数签名中的索引来表示。为该参数传递的数据是该键的值。

我已经创建了一个[自定义 Jest reporter 来帮助记录作为测试运行生命周期](https://gitlab.com/colinfwren/jest-reporter-debug)的一部分接收到的数据。这包括一个包含数据结构的 JSDoc。

## onRunStart

由于在此阶段没有传递太多数据，因此通常会看到第一个参数被解构以收集`numTotalTestSuites`值，例如:

```
onRunStart({ numTotalTestSuites }) {
    // do some stuff
}
```

The arguments onRunStart is called with

## onTestResult

传递的第一个参数提供了正在执行的测试的上下文，例如测试运行程序的配置和它作为其一部分运行的包。

传递的第二个参数提供了关于测试套件和执行的测试的信息，例如:

*   测试套件中通过了多少测试
*   测试套件中有多少测试失败
*   测试套件中所有测试花费的时间
*   测试套件的文件路径
*   测试运行的快照更改
*   测试的名称
*   测试持续时间
*   测试的祖先(对嵌套的描述块有用)
*   测试失败时的失败消息(用于将堆栈跟踪作为失败测试的注释)

传递的第三个参数提供了在该时间点的完整测试运行的信息，比如到目前为止运行的测试总数、通过/失败的测试总数以及通过/失败的测试套件。该对象还包含作为第二个参数传递的测试套件数据。

Arguments passed to onTestResult

## 未完成时

第一个参数似乎不包含任何信息，但我相信它包含了关于最后一次测试运行的信息。

第二个参数包含测试运行的结果，例如:

*   运行测试套件和测试用例的总数
*   通过/失败的测试套件总数
*   通过/失败测试的总数
*   测试运行的快照信息
*   每个测试套件的测试结果信息，包含测试套件中每个测试的测试结果信息

Arguments passed to onRunComplete

# 更多资源

我发现 Jest repo 上的以下帖子有助于弄清楚测试记者可以使用什么钩子:[https://github.com/facebook/jest/issues/4471](https://github.com/facebook/jest/issues/4471)

你可以在 npm 或者 [Github](https://github.com/search?utf8=%E2%9C%93&q=jest-reporter&type=) 上找到很多不同的 [jest 记者。](https://www.npmjs.com/search?q=jest-reporter)

我还用 jsdoc 为定制报告器创建了一个示例项目，它记录传递给每个钩子的参数，以帮助记录定制报告器可以处理的数据。