# ES6 —存储键值对的对象与映射

> 原文：<https://javascript.plainenglish.io/es6-object-vs-map-for-storing-key-value-pairs-9b06a7efece9?source=collection_archive---------2----------------------->

![](img/bbbab6fc99628470add15052bb51d73c.png)

*编辑 4 月 21 日:添加时间复杂度基准*

出于各种原因，我们经常需要存储键值对。我发现大多数时候人们(包括我自己)使用`Object`数据结构来实现这一点，尽管 ES6 提供了一个`Map`数据结构，看起来确实像我们应该使用的。

让我们试着看看这两者之间有什么不同，好吗？

披露:我将调用`key`的`Map`键(这是正确的)和`Object`属性，因为我们在本文中讨论存储键值对。

# 基础

当涉及到像添加或获取一个值这样的基本操作时，使用一个或另一个没有太大的区别。

## 目标

```
// init
const keyValue = {}// add
keyValue['firstname'] = 'tony'// get
keyValue['firstname']
```

## 地图

```
// init
const keyValue = new Map()// add
keyValue.set('firstname', 'tony')// get
keyValue.get('firstname')
```

有趣的是:当试图从一个不存在的键中获取一个值时，两者都返回`undefined`

```
const o = {}
o['firstname'] // undefinedconst m = new Map()
m.get('firstanme') // undefined
```

# 重复

存储和获取值是很好的，但是我们通常还需要遍历集合。

使用`Map`非常简单，因为它是可迭代的。这意味着您可以直接使用内置的`forEach`函数:

```
const kv = new Map()
kv.set('firstname', 'tony')
kv.set('lastname', 'stark')kv.forEach((value, key) => console.log(key, value))// firstname tony
// lastname stark
```

这在`Object`中是不可能的。尽管有很多方法可以解决这个问题。其中两个是:

```
const kv = {}
kv['firstname'] = 'tony'
kv['lastname'] = 'stark'Object.keys(kv).forEach((key) => console.log(key, kv[key]))// firstname tony
// lastname stark
```

和

```
Object.entries(kv).forEach((entry) => console.log(entry[0], entry[1]))// firstname tony
// lastname stark
```

使用`Map`绝对更容易，更简洁，可读性更强

# 检查是否有钥匙

在`Map`中检查一个键非常简单:

```
const kv = new Map()
kv.set('firstname', 'tony')
kv.set('lastname', 'stark')kv.has('firstname') // true
kv.has('nickname') // false
```

使用`Object`就没那么简单了:

```
const kv = {}
kv['firstname'] = 'tony'
kv['lastname'] = 'stark'Object.prototype.hasOwnProperty.call(kv, 'firstname') // true
Object.prototype.hasOwnProperty.call(kv, 'nickname') // false
```

关于检查对象是否有属性的更多细节[在这里](https://medium.com/javascript-in-plain-english/the-right-way-to-check-if-an-object-has-a-property-in-javascript-10da3160a1e4)。

这里`Map`似乎更方便

# 数量/长度/尺寸

猜猜看，由于`Map`有一个内置属性，什么更简单

```
const kv = new Map()
kv.set('firstname', 'tony')
kv.set('lastname', 'stark')kv.size // 2
```

使用`Object`你必须创建一个 iterable，然后获得它的长度

```
const kv = {}
kv['firstname'] = 'tony'
kv['lastname'] = 'stark'Object.keys(kv).length // 2
```

# 时间复杂度

哪个表现更好？我根据[这篇](https://stackoverflow.com/questions/31091772/javascript-es6-computational-time-complexity-of-collections/54385459) StackOverflow 帖子写了一小段代码，然后用`size = 1000`运行它:

```
const benchmarkMap = size => {
 console.time('Map insert')
  const kv = new Map()
  for (let i = 0; i < size; i++) {
   kv.set(i, i)
  }
  console.timeEnd('Map insert')
 console.time('Map retrieve')
  for (let i = 0; i < size; i++) {
   kv.get(i)
  }
  console.timeEnd('Map retrieve')
}const benchmarkObject = size => {
 console.time('Object insert')
  const kv = {}
  for (let i = 0; i < size; i++) {
   kv[i] = i;
  }
  console.timeEnd('Object insert')
  console.time('Object retrieve')
  for (let i = 0; i < size; i++) {
   kv[i]
  }
  console.timeEnd('Object retrieve')
}
```

这是我得到的信息:

```
Map insert: 0.099853515625ms
Map retrieve: 0.0751953125ms
Map insert: 0.095947265625ms
Map retrieve: 0.093994140625ms
Map insert: 0.076904296875ms
Map retrieve: 0.06005859375ms
Map insert: 0.0869140625ms
Map retrieve: 0.05908203125ms
Map insert: 0.078125ms
Map retrieve: 0.057861328125msObject insert: 0.041015625ms
Object retrieve: 0.028076171875ms
Object insert: 0.035888671875ms
Object retrieve: 0.02783203125ms
Object insert: 0.038818359375ms
Object retrieve: 0.029052734375ms
Object insert: 0.035888671875ms
Object retrieve: 0.031005859375ms
Object insert: 0.035888671875ms
Object retrieve: 0.029052734375ms
```

使用`Object`看起来更快，但是请注意，我们在这里讨论的是毫秒级，处理 1000 个键值对。

# 有趣的事情要知道

*   `Object`只能将`string`存储为关键字，而它可能是`Map`的任何内容。作为一个例子，下面是完全可以的

```
const m = new Map()
m.set(['a', 'b'], true)
m.set(['a', 'c'], false)
```

即使你指定一个数字作为键，它也会被当作一个`string`

```
const o = {}
o[1] = 'iron man'
o[1] // 'iron man'
o['1'] // 'iron man'
```

*   `Object`已经有了原型这意味着已经有了一些键集

```
const o = {}
o['constructor'] // ƒ Object() { [native code] }const m = new Map()
m.get('constructor') // undefined
```

其中`Map`没有

*   `JSON`支持`Object`但不支持`Map`

```
const kv = {}
kv['firstname'] = 'tony'
kv['lastname'] = 'stark'JSON.stringify(kv) // '{"firstname":"tony","lastname":"stark"}'
```

在哪里

```
const kv = new Map()
kv.set('firstname', 'tony')
kv.set('lastname', 'stark')JSON.stringify(kv) // '{}'
```

因此，如果您需要序列化数据，通过 HTTP 调用发送数据，那么`Map`不是一个好的选择

# 选哪个？

在我看来`Map`应该是你存储键值对的好方法，除非你需要序列化数据。你不仅可以使用`string`作为键，事实上它是在`has()`和`size`之上的一个 iterable，这将使你的生活变得轻松。

当你想表示某事时，应该使用。因此，您可能会对每个属性使用不同的逻辑。

```
const ironman = {
  firstname: 'tony',
  lastname: 'stark',
  hello: function() {
    return `I am ${this.firstname} ${this.lastname}`
  }
}
```