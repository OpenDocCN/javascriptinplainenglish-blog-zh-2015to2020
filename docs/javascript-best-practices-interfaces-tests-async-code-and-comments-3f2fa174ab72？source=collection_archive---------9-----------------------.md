# JavaScript 最佳实践—接口、测试、异步代码和注释

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-interfaces-tests-async-code-and-comments-3f2fa174ab72?source=collection_archive---------9----------------------->

![](img/f9c555dc26073e25a912e1d8b9d45ca8.png)

Photo by [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 界面分离原理

即使 JavaScript 没有显式接口，我们也不应该让外部代码依赖于它们不使用的接口。

我们应该创造他们都能使用的界面。

例如，我们不是用大量的方法来编写类，而是把它们分成更小的类。

这也使他们不会有一个以上的责任。

# 测试

测试对保持软件质量很重要。

我们可以自动化其中的大部分，使我们的工作更容易。

有些事情我们应该意识到。

# 每个测试一个概念

我们应该通过在每次测试中测试一个概念来简化测试。

例如，不写:

```
import assert from "assert";

describe("Rectangle", () => {
  it("handles dimension changes", () => {
    let rectangle;

    rectangle = new Rectangle(1, 2);
    rectangle.setWidth(30);
    assert.equal(30, rectangle.getWidth());

    rectangle = new Rectangle(1, 2);
    rectangle.setHeight(30);
    assert.equal(30, rectangle.getHeight());
  });
});
```

我们写道:

```
import assert from "assert";

describe("Rectangle", () => {
  it("changes height", () => {
    const rectangle = new Rectangle(1, 2);
    rectangle.setWidth(30);
    assert.equal(30, rectangle.getWidth());
  });

  it("changes width", () => {
    const rectangle = new Rectangle(1, 2);
    rectangle.setHeight(30);
    assert.equal(30, rectangle.getHeight());
  });
});
```

我们创建了两个测试，而不是一个。

# 异步ˌ非同步(asynchronous)

JavaScript 代码是异步的，所以我们必须写很多异步回调函数。

# 使用承诺，而不是回电

我们应该用承诺来做异步操作。

如果我们不得不做多个操作，这就特别有用。

例如，我们写道:

```
import { writeFile, readFile } from "fs";

readFile(
  './foo.txt',
  (err, data)) => {
    if (err) {
      console.error(err);
    } else {
      writeFile("./bar.txt", data, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

我们写道:

```
const fs = require("fs");
const { promisify } = require("util");

const writeFile = promisify(fs.writeFile);
const readFile = promisify(fs.readFile);async function main() {
  const data = await writeFile("/foo.txt");
  await writeFile("./bar.txt", data);
}
```

我们使用`async`和`await`来链接承诺。

`util.promisify`将一些异步函数转换成承诺。

# 错误处理

错误处理很重要。

我们不应该忽略被发现的错误。

例如，不写:

```
try {
  doSomething();
} catch (error) {
  console.log(error);
}
```

我们写道:

```
try {
  doSomething();
} catch (error) {
  console.error(error);
  notifyError(error);
}
```

我们用`console.error`或另一个函数或两者都用，使错误变得明显。

# 不要忽视被拒绝的承诺

我们不应该忽视被拒绝的承诺。

例如，不写:

```
getdata()
  .then(data => {
    doSomething(data);
  })
  .catch(error => {
    console.log(error);
  });
```

我们应该写:

```
getdata()
  .then(data => {
    doSomething(data);
  })
  .catch(error => {
    console.error(error);
    notifyError(error);
  });
```

我们用`console.error`或另一个函数或者两者都用，使错误变得明显。

# 格式化

我们不应该争论格式问题。

这是浪费时间。

# 使用一致的大写

我们应该在代码中使用一致的大写字母。

例如，我们将常量的名称大写:

```
const DAYS_IN_WEEK = 7;
```

此外，对于大多数其他标识符，我们使用 camel case:

```
function eraseDatabase() {}
```

对于构造函数或类，我们使用 Pascal case:

```
class Animal {}
```

# 函数调用者和被调用者应该接近

我们应该让调用者和被调用者离得很近，这样我们就不必跳来跳去地阅读我们的代码。

例如，我们可以写:

```
class Foo {
  foo(){}

  bar() {
    this.foo();
  }
}
```

我们尽可能让他们在一起。

这比:

```
class Foo {
  foo(){} baz(){} qux(){} bar() {
    this.foo();
  }
}
```

我们必须通过多种方法来读取这些内容。

# 评论

我们应该评论那些不能被代码告知的事情。

例如，不写:

```
// loop through objects
for (const item of items) {
  //...
}
```

我们可以省略它，因为代码已经告诉了我们它的作用:

```
for (const item of items) {
  //...
}
```

![](img/dccc252b5d41b4e9f20b6e69c3b34f8c.png)

Photo by [Christian Bowen](https://unsplash.com/@chrishcush?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该有小界面，这样我们就不必依赖大界面，因为大界面中的项目并没有被完全使用。

测试应该有一个单一的概念。

对异步代码使用承诺。

函数调用者和被调用者应该靠得很近。

我们不应该写注释来重复代码中所说的内容。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**