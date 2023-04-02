# 茉莉——间谍和媒人

> 原文：<https://javascript.plainenglish.io/jasmine-spies-and-matchers-6338ec628a92?source=collection_archive---------6----------------------->

![](img/3a5574cfae7f703545d6d394cca7f861.png)

Photo by [Pharma Hemp Complex](https://unsplash.com/@pharmahempcomplex?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

测试是 JavaScript 的一个重要部分。

在本文中，我们将看看如何用 Jasmine 创建更复杂的测试。

# `createSpy`

我们可以调用`createSpy`来存根一个函数。

它们可以用来跟踪争论。

例如，我们可以写:

```
describe("A spy", function () {
  let spy; beforeEach(function () {
    spy = jasmine.createSpy('spy');
    spy('foo', 'bar');
  }); it("tracks that the spy was called", function () {
    expect(spy).toHaveBeenCalled();
  });
});
```

我们用`jasmine.createSpy`创造了一个间谍。

它返回一个可以用 Jasmine 观看的函数。

这样我们就可以检查它是否已经用`toHaveBeenCalled`调用过了。

# `createSpyObj`

我们可以使用`createSpyObj`方法创建一个有多个间谍的模拟。

例如，我们可以写:

```
describe("Multiple spies", function () {
  let person; beforeEach(function () {
    person = jasmine.createSpyObj('person', ['eat', 'drink', 'sleep', 'walk']);
    person.eat();
    person.drink();
    person.sleep(0);
    person.walk(0);
  }); it("creates spies for each requested function", function () {
    expect(person.eat).toBeDefined();
    expect(person.drink).toBeDefined();
    expect(person.sleep).toBeDefined();
    expect(person.walk).toBeDefined();
  });
});
```

我们用间谍名作为第一个参数调用了`createSpyObj`。

以及第二个参数中的 spy 对象中可用的方法。

然后我们可以检查这些方法是否被定义

# 匹配和间谍

我们可以添加更多匹配的支票。

例如，我们可以写:

```
describe("Tests", function () {
  it("matches any value", function () {
    expect({}).toEqual(jasmine.any(Object));
    expect(12).toEqual(jasmine.any(Number));
  }); describe("when used with a spy", function () {
    it("is useful for comparing arguments", function () {
      const foo = jasmine.createSpy('foo');
      foo(12, () => true);
      expect(foo).toHaveBeenCalledWith(jasmine.any(Number), jasmine.any(Function));
    });
  });
});
```

我们可以使用`toEqual`来检查带有`jasmine.any`的值的类型。

我们可以使用`jasmine.any`和`toHaveBeenCalledWith`来检查是否调用了一个带有给定数据类型的间谍。

# 茉莉，什么都行

我们可以使用`jasmine.anythibng`来检查某个东西是否匹配不属于`null`或`undefined`的任何东西。

例如，我们可以写:

```
describe("jasmine.anything", function () {
  it("matches anything", function () {
    expect(1).toEqual(jasmine.anything());
  }); describe("when used with a spy", function () {
    it("is useful when the argument can be ignored", function () {
      const foo = jasmine.createSpy('foo');
      foo(12, 'bar');
      expect(foo).toHaveBeenCalledWith(12, jasmine.anything());
    });
  });
});
```

我们有`jasmine.anything()`调用来检查是否有任何东西不是`null`或`undefined`。

# 检查对象结构

我们可以用`jasmine.objectContaining`方法检查对象结构。

例如，我们可以写:

```
describe("jasmine.objectContaining", function () {
  let foo; beforeEach(function () {
    foo = {
      a: 1,
      b: 2,
      bar: "baz"
    };
  }); it("matches objects with key-value pairs", function () {
    expect(foo).toEqual(jasmine.objectContaining({
      a: 1, bar: "baz"
    }));
  }); describe("when used with a spy", function () {
    it("is useful for comparing arguments", function () {
      const callback = jasmine.createSpy('callback'); callback({
        bar: "baz"
      }); expect(callback)
        .toHaveBeenCalledWith(
          jasmine.objectContaining({
            bar: "baz"
          })
        );
    });
  });
});
```

我们有`jasmine.objectContaining`方法来观察对象结构。

我们只是传入想要检查的键值对。

它可以与值和函数参数一起使用。

# 结论

我们可以用各种 Jasmine 方法检查值和对象，并监视间谍。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**