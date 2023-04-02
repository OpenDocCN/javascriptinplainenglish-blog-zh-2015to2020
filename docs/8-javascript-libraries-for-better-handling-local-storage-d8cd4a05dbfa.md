# 8 个 JavaScript 库，用于更好地处理本地存储

> 原文：<https://javascript.plainenglish.io/8-javascript-libraries-for-better-handling-local-storage-d8cd4a05dbfa?source=collection_archive---------1----------------------->

## 有时仅仅 HTML 5 本地存储是不够的。

![](img/71e28ce1b7e43f17d237f639011aaab5.png)

Photo by [frank mckenna](https://unsplash.com/@frankiefoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我为我当前的项目测试了一些本地存储库。想知道它们都有哪些很酷的功能？继续读。

# 本地存储桥

[https://github.com/krasimir/lsbridge](https://github.com/krasimir/lsbridge)

如果你必须在同一个浏览器中从一个标签页向另一个标签页发送消息，你不必这么辛苦。本地存储桥可以使任务变得更容易。

基本用法:

```
// Sending
lsbridge.send(‘app.message.error’, { error: ‘Out of memory’ });// Listening
lsbridge.subscribe(‘app.message.error’, function(data) {
  console.log(data); // { error: ‘Out of memory’ }
});
```

# 罗勒

http://wisembly.github.io/basil.js/

Basil.js 统一了会话、本地存储和 cookie，为您提供了一种处理数据的简单方法。

基本用法:

```
let basil = new Basil(options);basil.set(‘name’, ‘Amy’);
basil.get(‘name’);
basil.remove(‘name’);
basil.reset();
```

# 商店. js

【https://github.com/marcuswestin/store.js 

Store.js 像其他任何东西一样处理数据存储。但是还有更多。其中一个高级功能是，它可以让您更深入地访问浏览器支持。

基本用法:

```
store.set(‘book’, { title: ‘JavaScript’ }); // Store a book
store.get(‘book’); // Get stored book
store.remove(‘book’); // Remove stored book
store.clearAll(); // Clear all keys
```

# lscache

[https://github.com/pamelafox/lscache](https://github.com/pamelafox/lscache)

它类似于 localStorage API。事实上，它是 localStorage 的包装器，并使用 HTML5 模拟 memcaches 函数。在上面的文档中了解更多功能。

基本用法:

```
lscache.set(‘name’, ‘Amy’, 5); // Data will be expired after 5 minutes
lscache.get(‘name’);
```

# 洛克尔

[https://github.com/tsironis/lockr](https://github.com/tsironis/lockr)

Lockr 构建在 localStorage API 之上。它为您提供了一些更容易处理本地数据的有用方法。

是什么让你想使用这个库而不是 localStorage API？

好吧，localStorage API 只允许你存储字符串。如果你想存储一个数字，你需要先把这个数字转换成字符串。这在 Lockr 中不会发生，因为 Lockr 允许您存储更多的数据类型甚至对象。

基本用法:

```
Lockr.set(‘name’, ‘Amy’);
Lockr.set(‘age’, 28);
Lockr.set(‘books’, [{title: ‘JavaScript’, price: 11.0}, {title: ‘Python’, price: 9.0}]);
```

# 车库

[https://github.com/arokor/barn](https://github.com/arokor/barn)

Barn 在 localStorage 之上提供了一个类似 Redis 的 API。如果持久性很重要，那么您将需要这个库来保存数据状态，以防出现错误。

基本用法:

```
let barn = new Barn(localStorage);// Primitive types
barn.set(‘name’, ‘Amy’);
let name = barn.get(‘name’); // Amy// List
barn.lpush(‘names’, ‘Amy’);
barn.lpush(‘names’, ‘James’);
let name1 = barn.rpop(‘names’); // Amy
let name2 = barn.rpop(‘names’); // James
```

# 本地饲料

[https://github.com/localForage/localForage](https://github.com/localForage/localForage)

这个简单快速的库将通过 IndexedDB 或 WebSQL 使用异步存储来改善您的 web 的离线体验。它类似于 localStorage，但是有一个额外的特性是回调。

基本用法:

```
localforage.setItem(‘name’, ‘Amy’, function(error, value) {
  // Do something
});localforage.getItem(‘name’, function(error, value) {
  if (error) {
    console.log(‘an error occurs’);
  } else {
    // Do something with the value
  }
});
```

# crypt.io

[https://github.com/jas-/crypt.io](https://github.com/jas-/crypt.io)

crypt.io 通过[Stanford JavaScript 加密库](http://bitwiseshiftleft.github.io/sjcl/)实现安全浏览器存储。使用 crypto.io 时，有三个存储选项:sessionStorage、localStorage 或 cookies。

基本用法:

```
let storage = crypto;
let book = { title: ‘JavaScript’, price: 13 };storage.set(‘book’, book, function(error, results) {
  if (error) {
    throw error;
  }

  // Do something
});storage.get(‘book’, function(error, results) {
  if (error) {
    throw error;
  } // Do something
});
```

你知道其他地方的图书馆吗？你为什么用它？请在下面的评论中告诉我

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/8-places-to-find-helpful-javascript-snippets-3733b8a62768) [## 找到有用的 JavaScript 代码片段的 8 个地方

### 为自己节省大量时间。

medium.com](https://medium.com/javascript-in-plain-english/8-places-to-find-helpful-javascript-snippets-3733b8a62768) 

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**