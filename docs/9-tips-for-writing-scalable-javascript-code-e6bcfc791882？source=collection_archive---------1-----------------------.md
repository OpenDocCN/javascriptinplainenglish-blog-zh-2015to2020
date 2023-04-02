# 编写可伸缩 JavaScript 代码的 9 个技巧

> 原文：<https://javascript.plainenglish.io/9-tips-for-writing-scalable-javascript-code-e6bcfc791882?source=collection_archive---------1----------------------->

## 您应该从一开始就准备好扩展您的项目。

![](img/02bd50108840b81689aef2e78d3a0bf0.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在某个时候，你的项目会增长。我有一个好消息和一个坏消息告诉你。

坏消息是你的代码库可能会失去控制，所以你最终会把它扔进篮子里，然后从头开始。你希望这次你能做得更好。除非你保持同样的心态。

幸运的是，好消息是你可以通过使用我下面要分享的技巧来编写可伸缩的代码来避免坏消息。继续读。

# 1.使用设计模式

如果你想以可重用的方式解决编码问题，你需要在设计模式上花时间。

设计模式就像是解决编码中常见问题的模板。这是一个行之有效的解决方案，你不需要浪费时间去重新发明轮子。

试着掌握它，你会发现作为一名开发人员，你的生活是多么容易。

[](https://medium.com/javascript-in-plain-english/7-javascript-design-patterns-every-developer-should-know-df9c40e7debf) [## 每个开发人员都应该知道的 7 个 JavaScript 设计模式

### 如果你能把你的源代码组织成一个漂亮的模板，可以应用到相同的每个项目中，那会怎么样

medium.com](https://medium.com/javascript-in-plain-english/7-javascript-design-patterns-every-developer-should-know-df9c40e7debf) 

# 2.分离您的代码

不要在一个地方混合所有类型的代码。例如，您应该将它们分为逻辑、视图和数据。你可以使用一些类似 MVC 的架构来装饰你的代码库。分开的代码总是比乱七八糟的更可控。

当你写函数的时候，同样的心态也应该适用。长话短说，不要把一堆东西放到一个函数里。永远记住一个功能一个使命。

# 3.构建模块

这类似于分离你的代码，但是在不同的层次上。

模块是将相关代码分组的地方。例如，所有旨在处理授权的功能都应该放在一个单独的模块中。我们称之为授权模块。

```
function validate(username, password) {
  // validate data
}function login(username, password) {
  // login
}function logout() {
  // logout
}module.exports = {
  validate,
  login,
  logout
}
```

当您需要在其他地方使用它们时:

```
const { validate, login, logout } = require(‘./AuthorizationModule’)validate(‘amy’, 123);
login(‘amy’, 123);
logout();
```

这样你就可以保持你的代码有条理。

# 4.保持你的文件小，甚至很小

我喜欢小文件。当我在这些文件中查找内容时，可以更容易、更快地找到我需要的内容。

在大文件中，任务变得复杂。我有时会处理那些大文件。它们有一万行代码，你能想象吗？这里面有一堆逻辑。这是一种痛苦。

我倾向于把我的文件保持在 500 行左右。这是我的规则。如果它要增长超过那个阈值，我就把它分解成新的文件。用模块组织相关逻辑就是其中一种方式。

如果你能写这些小文件并保持它们的良好结构，你会为自己感到骄傲。

# 5.相比回调，更倾向于承诺和异步/等待

技术上来说，回调是有帮助的。然而，当它在多个嵌套层次(也称为回调地狱)中增长时，会给你带来很多麻烦。而且，很丑。为了可读性，您应该尽可能避免使用它。

请改用 async/await。

嵌套回调将打破自然的思维流程，而 async/await 仍然是流程。简单地说，async/await 让我们能够编写看起来同步的异步代码。这就是 async/await 如何让代码更漂亮、可读性更好。

使用回调示例:

```
function getBook(bookId, callback) {
  setTimeout(() => {
    let book = {
      title: ‘JavaScript’,
      authorId: ‘123’
    }

    callback(book);
  }, 500);
}function getAuthor(authorId, callback) {
  setTimeout(() => {
    let author = {
      name: ‘Amy’,
      age: 28
    } callback(author);
  }, 500);
}function fetchData() {
  // This will reach the hell level if having more callbacks. I keep it simple for example

  getBook(2020, book => {
    getAuthor(book.authorId, author => {
      console.log(author);
    })
  });
}fetchData(); // { name: ‘Amy’, age: 28 }
```

现在，我们将删除回调:

```
function getBook(bookId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      let book = {
        title: ‘JavaScript’,
        authorId: ‘123’
      }

      resolve(book);
    }, 500);
  });
}function getAuthor(authorId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      let author = {
        name: ‘Amy’,
        age: 28
      } resolve(author);
    }, 500);
  });
}async function fetchData() {
  let book = await getBook(2020);
  let author = await getAuthor(book.authorId); console.log(author)
}fetchData(); // { name: ‘Amy’, age: 28 }
```

现在，看看函数 **fetchData** 有多令人满意。

# 6.解构

ES6 配备了许多令人惊叹的功能。解构就是其中之一。

该特性使您能够从对象中提取特定的字段，并根据需要将它们分配给变量。

示例:

```
let person = {
  name: ‘Amy’,
  age: 28,
  job: ‘Entrepreneur’
};let { name, age, job } = person;
console.log(name, age, job); // Amy 28 Entrepreneur
```

当你使用析构时，你的代码可读性更好。你不必总是写一些像**这样的东西。相反，你写一个简单的变量**名**。没有更多嵌套属性。你的同事会清楚地知道你在使用什么变量。**

[](https://medium.com/better-programming/5-ways-to-make-the-most-of-destructuring-in-javascript-to-write-cleaner-code-d674c00da9c7) [## 充分利用 JavaScript 中析构的 5 种方法来编写更简洁的代码

### 析构是 ES6 最激动人心的特性之一

medium.com](https://medium.com/better-programming/5-ways-to-make-the-most-of-destructuring-in-javascript-to-write-cleaner-code-d674c00da9c7) 

# 7.初始化默认值

当缺省值伴随着函数参数的析构时，这是一笔大买卖。

在 JavaScript 中，很难看到一个参数就知道可以传递什么类型的数据。默认值会有所帮助，因为它们给出了可以传递给函数的数据示例。

我们经常检查一个论点是否被定义。如果您为它初始化了一个默认值，您就不需要再做那个任务了。

# 8.使用更漂亮和 ESLint 来修饰你的代码

团队的每个成员都有自己的编码风格。为了保持项目的一致性并易于扩展，您和您的同事必须遵循一种风格。

这就是埃斯林特和漂亮地跳进游戏的时候。

ESLint 附带了限制编码风格的默认规则。然而，您也可以根据自己的需要编辑或定义您的规则。

更漂亮地自动修饰你的代码，使之一致。您只需专注于编写高质量的代码，将格式化部分留给更漂亮的代码。

当事情可以用工具自动完成时，就去做吧。把你有意义的时间花在更重要的任务上。

[](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) [## Visual Studio 代码的 9 大 JavaScript 扩展加速您的开发

### 谁想更快更容易地编码？

medium.com](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) 

# 9.定义多个参数，而不是一个对象参数

说到编写函数，有两种类型的开发人员。

第一个:

```
function showProduct(name, price, description) {
  console.log(name, price, description);
}
```

第二个:

```
function showProduct(product) {
  console.log(product.name, product.price, product.description);
}
```

你是什么类型的？

乍一看，第二个似乎很方便，因为我们不需要写一长串参数。

但事情是这样的:

第一个很简单。无需猜测，您就能准确地知道向函数传递什么。

如果您将参数作为单个对象传递，有时您不知道对象内部的必要属性是否可用。

然而，这一规则并不总是合适的。如果你定义了一个超过四个参数的函数，那么使用对象参数。运用你的常识。

# 结论

编写可伸缩代码的话题永远不会过时。我们写的代码越多，我们的编码技能就提高得越多。

到目前为止，以上是我每天使用的保持代码库可伸缩性的技巧。希望你觉得有用。

你有自己的建议吗？如果你能在下面的评论中分享它们，那就太好了。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/how-to-write-effective-javascript-code-that-every-programmer-loves-to-maintain-a490b42b9b9f) [## 如何编写每个程序员都喜欢维护的有效 JavaScript 代码

### 要不要写更易维护的代码？运用坚实的原则

medium.com](https://medium.com/javascript-in-plain-english/how-to-write-effective-javascript-code-that-every-programmer-loves-to-maintain-a490b42b9b9f)