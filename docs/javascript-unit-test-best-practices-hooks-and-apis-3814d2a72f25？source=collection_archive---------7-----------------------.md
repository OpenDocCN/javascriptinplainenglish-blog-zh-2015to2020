# JavaScript 单元测试最佳实践—钩子和 API

> 原文：<https://javascript.plainenglish.io/javascript-unit-test-best-practices-hooks-and-apis-3814d2a72f25?source=collection_archive---------7----------------------->

![](img/015b3fe2e259fc95a1ac3c75c42a8f4c.png)

Photo by [Anders Wideskott](https://unsplash.com/@wideshot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 正确设置适用于所有相关测试的操作

如果我们在每次测试之前运行相同的东西，我们应该把它放在一个`beforeEach`钩子中。

这样，我们在每次测试之前运行相同的代码，而不需要重复代码。

例如，我们可以写:

```
describe('Saving the user profile', () => { beforeEach(() => {
    login();
  }); it('should save updated profile setting to database', () => {
    //... expect(request.url).toBe('/profiles/1');
    expect(request.method).toBe('POST');
    expect(request.data()).toEqual({ username: 'james' });
  }); it('should notify the user', () => {
    //...
  });
}); it('should redirect user', () => {
    //...   
  });
});
```

同样，如果我们有一些代码需要在每次测试后运行，我们应该有一个`afterEach`钩子在每次测试后运行:

```
describe('Saving the user profile', () => { beforeEach(() => {
    login();
  }); afterEach( () => {
    logOut();
  }); it('should save updated profile setting to database', () => {
    //... expect(request.url).toBe('/profiles/1');
    expect(request.method).toBe('POST');
    expect(request.data()).toEqual({ username: 'james' });
  }); it('should notify the user', () => {
    //...
  }); it('should redirect user', () => {
    //...   
  });
});
```

# 考虑在测试中使用工厂函数

工厂函数有助于减少安装代码。

它们使每个测试更具可读性，因为创建是在单个函数调用中完成的。

并且它们在创建新实例时提供了灵活性。

例如，我们可以写:

```
describe('User profile module', () => {
  let user; beforeEach(() => {
    user = createUser('james');
  }); it('should publish a topic "like" when like is called', () => {
    spyOn(user, 'notify');
    user.like();
    expect(user.notify).toHaveBeenCalledWith('like', { count: 1 });
  }); it('should retrieve the correct number of likes', () => {
    user.like();
    user.like();
    expect(user.getLikes()).toBe(2);
  });
});
```

我们有`createUser`函数，通过一次函数调用来创建一个用户。

这样，我们不必为每个测试编写相同的设置代码。

我们也可以在 DOM 测试中使用它们:

```
function createSearchForm() {
  fixtures.inject(`<div id="container">
    <form class="js-form" action="/search">
      <input type="search">
      <input type="submit" value="Search">
    </form>
  </div>`); const container = document.getElementById('container');
  const form = container.getElementsByClassName('js-form')[0];
  const searchInput = form.querySelector('input[type=search]');
  const submitInput = form.querySelector('input[type=submit]'); return {
    container,
    form,
    searchInput,
    submitInput
  };
}describe('search component', () => {
  describe('when the search button is clicked', () => {
    it('should do the search', () => {
      const { container, form, searchInput, submitInput } = createSearchForm();
      //...
      expect(search.validate).toHaveBeenCalledWith('foo');
    }); // ...
  });
});
```

我们在`createSearchForm`函数中有搜索表单创建代码。

在函数中，我们返回表单的 DOM 对象的各个部分，以便检查代码。

# 使用测试框架的 API

我们应该利用测试框架的 API。

这样，我们就可以利用它的功能来简化测试。

例如，我们可以写:

```
fit('should call baz with the proper arguments', () => {
  const foo = jasmine.createSpyObj('foo', ['bar', 'baz']);
  foo.baz('baz');
  expect(foo.baz).toHaveBeenCalledWith('baz');
});it('should do something else', () => {
  //...
});
```

用茉莉。

我们窥探了存根函数，看看它们是否是用`createSpyObj`方法调用的。

我们使用`fit`以便只运行第一个测试。

![](img/30d72475cafbd230448faf392913038e.png)

Photo by [Lynda Hinton](https://unsplash.com/@lyndaann1975?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该确定测试框架的 API 以使测试更容易。

此外，我们应该确保将重复的代码放在钩子中，以避免重复。

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！