# Object.freeze vs Object.seal —不变性

> 原文：<https://javascript.plainenglish.io/object-freeze-vs-object-seal-immutability-7c22f80aa8ae?source=collection_archive---------5----------------------->

## Java Script 语言

## 两个本地方法与不变性相关

![](img/8e3e8f78f7bffe8e066d8f97be54e4ed.png)

Photo by [Christine Donaldson](https://unsplash.com/@christineashleydonaldson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

数据不变性在编程语言中一直非常重要，在 JavaScript 中也是如此。这里，有两个 JavaScript 方法可以部分保证不变性——object . freeze 和 Object.seal。

# 对象定义属性

在了解每一个`freeze`和`seal`之前，我们需要先知道 Object 中的`defineProperty`方法是什么。当您或引擎在初始处理期间创建一个对象时，JavaScript 会为新创建的对象提供基本属性，以处理来自外部的请求，例如访问或删除属性。

您可以修改或设置的属性如下。

*   值—属性的值
*   可枚举—如果为真，则允许属性可通过`for-in`循环或`Object.keys()`进行搜索。默认为`false`。
*   可写-如果为 false，则不能修改该属性。它在严格模式下抛出一个错误。默认为`false`。
*   可配置—如果为 false，这将使对象的属性不可枚举、不可写、不可删除和不可配置。默认为`false`。
*   get —当您尝试访问属性时提前调用的函数。默认为`undefined`。
*   set —当您尝试为属性设置某个值时提前调用的函数。默认为`undefined`。

我知道你会想我在说什么。所以我们来看一些简单的例子。

## 可列举的

```
const obj = {};Object.defineProperty(obj, 'a', {
  value: 100,
 **enumerable: false**
});for (const key in obj) {
  console.log(key);
}
// undefinedObject.keys(obj);
// []
```

## 可写的

```
const obj = {};Object.defineProperty(obj, 'a', {
  value: 100,
 **writable: false**
});obj.a = 200;
obj.a === 100; // true(() => {
  'use strict';
  obj.a = 100; 
  // TypeError in the strict mode
})()
```

## 可配置的

```
const obj = {};Object.defineProperty(obj, 'a', {
  value: 100,
 **configurable: false**
});// **1\. non-enumerable**
for (const key in obj) {
  console.dir(key);
}
// undefinedObject.keys(obj);
// [// **2\. non-writable**
(() => {
  'use strict';
  obj.a = 200;
  // TypeError in the strict mode
})()// 3\. **non-deletable**
delete obj.a;
obj.a === 100; // true
```

但是当`writable`或`enumerable`为`true`时，`configurable: false`被忽略。

# 对象。海豹

![](img/c7f1b209d76f327f6e392ca8e252b737.png)

Photo by [戸山 神奈](https://unsplash.com/@samuelsparkle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

听到“海豹”这个词，你会想到什么？“印章”的第一个意思是封闭信件或其他东西的邮票或蜡。在 JavaScript 中，`Object.seal`也做和“海豹”一样的事情。

`Object.seal`使传递给它的对象的所有属性不可配置。我们举个例子。

```
const obj = { a: 100 };
Object.getOwnPropertyDescriptors(obj);/* { 
 *   a: { 
 *        configurable: true,
 *        enumerable: true,
 *        value: 100,
 *        writable: true
 *      }
 * }
 * 
 */Object.seal(obj);
Object.getOwnPropertyDescriptors(obj);/* { 
 *   a: { 
 *        **configurable: false,**
 *        enumerable: true,
 *        value: 100,
 *        writable: true
 *      }
 * }
 * 
 */obj.a = 200;
console.log(obj.a);
// 200delete obj.a;
console.log(obj.a);
// 200obj.b = 500;
console.log(obj.b);
// undefined
```

因此，对象`obj`有一个属性值为 100 的属性。而`obj`的初始描述符如上，`configurable`、`enumerable`、`writable`都是`true`。然后我用`Object.seal`密封对象，看看哪些描述符被修改了，哪些没有——只有`configurable`被修改为`false`。

```
obj.a = 200;
```

即使密封对象的`configurable`现在为 false，现有的属性值也更改为 200。正如我前面解释过的，将`configurable`设置为`false`会使属性不可写，但是如果`writable`显式为`true`则不起作用。当你创建一个对象并设置一个新的属性时，默认情况下它有`writable: true`。

```
delete obj.a;
```

密封对象使每个属性都不可配置，从而防止它们不可删除。

```
obj.b = 500;
```

这在密封对象后失败了，但是为什么呢？当调用`Object.seal`或`Object.freeze`时，传递给它们的对象变成一个不可扩展的对象，这意味着你不能从中删除任何属性或向其中添加任何属性。

# 对象.冻结

这比`Object.seal`更多地限制了传递的对象。我们也举一个类似的例子。

```
const obj = { a: 100 };
Object.getOwnPropertyDescriptors(obj);/* { 
 *   a: { 
 *        configurable: true,
 *        enumerable: true,
 *        value: 100,
 *        writable: true
 *      }
 * }
 * 
 */Object.freeze(obj);
Object.getOwnPropertyDescriptors(obj);/* { 
 *   a: { 
 *        **configurable: false,**
 *        enumerable: true,
 *        value: 100,
 *        **writable: false**
 *      }
 * }
 * 
 */obj.a = 200;
console.log(obj.a);
// 100delete obj.a;
console.log(obj.a);
// 200obj.b = 500;
console.log(obj.b);
// undefined
```

所以和`Object.seal`的区别在于`writable`也被设置为`false`，冻结后。

```
obj.a = 200;
```

因此，修改现有属性总是会失败。

```
delete obj.a;
```

和`Object.seal`一样，`Object.freeze`也使得传递的对象不可配置，这使得其中的每个属性都不可删除。

```
obj.b = 500;
```

冻结对象还会使对象不可扩展。

# 他们有什么共同点？

1.  传递的对象变得不可扩展，这意味着您不能添加新的属性。
2.  传递的对象中的每个元素都变得不可配置，这意味着您不能删除它们。
3.  如果在“使用严格”模式下调用操作，例如在严格模式下调用`obj.a = 500`，两种方法都可能抛出错误。

# 比较

`Object.seal`允许您修改属性，而`Object.freeze`不能。

# 裂缝

`Object.freeze`和`Object.seal`在“实用”方面都有“瑕疵”，它们只冻结/封印了物体的第一个深度。

下面是快速对比。

```
const obj = {
  foo: {
    bar: 10
  }
};
```

现在`obj`里面有了嵌套的对象，`foo`。它里面有`bar`。

```
Object.getOwnPropertyDescriptors(obj);
/* { 
 *   foo: { 
 *        configurable: true,
 *        enumerable: true,
 *        value: {bar: 10},
 *        writable: true
 *      }
 * }
 */Object.getOwnPropertyDescriptors(obj.foo);
/* { 
 *   bar: { 
 *        configurable: true,
 *        enumerable: true,
 *        value: 10,
 *        writable: true
 *      }
 * }
 */
```

密封或冷冻后，

```
Object.seal(obj);
Object.freeze(obj);// Either way is fine for this test
```

让我们看看描述符是如何改变的。

```
Object.getOwnPropertyDescriptors(obj);
/* { 
 *   foo: { 
 *        **configurable: false**,
 *        enumerable: true,
 *        value: {bar: 10},
 *        **writable: false**
 *      }
 * }
 */Object.getOwnPropertyDescriptors(obj.foo);
/* { 
 *   bar: { 
 *        configurable: true,
 *        enumerable: true,
 *        value: 10,
 *        writable: true
 *      }
 * }
 */
```

`foo`的描述符被更改，但`obj.foo`的嵌套对象被更改。这意味着它仍然是可修改的。

```
obj.foo = { bar: 50 };
// Doesn't work
```

因为`obj.foo`不允许你改变它的值，因为它被冻结了，

```
obj.foo.bar = 50;
// It works
```

`obj.foo.bar`仍然允许你改变它的值，因为它没有被冻结。

那么如何将对象冻结/密封到最底层的嵌套对象呢？MDN 建议您使用[解决方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze#What_is_shallow_freeze)来解决这个问题。

```
function deepFreeze(object) { // Retrieve the property names defined on object
  var propNames = Object.getOwnPropertyNames(object); // Freeze properties before freezing self

  for (let name of propNames) {
    let value = object[name]; if(value && typeof value === "object") { 
      deepFreeze(value);
    }
  } return Object.freeze(object);
}
```

测试结果如下。

```
const obj = { foo: { bar: 10 } };
deepFreeze(obj);obj.foo = { bar: 50 };
// Doesn't workobj.foo.bar = 50;
// Doesn't work
```

# 结论

`Object.freeze`和`Object.seal`绝对是有用的方法。但是你应该考虑到嵌套的对象也应该被冻结，使用`deepFreeze`。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**

# 资源

*   [object . defined property—MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
*   [Object.freeze — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
*   [Object.seal — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)
*   [object . getownpropertysdescriptors—MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)
*   [对象的可配置和可写属性之间的差异](https://stackoverflow.com/questions/23590502/difference-between-configurable-and-writable-attributes-of-an-object)