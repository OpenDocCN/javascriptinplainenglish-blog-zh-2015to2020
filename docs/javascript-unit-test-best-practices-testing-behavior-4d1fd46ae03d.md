# JavaScript 单元测试最佳实践—测试行为

> 原文：<https://javascript.plainenglish.io/javascript-unit-test-best-practices-testing-behavior-4d1fd46ae03d?source=collection_archive---------7----------------------->

![](img/63ffdb88c2fbedb79479e4308d3a95a8.png)

Photo by [Battlecreek Coffee Roasters](https://unsplash.com/@battlecreekcoffeeroasters?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 测试行为而不是内部实现

我们应该只测试结果，不要担心内部实现。

这样，我们就不会担心一些不需要在测试中检查的东西。

例如，我们不应该测试内部变量:

```
it('should add a user to database', () => {
  userManager.addUser('james', 'password'); expect(userManager._users[0].name).toBe('james');
  expect(userManager._users[0].password).toBe('password');
});
```

相反，我们写道:

```
it('should add a user to database', () => {
  userManager.addUser('james', 'password');
  expect(userManager.login('james', 'password')).toBe(true);
});
```

我们只是测试返回的结果，而不是内部变量。

这样，当实现改变时，我们不必改变我们的测试。

# 不要嘲笑一切

我们不应该嘲笑一切。

这样，至少我们在测试一些东西。

例如，我们可以写:

```
describe('when the user has already visited the page', () => {
  describe('when the message is available', () => {
    it('should display the message', () => {
      const storage = new MemoryStorage(); 
      storage.setItem('page-visited', '1');
      const messageManager = new MessageManager(storage);
      spyOn(messageManager, 'display');
      messageManager.start();
      expect(messageManager.display).toHaveBeenCalled();
    });
  });
});
```

我们使用内存存储解决方案，而不是真正的本地存储。

这样，我们的测试不会对我们的测试产生任何副作用。

我们没有模仿`messageManager.display`，因为我们只是想检查它是否被调用。

如果设置简单，我们使用真实版本的对象。

他们也不应该在测试之间创建共享状态。

实物如果用的话速度应该很快。

真实对象也不应该发出任何网络请求或重新加载浏览器页面。

# 为每个缺陷创建新的测试

应该有针对所有已修复缺陷的新测试。

这样，我们可以修复它，并且永远不会让它以相同的形式再次出现。

# 不要为复杂的用户交互编写单元测试

单元测试应该用来测试简单的动作。

如果我们想要测试更复杂的工作流，那么我们应该添加集成或者端到端测试。

它们都是更复杂的工作流所需要的，比如填表和提交数据等。

功能测试可以用 Selenium 或 Cypress 这样的框架来编写。

# 测试简单的用户操作

我们应该测试简单的用户动作，比如点击和输入。

例如，我们可以写:

```
describe('clicking on the "Preview profile" link', () => {
  it('should show the profile preview if it is hidden', () => {
    const button = document.querySelector('button');
    const profileModule = createProfileModule({ visible: false });   
    spyOn(profileModule, 'show'); 
    button.click(previewLink);
    expect(profileModule.show).toHaveBeenCalled();
  }); it('should hide the profile preview if it is displayed', () => {
    const button = document.querySelector('button');
    const profileModule = createProfileModule({ visible: true });
    spyOn(profileModule, 'hide');
    button.click();
    expect(profileModule.hide).toHaveBeenCalled();
  });
});
```

我们有各种状态的`profileModule`,我们点击每个状态。

然后我们检查哪个函数被调用。

# 审查测试代码

应该查看测试代码，这样我们就能很快知道开发人员的意图。

![](img/3315cd48a65a95e7e9ada4c6521ff190.png)

Photo by [National Cancer Institute](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该在测试中测试简单的行为。

此外，我们不应该嘲笑一切来进行更真实的测试。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 找到所有内容的链接！