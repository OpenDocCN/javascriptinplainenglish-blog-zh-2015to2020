# JavaScript 单元测试最佳实践—组织

> 原文：<https://javascript.plainenglish.io/javascript-unit-test-best-practices-organization-e14240ef2582?source=collection_archive---------10----------------------->

![](img/73b94edeb1b4081893f6f04366b25342.png)

Photo by [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 精益测试设计

测试代码应该简单、简短、无抽象。

它们也应该是瘦的。

我们的测试代码越简单，就越好。

我们只是做一些事情，然后在测试中检查结果。

# 每个测试名称包含 3 个部分

我们应该在每个测试名称中有 3 个部分。

第一部分是被测试的模块。

第二个是我们测试代码的环境。

预期的结果就是我们在测试中要检查的。

这样，我们可以知道正在测试什么，并找出在失败时应该修复什么。

例如，我们写道:

```
describe('User Service', function() {
  describe('Add new user', function() {
    it('When no password is specified, then the product status is pending', ()=> {
      const user = new UserService().add(...);
      expect(user.status).to.equal('pending');
    });
  });
});
```

# AAA 模式的结构测试

我们应该用 AAA 模式来组织我们的测试。

3 A 代表安排、行动和主张。

Arrange 意味着添加设置代码是为了让我们进行测试。

Act 正在做测试。

断言是在做了我们已经做的事情之后检查结果。

所以如果我们有:

```
describe('User Service', function() {
  describe('login', function() {
    it('if the password is wrong, then we cannot log in', () => {
      const user = new UserService().add({ username: 'james', password: '123456' });
      const result = login('james', '123'); 
      expect(result).to.equal(false);
    });
  });
});
```

我们安排了第一行的测试。

Act 是对`login`函数的调用。

断言是`expect`调用。

# 用产品语言描述期望

我们应该用类似人类的语言来描述测试。

行为应该被描述，以便我们能够理解测试在做什么。

`expect`或`should`应该是人类可读的。

例如，我们写道:

```
describe('User Service', function() {
  describe('Add new user', function() {
    it('When no password is specified, then the product status is pending', ()=> {
      const user = new UserService().add(...);
      expect(user.status).to.equal('pending');
    });
  });
});
```

我们传递给`describe`和`it`的字符串清楚地解释了场景和测试。

# 只测试公共方法

我们应该只测试公共方法，这样我们就不用测试实现了。

我们不关心测试的实现。

我们只关心结果。

当我们没有从测试中得到预期的结果时，我们应该看看它们。

这就是所谓的行为测试。

我们测试的是行为，不是别的。

例如，我们不应该编写这样的代码:

```
it('should add a user to database', () => {
  userManager.updateUser('james', 'password'); expect(userManager._users[0].name).toBe('james');
  expect(userManager._users[0].password).toBe('password');
});
```

我们不应该测试随时可能改变的私有变量。

# 避免嘲笑树桩和间谍

因为我们不能像在真实环境中那样在测试中做所有的事情，所以我们必须消除一些依赖性。

我们的测试不应该依赖于外界的任何东西，它们应该是孤立的。

因此，我们删除了所有会产生副作用的依赖项，这样我们就可以单独测试我们想要测试的东西。

我们只是存根任何网络请求代码。

而我们看我们要检查的东西就叫做有间谍。

例如，不要模仿这样的数据库:

```
it("should delete user", async () => {
  //...
  const dataAccessMock = sinon.mock(DAL);
  dataAccessMock
    .expects("deleteUser")
    .once()
    .withArgs(DBConfig, user, true, false);
  new UserService().delete(user);
  dataAccessMock.verify();
});
```

我们监视我们检查的函数，它是通过编写:

```
it("should delete user", async () => {
  const spy = sinon.spy(Emailer.prototype, "sendEmail");
  new UserService().delete(user);
  expect(spy.calledOnce).to.be.true;
});
```

我们窥探了`Emailer`构造函数的`sendEmail`方法，检查它是否是在我们调用了`UserService`实例的`delete`方法之后被调用的。

我们避免用 spies 方法模拟任何服务器端的交互。

![](img/68c1d868797bb33079e1d6a88bbb8e58.png)

Photo by [Scott Graham](https://unsplash.com/@sctgrhm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该用树桩和间谍来测试。

测试应该是精简的，将我们的测试分为 3 个部分。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 找到所有内容的链接！