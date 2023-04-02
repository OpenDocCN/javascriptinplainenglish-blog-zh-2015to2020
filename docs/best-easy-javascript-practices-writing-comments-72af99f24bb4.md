# 最佳简易 JavaScript 实践——编写注释

> 原文：<https://javascript.plainenglish.io/best-easy-javascript-practices-writing-comments-72af99f24bb4?source=collection_archive---------3----------------------->

## 通过注释使您的 JavaScript 代码更具可读性

![](img/0d80dd237d059863b2b147db8321da73.png)

Photo by [Safar Safarov](https://unsplash.com/@codestorm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在源代码中添加注释，纯属个人选择。一些工程师认为代码中的注释有助于他们和其他人轻松理解他们的代码。他们中的一些人认为代码应该以不言自明的方式编写。无论是哪种情况，我们都希望我们的代码可读性强，易于理解。

如果你选择在你的代码中添加注释，你最好以这样一种方式去做，让它成为一种祝福而不是一场噩梦。

下面是一些帮助你在代码中写更好的注释的实践。

1.  使用`//`进行单行注释。

```
// bad
/* Base Url */
const baseUrl = 'http://127.0.0.1';// good
// Base Url
const baseUrl = 'http://127.0.0.1';
```

2.注释应该放在新的一行，而不是附加在代码语句之后。

在注释的开始给出一个空格，也可以提高可读性。

```
// bad
const baseUrl = 'http://127.0.0.1'; //Webapp Base Url// good
// Webapp Base Url
const baseUrl = 'http://127.0.0.1';
```

3.尽量在代码中使用简短而精确的注释。

```
// bad
// Base url to be used by the API
const baseUrl = 'http://127.0.0.1';// good
// API Base Url
const baseUrl = 'http://127.0.0.1';
```

4.使用`/* */`进行多行注释，而不是多个单行注释。长的单行注释也可以被格式化为多行注释。

```
// bad
// check move validation
// based on boundary conditions
function makeMove(move){
 // ...
}// good
/*
 check move validation
 based on boundary conditions
/*
function makeMove(move){
 // ...
}// best - using JSDoc style
/** 
 * check move validation
 * based on boundary conditions
 /*
function makeMove(move){
 // ...
}
```

5.在注释中使用注释，如`TODO`和`FIXME`。

如果你想表示一项尚未完成的任务，可以用`TODO`。这将有助于您在未来轻松找到您的未决任务。

```
function fetchData(){
 // TODO: Use environment variable instead 
 const url = 'http://127.0.0.1:8000/newsfeed';
 // ..
}
```

使用`FIXME`来指出您的代码中需要解决的可能问题或 bug。顾名思义，这个注释将帮助您快速找出哪里需要修复。

```
// FIXME: Find a better solution
function longTimeTakingOperation(){
 // ...
}
```

此外，注释也有助于借助 [JSDoc](https://devdocs.io/jsdoc/about-getting-started#adding-documentation-comments-to-your-code) 快速制作 API 文档。添加像`param`这样的特殊`JSDoc`标签来提供额外的信息。JSDoc 将扫描你的源代码并生成 HTML 文档。

```
/**
 * Fetches newsfeed for user
 * @param {string} userId - The userId of user.
 */
function Newsfeed(userId) {
  // ...
}
```

使用上述实践在代码中编写注释不仅会提高代码的可读性，还会帮助您和他人更容易理解您的代码。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**