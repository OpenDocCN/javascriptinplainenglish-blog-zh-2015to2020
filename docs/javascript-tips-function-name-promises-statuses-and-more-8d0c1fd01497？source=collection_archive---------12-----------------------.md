# JavaScript 提示—函数名、承诺状态等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-function-name-promises-statuses-and-more-8d0c1fd01497?source=collection_archive---------12----------------------->

![](img/fe268d0e5ac01ba3e1485f08465b2d44.png)

Photo by [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 从函数中获取函数名

我们可以通过使用`name`属性从函数中获取函数名。

例如，我们可以写:

```
const foo = () => {
  console.log(foo.name);
}foo();
```

然后我们应该看到`foo`被记录。

# 脚本的加载顺序

如果我们没有动态加载脚本或者用`defer`或`async`标记它们，那么它们会按照在页面上出现的顺序加载。

这个顺序适用于内联脚本和外部脚本。

异步脚本以不可预知的顺序异步加载。

带有`defer`的脚本是异步加载的，但是按照它们遇到的顺序。

这让我们可以异步加载相互依赖的脚本。

# 将 HTML 页面滚动到给定的锚点

我们可以将哈希值设置为`location.hash`的值，以滚动到具有给定 ID 的元素。

例如，我们可以写:

```
const scrollTo = (id) => {
  location.hash = `#${id}`;
}
```

我们传入元素的 ID 来滚动到具有该 ID 的元素。

# 提前解决或拒绝后返回

我们需要在早期解决或拒绝的行后添加一个`return`。

例如，我们可以写:

```
const add = (a, b) => {
  return new Promise((resolve, reject) => {
    if (typeof a !== 'number' || typeof b !== 'number') {
      reject("a or b isn't a number");
      return;
    }
    resolve(a + b);
  });
}
```

我们需要在`reject`调用后添加`return`，这样它下面的代码就不会运行了。

# JavaScript 中的类与静态方法

类方法不同于 JavaScript 中的静态方法。

类方法必须在实例上被调用，并且可以访问`this`，它是类的实例。

静态方法直接在类本身上调用，不能访问`this`。

例如，如果我们有:

```
function Foo(name){
  this.name = name;
}Foo.prototype.bar = function(){
  console.log(this.name);
}
```

然后，如果我们创建一个新的`Foo`实例，我们可以调用`bar`方法:

```
const foo = new Foo('mary');
foo.bar();
```

那么应该记录`'mary'`，因为它是在实例化它时设置的`this.name`属性的值。

另一方面，静态方法被直接定义为构造函数的属性:

```
function Foo(){}
Foo.hello = function() {
  console.log('hello');
}
```

然后我们直接调用`Foo.hello`:

```
Foo.hello();
```

并且`'hello'`被记录。

有了类语法，我们可以把它们都放在一个地方:

```
class Foo {
  constructor(name) {
    this.name = name;
  } bar() {
    console.log(this.name);
  } static hello() {
    console.log('hello');
  }
}
```

`bar`是类方法，`hello`是静态方法。

# 从数组中获取子数组

我们可以用`slice`方法从一个数组中得到一个子数组。

例如，我们可以写:

```
const arr = [1, 2, 3, 4, 5];
const arr2 = arr.slice(1, 4);
```

然后我们得到`arr2`的`[2, 3, 4]`。不包括结束索引。

# 用 JavaScript 预加载图像

为了预加载图像，我们可以用`Image`构造函数创建新的 img 元素，并设置它的`src`属性。

例如，我们可以写:

```
const images = [
  'http://placekitten.com/200/200',
  'http://placekitten.com/201/201',
  'http://placekitten.com/202/202',
]
```

然后我们可以写:

```
for (const image of images) {
  const img = new Image();
  img.src = image;
}
```

# 处理 Promise.all 中的错误

我们可以通过检查结果中是否有任何`Error`实例来处理来自`Promise.allSettled`的错误。

例如，我们可以写:

```
const getAll = async (promises) => {
  const results = await Promise.allSettled(promises);
  console.log(result.map(x => s.status));
}
```

我们得到了`promises`，这是一组承诺。

然后我们调用`Promise.allSettled`从承诺中得到结果。

最后，我们用`status`属性获得每个承诺的状态。

![](img/9b1fc54ccb31452de274ee2e7fc8db1e.png)

Photo by [Daniel Jerez](https://unsplash.com/@danieljerez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`name`属性得到一个函数的名字。

脚本的加载顺序与 HTML 中列出的顺序相同。

类和静态方法是不同的。

我们可以使用`slice`来获得一个数组的子数组。

`Promise.allSettled`可以得到所有承诺的状态。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**