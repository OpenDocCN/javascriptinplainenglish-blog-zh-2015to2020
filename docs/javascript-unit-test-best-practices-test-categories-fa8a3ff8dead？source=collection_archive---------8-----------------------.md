# JavaScript 单元测试最佳实践—测试类别

> 原文：<https://javascript.plainenglish.io/javascript-unit-test-best-practices-test-categories-fa8a3ff8dead?source=collection_archive---------8----------------------->

![](img/86295077fe5871ce3adff7c6be65bff2.png)

Photo by [Andy Holmes](https://unsplash.com/@andyjh07?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 标记我们的测试

我们应该标记我们的测试，以便它们可以在提交或构建运行时运行。

这样，我们可以，我们可以在提交任何东西之前，通过运行最关键的测试来进行健全性检查。

例如，我们可以写:

```
describe("user service", function() {
  describe("log in #sanity", function() {
    test("can log in with the right username and password #sanity", function() {
      //...
    });
  });
});
```

# 将测试分为至少两个级别

我们应该将一些结构应用到我们的测试套件中，以便任何人都能理解需求。

对测试进行分类将会提高测试报告的可读性。

我们可以直接转到所需的部分，找出失败的原因。

例如，我们可以通过书写来对它们进行分类:

```
describe("user service", () => {
  describe("log in", () => {
    it("user can log in with right username and password", () => {}); it("should return error with wrong username or password", () => {});
  });
});
```

一点嵌套帮助我们发现我们的测试更容易。

# 后端测试

我们的测试组合可以通过单元测试之外的测试来改进。

我们还关注集成测试和端到端测试，以改进我们的测试组合。

三种测试结合在一起会有比单元测试更全面的测试覆盖面。

有很多事情单元测试没有涉及到，比如服务间调用和消息队列，它们没有用单元测试来测试。

但是它们可以用端到端测试来测试。

我们可以按照测试金字塔进行大量的单元测试、一些集成测试和一些端到端测试，以测试我们应用程序中最关键的部分。

# 组件测试

单元测试只覆盖了应用程序的一小部分，覆盖所有的部分是很昂贵的。

端到端测试可以覆盖应用程序的多个部分，但它们不可靠且速度较慢。

一个平衡的方法会更合适。

我们可以写出比单元测试更大，比端到端测试更小的东西。

他们可以测试组件，这是一个单元。

我们可以测试整个 API，而不会嘲讽任何属于微服务本身的东西。

我们有真正的数据库，至少有一个内存版本。

这可以让我们在测试中获得更多的信心。

我们仍然保留外部 API 和其他外部依赖。

# 确保新版本不会破坏 API

为了确保我们在向客户发布 API 时不会破坏任何东西，我们应该测试 API 的契约。

这样，我们知道当我们用给定的有效负载发出请求时，我们得到的响应与客户机期望的相同。

这确保了我们可以在客户得到他们的 API 之前测试我们的 API。

![](img/46696135e2e92be067a62c250b7ec330.png)

Photo by [Lukáš Vaňátko](https://unsplash.com/@otohp_by_sakul?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以标记我们的测试，使它们在提交期间快速运行。

此外，我们对测试进行分类，使它们更容易找到。

我们还可以添加组件测试，以确保我们不会破坏任何东西。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[T3【plain English . io找到一切的链接！](https://plainenglish.io/)