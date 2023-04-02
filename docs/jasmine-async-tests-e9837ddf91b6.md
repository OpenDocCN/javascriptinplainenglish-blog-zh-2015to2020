# Jasmine —异步测试

> 原文：<https://javascript.plainenglish.io/jasmine-async-tests-e9837ddf91b6?source=collection_archive---------4----------------------->

![](img/b92656d16e14396434c36296e8ce01c1.png)

Photo by [Avin CP](https://unsplash.com/@avincp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

测试是 JavaScript 的一个重要部分。

在本文中，我们将看看如何用 Jasmine 创建更复杂的测试。

# 测试承诺

我们可以用茉莉来测试承诺。

`beforeAll`、`afterAll`、`beforeEach`和`afterEach`功能都支持承诺。

例如，我们可以写:

```
describe("Using promises", function () {
  let value; beforeEach(function () {
    return Promise.resolve().then(function () {
      value = 0;
    });
  }); it("should support async execution of tests", function () {
    return Promise.resolve().then(function () {
      value++;
      expect(value).toBeGreaterThan(0);
    });
  });
});
```

我们在`beforeEach`和`it`的复试中给`then`打了电话。

然后我们可以检查那里的值。

它也适用于`async`和`await`语法。

例如，我们可以写:

```
describe("Using promises", function () {
  let value; beforeEach(async function () {
    await Promise.resolve()
    value = 0;
  }); it("should support async execution of tests", async function () {
    await Promise.resolve()
    value++;
    expect(value).toBeGreaterThan(0);
  });
});
```

我们只是用`async`和`await`语法重写了承诺。

在这两个例子中，它们将按顺序运行。

测试从`beforeEach`回调开始，然后运行`it`回调。

# 改变等待时间

`beforeEach`和`afterEach`函数采用可选的第二个参数来设置等待，直到`done`完成。

例如，我们可以写:

```
describe("long asynchronous specs", function () {
  beforeEach(function (done) {
    done();
  }, 1000); it("takes a long time", function (done) {
    setTimeout(function () {
      done();
    }, 9000);
  }, 10000); afterEach(function (done) {
    done();
  }, 1000);
});
```

传递第二个参数，等待时间以毫秒为单位。

# 处理故障

用承诺处理失败可以像任何其他代码一样完成。

如果一个承诺在我们的测试中被拒绝，那么测试就会失败。

例如，如果我们有:

```
describe("promise test", function () {
  it('does a thing', function () {
    return Promise.reject()
  });
});
```

那么测试就会失败。

使用`async`和`await`，我们得到相同的结果:

```
describe("long asynchronous specs", function () {
  it('does a thing', async function () {
    await Promise.reject()
  });
});
```

这也会失败。

我们可以通过复试来让测试失败。

从 Jasmine 3.0 开始，`done`函数可以检测是否有一个`Error`对象被传递给该函数。

如果是，那么测试将失败。

例如，我们可以写:

```
describe("long asynchronous specs", function () {
  beforeEach(function (done) {
    setTimeout(function () {
      let err; try {
        throw new Error()
      } catch (e) {
        err = e;
      } done(err);
    });
  }); it('does something', () => { })
});
```

我们的`beforeEach`回调抛出一个错误。

因此，既然我们用`err`对象调用`done`，我们将会看到测试将会失败。

# 模拟时钟

我们可以使用模拟时钟来避免编写异步测试。

它将使异步代码同步运行。

例如，我们可以写:

```
describe("long asynchronous specs", function () {
  beforeEach(function () {
    jasmine.clock().install();
  }); afterEach(function () {
    jasmine.clock().uninstall();
  }); it('does something after 10 seconds', function () {
    const callback = jasmine.createSpy('callback');
    setTimeout(function () {
      callback('foo');
    }, 10000);
    jasmine.clock().tick(10000);
    expect(callback).toHaveBeenCalledWith('foo');
  });
});
```

用模拟时钟创建一个测试。

我们调用`jasmine.clock().install()`来创建时钟。

然后我们通过调用`tick`运行测试代码，移动到我们想要的时间。

然后当测试完成时，我们调用`jasmine.clock().uninstall()`来移除模拟时钟。

![](img/bf68f682b6abda420b8d2de91337a8ee.png)

Photo by [Autumn Kuney](https://unsplash.com/@autumnkuney?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Jasmine 测试各种类型的异步代码。

承诺和回调都是支持的。

我们可以让计时器代码与模拟时钟同步。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**