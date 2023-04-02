# 现代 JavaScript 类型数组的精华

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-typed-arrays-dbe6038cb281?source=collection_archive---------11----------------------->

![](img/de2100ec5541d9230d9516a25fd77274.png)

Photo by [Ben Hershey](https://unsplash.com/@benhershey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 类型化数组。

# 箝位转换

钳位转换与模块转换的工作方式不同。

JavaScript 为我们提供了具有类型化数组的构造函数，这种构造函数可以进行钳位转换，比如`Uint8ClampedArray`构造函数。

它与模转换的工作方式不同，因为所有下溢值都被转换为最低值。

并且所有溢出值都被转换为最高值。

例如，我们可以通过编写以下内容来创建一个:

```
const uint8c = new Uint8ClampedArray(1);
```

那么如果我们写:

```
uint8c[0] = 255;
```

我们得到 255 作为`uint8c[0]`的值。

如果我们写:

```
uint8c[0] = 256;
```

我们仍然得到 255。

另一方面，如果我们写:

```
uint8c[0] = 0;
```

我们得到 0。

如果我们写下:

```
uint8c[0] = -1;
```

我们仍然得到 0。

# 字节序

如果我们在类型化数组中存储多个字节，那么字节序就很重要。

Big-endian 意味着最重要的字节先出现。

所以如果我们有 2 个字节像`0xABCD`，那么`0xAB`先来，然后是`0xCD`。

Little-endian 意味着最不重要的字节排在最前面。

这意味着数字的存储顺序与它们在大端数组中的存储顺序相反。

CPU 架构之间的字节顺序不同，但在本地 API 之间是一致的。

类型化数组使我们能够与这些 API 进行通信。

这就是为什么他们的字节序不能改变。

二进制文件和协议的字节顺序跨平台是固定的。

数据视图允许我们指定字节序，这样我们就可以跨多个平台传输文件并使用不同的协议进行通信。

# 负指数

负指数可与`slice`方法一起使用。

Index -1 表示类型化数组的最后一个元素。

例如，我们可以写:

```
const arr = Uint8Array.of(0, 1, 2);
const last = arr.slice(-1);
```

我们用一些数字创建了一个`Uint8Array`。

然后我们用-1 调用了`slice`，用`arr`的最后一个条目返回一个类型化数组。

偏移量必须是非负整数。

因此，如果我们向`DataView.prototype.getInt8`方法传递一个负数，我们将得到一个 RangeError。

# 数组缓冲器

`ArrayBuffers`存储数据和视图让我们读取和更改它们。

为了创建 DataView，我们需要为构造函数提供一个 ArrayBuffer。

类型化数组构造函数可以有选择地为我们创建 ArrayBuffers。

`ArrayBuffer`构造函数接受一个数组缓冲区长度的数字。

如果我们传入的参数是一个 ArrayBuffers 的视图，那么`ArrayBuffer`构造函数有`isView`静态方法返回`true`。

只有类型数组和数据视图有使它们成为视图的`[[ViewedArrayBuffer]]`内部槽。

`ArrayBuffer.prototype.byteLength`是一个实例方法，以字节为单位返回 ArrayBuffer 的容量。

这是一个 getter 方法。

`ArrayBuffer.prototype.slice(start, end)`是一个实例方法，返回一个索引大于等于`start`小于`end`的新 ArrayByffer。

`start`和`end`可以是负数。

![](img/93b4b5d4de930b6388e59731623b8ca5.png)

Photo by [Joseph Balzano](https://unsplash.com/@josephbalzanodev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

类型化数组可以用各种方式包装值。

此外，我们必须有正确的字节顺序来正确地存储和交流数据。

ArrayBuffers 让我们存储二进制数据并切片。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**