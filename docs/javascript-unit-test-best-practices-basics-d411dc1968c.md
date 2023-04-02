# JavaScript 单元测试最佳实践—基础

> 原文：<https://javascript.plainenglish.io/javascript-unit-test-best-practices-basics-d411dc1968c?source=collection_archive---------4----------------------->

![](img/3d348afba10c2ab23f1e4e9b573914ae.png)

Photo by [maggie bell](https://unsplash.com/@mags666?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 单元测试

每个单元测试测试一个工作单元。

它可能涉及多个返回值或抛出异常的方法或类。

他们还可以测试系统中的状态变化。

我们还可以测试第三方 API 调用。

每个测试都应该相互隔离。

任何行为都应该只有一个测试。

而且他们也影响不了别人。

# 单元测试是轻量级测试

轻量级测试是可重复的、快速的、一致的，并且易于编写和阅读。

单元测试也编码。

它们应该满足与被测试代码相同的质量。

可以对它们进行重构，使它们更易于维护和阅读。

# 设计原则

我们的代码应该是可测试的。

我们应该避免好的命名约定，并评论为什么我们有代码。

评论不能代替糟糕的设计。

此外，我们应该避免任何重复。

每段代码都应该有一个单独的职责，这样单元测试就可以测试它。

我们的代码也应该在同一个组件中有一个单一的抽象层次。

我们不应该在同一方法中将业务逻辑与其他技术细节混在一起。

此外，我们的代码应该是可配置的，以便我们可以在测试时在任何环境中运行我们的代码。

此外，我们应该采用依赖注入这样的模式，将对象创建与业务逻辑分开。

全局可变状态也很难测试和调试，所以我们应该避免它们。

# 使用 TDD

测试驱动开发是交互测试软件组件的好方法。

这样，我们可以在每次测试中检查每个部分的行为。

工作流程是我们从编写一个失败的测试开始。

然后，我们用新的代码变更使测试通过。

最后，我们清理代码，确保它仍然适用于我们的测试。

测试第一周期使代码设计可测试。

小的变化是可维护的。

而且代码库可以很容易地用重构来增强，因为我们可以用测试来测试它。

频繁地、小幅度地改变颂歌也更便宜。

测试产生了增加功能、修复错误和探索新设计的信心。

# 适当地构造测试

我们应该适当地组织我们的测试。

我们可以嵌套测试子集。

而不是写:

```
describe('A set of functionalities', () => {
  it('should do something nice', () => {
  }); it('some feature should do something great', () => {
  }); it('some feature should do something good', () => {
  }); it('another subset of features should also do something ', () => {
  });
});
```

我们可以写:

```
describe('A set of functionalities', () => {
  it('should do something nice', () => {
  }); describe('some feature', () => {
    it('should do something great', () => {
    }); it('should do something awesome', () => {
    });
  }); describe('another subset of features', () => {
    it('should also do something great', () => {
    });
  });
});
```

然而，不要过度嵌套，因为它很难阅读。

![](img/930caba5b21794d9f18502ed752339e4.png)

Photo by [Joyce Romero](https://unsplash.com/@joyceromero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

单元测试是测试应用程序一小部分的测试。我们可以把他们组织成小组。

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**寻找一切的链接 plainenglish.io**](https://plainenglish.io/) ！