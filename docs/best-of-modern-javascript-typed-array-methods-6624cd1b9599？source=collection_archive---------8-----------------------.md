# 最好的现代 JavaScript 类型数组方法

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-typed-array-methods-6624cd1b9599?source=collection_archive---------8----------------------->

![](img/b96b79cc3066a707f1cd8005eccdaaaa.png)

Photo by [Isi Parente](https://unsplash.com/@isiparente?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 类型化数组方法。

# 静态方法

静态方法让我们将类似数组或可迭代的对象转换成类型化数组。

第一个参数是我们要转换成类型化数组的 iterable 对象。

第二个是回调函数，它将条目的值和索引作为数组的参数，让我们返回我们想要的东西。

第三个参数是我们希望在第二个参数的回调中使用的`this`的值。

第二个和第三个参数是可选的。

例如，我们可以写:

```
const typedArr = Uint16Array.from([0, 1, 2]);
```

我们传入数组值并返回一个包含数组中条目的`Uint16Array`。

映射条目的回调让我们创建一个不会溢出的新数组，而不是让它们溢出。

例如，不要写:

```
const arr = Int8Array.of(200, 201, 202).map(x => 2 * x)
```

并得到值为`[-112, -110, -108]`的`Int8Array`。
我们可以写:

```
const arr = Int32Array.from(Int8Array.of(120, 121, 122), x => x * 2)
```

并在一个`Int32Array`中得到`[240, 242, 244]`。

# 实例属性

类型化数组包含许多实例属性。

属性是一个 getter，它返回存储类型化数组数据的缓冲区。

`byteLength`属性是一个以字节为单位返回类型化数组缓冲区大小的数字。

`byteOffset`返回 ArrayByffr 中类型化数组开始的偏移量。

`set`将数组或类型化数组的所有元素复制到给定的类型化数组中。

如果是一个普通数组，那么所有的元素都被转换成数字。

如果参数是类型化数组，则每个元素都被转换为适合类型化数组的类型。

`subarray`返回一个新的类型化数组，该数组与调用它的类型化数组具有相同的缓冲区。

它将起始索引作为第一个参数。该值的默认值为 0。

end 索引是它的第二个参数。这个的默认值是数组的`length`。

这些值可以是负数。

# 数组方法

类型化数组可以使用`copyWithin`、`entries`、`every`、`fill`、`find`、`findIndex`、`forEach`、`indexOf`、`join`、`keys`、`lastIndexOf`、`length`、`map`、`reduce`、`reduceRight`、`reverse`、`slice`、`some`、`sort`、`toLocaleString`、`toString`、`values`等数组方法。

它们与常规数组方法采用相同的参数并具有相同的返回值。

# 构造器

类型化数组构造函数包括`Int8Array`、`Uint8Array`、`Uint8ClampedArray`、`Int16Array`、`Uint16Array`、`Int32Array`、`Uint32Array`、`Float32Array`、`Float64Array`。

他们可以有签名`(buffer, byteOffset=0, length?)`、`(length)`、`(typedArray)`、`(arrayLikeObject)`或`()`。

`buffer`是类型化数组的缓冲区。

`ByteOffset`是从索引 0 开始的偏移量。

`length`是类型化数组的长度。

`typedArray`是另一个类型化数组。

`arrayLikeObject`是类数组或可迭代对象。

类似数组的对象是具有`length`属性和整数键的对象。

![](img/633954b2c41c48a949f2dfaf6d9b2767.png)

Photo by [Pierre Bamin](https://unsplash.com/@bamin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

类型化数组有许多数组方法。

该构造函数不同于常规的`Array`构造函数。

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**