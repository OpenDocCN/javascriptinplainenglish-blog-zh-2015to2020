# JavaScript 单元测试最佳实践—性能和冒烟测试

> 原文：<https://javascript.plainenglish.io/javascript-unit-test-best-practices-performance-and-smoke-tests-721307eb02e4?source=collection_archive---------12----------------------->

![](img/848fed273fd018a2a72db4988912c953.png)

Photo by [Tomasz Sroka](https://unsplash.com/@srook?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 观察内容是如何通过网络提供的

我们想知道我们的内容通过网络提供给用户的速度有多快。

为了衡量这一点，我们可以使用类似 pingdom 或 Lighthouse 这样的工具。

它们作为程序提供，我们可以将其添加到 CI 渠道中，以确保持续监控。

他们向我们展示了各种格式的测试结果。

# 存根片状和缓慢的资源，如后端 API

如果我们运行前端测试，那么像后端 API 这样的慢速资源应该被剔除。

这样，我们可以尽可能快地运行我们的前端测试。

我们可以用不同的库来存根它们。

这让我们可以模拟各种 API 行为，以便为我们的前端提供所需的数据。

没有存根数据，测试将会缓慢而可靠。

例如，我们可以编写这样一个测试:

```
test("show message when product doesn't exist", () => {
  nock("api")
    .get(`/products`)
    .reply(404); const { getByTestId } = render(<ProductsList />);
  expect(getByTestId("no-products-message")).toBeTruthy();
});
```

我们用`nock`存根 API 调用，这样我们就不必进行实际的 API 调用。

# 进行一些跨整个系统的端到端测试

我们应该只有几个横跨整个系统的端到端测试。

它们很慢，所以应该保留下来用于测试我们系统中最关键的部分。

他们模拟真实的用户交互，所以我们知道他们在用户交互中的行为是正确的。

它们也很脆，所以很难运行。

此外，他们应该在类似生产的环境中运行，这样他们就可以测试一些真实的东西。

# 通过重用登录凭证加速 E2E 测试

我们应该登录一次，然后做所有的测试。

登录需要额外的时间，所以我们应该把它留到开始。

我们可以将登录代码放入一个 before all 钩子中，以便它在所有测试运行之前运行。

任何与用户相关的记录都应该与测试一起生成。

我们可以用 Cypress 保存 auth 令牌，例如:

```
let authenticationToken;before(() => {
  cy.request('POST', 'http://localhost:8888/login', {
    username: Cypress.env('username'),
    password: Cypress.env('password'),
  })
  .its('body')
  .then((res) => {
    authenticationToken = res.token;
  })
})beforeEach(setUser => () {
  cy.visit('/profile', {
    onBeforeLoad (win) {
      win.localStorage.setItem('token', JSON.stringify(authenticationToken))
    },
  })
})
```

我们从环境变量中获取用户名和密码。

然后，我们使用它登录，并通过使用 API 而不是 GUI 来获取令牌。

然后我们得到令牌并在每次测试前使用它。

# E2E 烟尘测试，刚刚通过网站地图

端到端的测试只是在整个站点上运行，确保我们站点的所有部分都工作正常。

它易于维护，可以发现任何功能、网络或部署问题。

其他种类的烟雾测试并不可靠或详尽。

有了柏树，我们可以写下:

```
it("can go to different pages", () => {
  cy.visit("https://example.com/home");
  cy.contains("Home");
  cy.contains("https://example.com/profile");
  cy.contains("Profile");
  cy.contains("https://example.com/about");
  cy.contains("About");
});
```

![](img/3faa6a156b76050e3e398b0995166c9d.png)

Photo by [Marc-Olivier Jodoin](https://unsplash.com/@marcojodoin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加各种测试来测试性能和冒烟测试。

## **简明英语 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**找到一切的链接 plainenglish.io**](https://plainenglish.io/) ！