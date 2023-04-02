# 用 Jest 运行重复的测试代码

> 原文：<https://javascript.plainenglish.io/running-repetitive-test-code-with-jest-d71bda3eb01d?source=collection_archive---------3----------------------->

![](img/43f65a916cd7dd0c455de9600d3d90ff.png)

Photo by [Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在我们的单元测试中，我们经常不得不编写在每次测试前后运行的测试代码。我们必须经常编写它们来运行代码，在测试前设置夹具，并在测试后运行代码来清理一切。

我们可以很容易地用 Jest 做到这一点，因为它附带了一些钩子来做到这一点。

在本文中，我们将看看如何以不重复的方式编写重复的安装和拆卸代码。

# 如何编写安装和拆卸代码

有了 Jest，我们可以通过使用`beforeEach`和`afterEach`钩子来编写安装和拆卸代码。

`beforeEach`在每次测试前运行代码，`afterEach`在每次测试后运行代码。该顺序适用于一个`describe`块内，如果没有`describe`块，则适用于整个文件。

例如，假设我们在`example.js`中有以下代码:

```
const getStorage = (key) => localStorage.getItem(key);
const setStorage = (key, value) => localStorage.setItem(key, value);
module.exports = {
    getStorage,
    setStorage
}
```

我们为他们编写的测试如下:

```
const { getStorage, setStorage } = require('./example');beforeEach(() => {
    localStorage.setItem('foo', 1);
    expect(localStorage.getItem('bar')).toBeNull();
});afterEach(() => {
    localStorage.clear();
});test('getStorage("foo") is 1', () => {
    expect(getStorage('foo')).toBe('1');
});test('setStorage saves data to local storage', () => {
    setStorage('bar', 2);
    const bar = +localStorage.getItem('bar');
    expect(getStorage('foo')).toBe('1');
    expect(bar).toBe(2);
});
```

我们向`beforeEach`钩子传递一个回调函数，在下面的每个测试之前运行代码。

在这个例子中，我们用一个带有键`'foo'`和值`'1'`的条目预填充本地存储。

我们还检查本地存储中是否没有关键字为`'bar’`的项目:

```
expect(localStorage.getItem('bar')).toBeNull();
```

由于我们将在`afterEach`钩子中运行`localStorage.clear()`,所有测试都将通过上面的`expect`。

同样，我们在每个测试运行之后，向`afterEach`传递一个回调来运行代码。

我们通过以下方式清除本地存储:

```
localStorage.clear();
```

然后当运行测试时，我们得到键为`'bar'`的条目是`null`，因为我们在第一次测试中没有填充它。

在第二个测试中，我们将让那个`expect(getStorage(‘foo’)).toBe(‘1’);`通过，因为我们在我们的`beforeEach`钩子中用它填充了本地存储。

然后因为我们跑了:

```
setStorage('bar', 2);
```

要保存一个带有键`'bar'`和值`'2'`的项目，我们将得到:

```
expect(bar).toBe(2);
```

通过，因为我们在测试中保存了项目。

![](img/ad31672ddc2be7993a61b28ec5513eda.png)

Photo by [Hal Gatewood](https://unsplash.com/@halgatewood?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 异步代码

在上面的例子中，我们在钩子中运行同步代码。如果钩子接受一个`done`参数或者返回一个承诺，我们也可以在钩子中运行异步代码。

如果我们想运行一个接受回调的函数，并按如下方式异步运行它:

```
const asyncFn = (callback) => {
    setTimeout(callback, 500);
}
```

然后在`beforeEach`钩子中，我们可以如下运行`asyncFn`:

```
const { asyncFn } = require('./example');beforeEach((done) => {
    const callback = () => {
        localStorage.setItem('foo', 1);
        done();
    }
    asyncFn(callback);
    expect(localStorage.getItem('bar')).toBeNull();
});
```

它和前面的`beforeEach`回调做同样的事情，只是它是异步完成的。注意，我们在回调中调用从参数传入的`done`。

如果我们忽略它，测试就会超时而失败。测试中的代码和以前一样。

我们可以通过在传递给钩子的回调中返回承诺来等待承诺的解决。

例如，在`example.js`中，我们可以编写以下函数来运行一个承诺:

```
const promiseFn = () => {
    return new Promise((resolve) => {
        localStorage.setItem('foo', 1);
        resolve();
    });
}
```

然后我们可以将`promiseFn`放在`module.exports`中，然后在我们的`beforeEach`中运行:

```
beforeEach(() => {
    expect(localStorage.getItem('bar')).toBeNull();
    return promiseFn();
});
```

我们运行了在这个钩子之前导入的`promiseFn`,并且我们返回了由那个函数返回的承诺，它像第一个例子一样设置本地存储，除了它是异步完成的。

然后，在我们把所有东西放在一起之后，我们在`example.js`中有了下面的代码，我们在钩子中的测试代码中运行这些代码并进行测试:

```
const getStorage = (key) => localStorage.getItem(key);
const setStorage = (key, value) => localStorage.setItem(key, value);
const asyncFn = (callback) => {
    setTimeout(callback, 500);
}
const promiseFn = () => {
    return new Promise((resolve) => {
        localStorage.setItem('foo', 1);
        resolve();
    });
}
module.exports = {
    getStorage,
    setStorage,
    asyncFn,
    promiseFn
}
```

然后我们用`asyncExample.test.js`将钩子改为异步的:

```
const { getStorage, setStorage, asyncFn } = require('./example');beforeEach((done) => {
    const callback = () => {
        localStorage.setItem('foo', 1);
        done();
    }
    asyncFn(callback);
    expect(localStorage.getItem('bar')).toBeNull();
});afterEach(() => {
    localStorage.clear();
});test('getStorage("foo") is 1', () => {
    expect(getStorage('foo')).toBe('1');
});test('setStorage saves data to local storage', () => {
    setStorage('bar', 2);
    const bar = +localStorage.getItem('bar');
    expect(getStorage('foo')).toBe('1');
    expect(bar).toBe(2);
});
```

那么在`example.test.js`中我们有:

```
const { getStorage, setStorage } = require('./example');beforeEach(() => {
    localStorage.setItem('foo', 1);
    expect(localStorage.getItem('bar')).toBeNull();
});afterEach(() => {
    localStorage.clear();
});test('getStorage("foo") is 1', () => {
    expect(getStorage('foo')).toBe('1');
});test('setStorage saves data to local storage', () => {
    setStorage('bar', 2);
    const bar = +localStorage.getItem('bar');
    expect(getStorage('foo')).toBe('1');
    expect(bar).toBe(2);
});
```

最后，在`promiseExample.test.js`中，我们有:

```
const { getStorage, setStorage, promiseFn } = require('./example');beforeEach(() => {
    expect(localStorage.getItem('bar')).toBeNull();
    return promiseFn();
});afterEach(() => {
    localStorage.clear();
});test('getStorage("foo") is 1', () => {
    expect(getStorage('foo')).toBe('1');
});test('setStorage saves data to local storage', () => {
    setStorage('bar', 2);
    const bar = +localStorage.getItem('bar');
    expect(getStorage('foo')).toBe('1');
    expect(bar).toBe(2);
});
```

# 之前和之后

为了在每个文件中只运行一次安装和拆卸代码，我们可以使用`beforeAll`和`afterAll`钩子。我们可以像处理`beforeEach`和`afterEach`钩子一样，用我们想要运行的代码传入回调。

唯一的区别是回调在文件中的测试运行之前和文件中的测试运行之后运行一次，而不是在每次测试之前和之后运行。

`beforeAll`的回调在`beforeEach`的回调之后运行，而`afterAll`的回调在`afterEach`的回调之后运行。

该顺序适用于一个`describe`块内，如果没有`describe`块，则适用于整个文件。

# 仅运行一个测试

我们可以编写`test.only`而不是`test`来只运行一个测试。这对于故障排除是很方便的，因为我们不必运行所有的测试，它帮助我们锁定失败的测试的问题。

为了编写所有测试都需要运行的测试代码，我们开玩笑地使用了`beforeEach`和`afterEach`钩子。

为了编写只在每个`describe`块或文件中运行的测试代码，我们可以使用`beforeAll`和`afterAll`钩子。

`beforeAll`的回调在`beforeEach`的回调之后运行，而`afterAll`的回调在`afterEach`的回调之后运行。