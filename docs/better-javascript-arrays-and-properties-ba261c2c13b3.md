# 更好的 JavaScript——数组和属性

> 原文：<https://javascript.plainenglish.io/better-javascript-arrays-and-properties-ba261c2c13b3?source=collection_archive---------12----------------------->

![](img/c40e827d8eabbc995f296b05f0178d76.png)

Photo by [Vivint Solar](https://unsplash.com/@vivintsolar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 安全调用 hasOwnProperty

`hasOwnProperty`是`Object.prototype`的一部分，这意味着它可以被覆盖或删除。

因此，为了确保它始终可用，我们可以直接从`Object.prototype`调用它。

我们可以写:

```
const hasOwnProperty = Object.prototype.hasOwnProperty;
```

或者:

```
const hasOwnProperty = {}.hasOwnProperty;
```

那么我们可以这样称呼它:

```
hasOwnProperty.call(dict, 'james');
```

鉴于我们有:

```
const dict = {
  james: 33,
  bob: 22,
  mary: 41
};
```

我们可以抽象出检查对象入口的逻辑。

例如，我们可以用项目创建一个类。

我们可以写:

```
class Dict {
  constructor(elements) {
    this.elements = elements;
  } has(key) {
    return {}.hasOwnProperty.call(this.elements, key);
  } set(key, val) {
    this.elements[key] = val;
  } get(key) {
    return this.elements[key];
  } remove(key) {
    delete this.elements[key];
  }
}
```

我们创建了一个`Dict`类来保存一个`elements`实例变量，我们可以用一些方法来操作这个变量，以完成常见的字典操作。

`has`检查该属性是否存在。

`set`用值设置键。

`get`用给定的键获取值。

`remove`从对象中删除一个条目。

现在我们可以通过书写来使用它:

```
const dict = new Dict({
  james: 33,
  bob: 22,
  mary: 41
});
```

# 对有序集合使用数组

数组应该用于有序集合。

有序集合有一个索引，它们按照给定的顺序被迭代。

我们可以通过编写以下内容来创建数组:

```
const arr = [1, 2, 3];
```

然后我们可以通过写来使用它:

```
for (const a of arr) {
  console.log(a);
}
```

我们用 for-of 循环遍历了`arr`数组。

它总是从开始迭代到结束，所以顺序是可预测的。

for-of 循环不应该被误认为是 for-in 循环，for-in 循环以不可预知的顺序遍历所有项。

# 切勿向 Object.prototype 添加可枚举属性

我们不应该给`Object.prototype`添加可枚举属性。

`Object.prototype`是一个我们不拥有的属性，所以我们不应该改变它，因为它会给出大多数人不期望的结果。

此外，for-in 循环将获取原型的可枚举属性，因此它将被迭代。

我们不希望这种情况发生。

如果我们想给`Object.prototype`添加一个属性，那么它应该是不可枚举的。

例如，我们可以写:

```
Object.defineProperty(Object.prototype, "allKeys", {
  value() {
    const result = [];
    for (const key in this) {
      result.push(key);
    }
    return result;
  },
  writable: true,
  enumerable: false,
  configurable: true
});
```

我们将`enumerable`设置为`false`，这样它就不会被 for-in 循环拾取。

![](img/df2e10433becdec3b3a3c572732d817d.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该给`Object.prototype`添加可枚举属性。

同样，为了安全地调用`hasOwnProperty`，我们不应该直接从对象本身调用它，因为它可以被修改。

数组适用于有序集合。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**