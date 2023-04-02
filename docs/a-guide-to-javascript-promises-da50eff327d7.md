# JavaScript 承诺指南

> 原文：<https://javascript.plainenglish.io/a-guide-to-javascript-promises-da50eff327d7?source=collection_archive---------0----------------------->

## JavaScript 中所有异步事物的短暂游览:从回调地狱到异步/等待

![](img/52bcac33109236c1d2f9d9d4fe44fb26.png)

Image Credit: [Marcos Mayer / Unsplash](https://unsplash.com/photos/8_NI1WTqCGY)

我对 web 开发了解得越多，就越开始意识到异步代码的重要性。一旦你超越了静态网站，异步代码就变得不可或缺。每个全栈应用都依赖于通过 API 发送、接收和处理数据。

但是编写异步代码和编写常规的同步 JavaScript 感觉非常不同。同步代码允许你做更多的事情。例如，同步代码中的操作顺序更宽松。结构混乱的代码仍然可以成功执行——即使这会让你的同事不高兴！

相比之下，异步代码的结构和顺序必须严格得多，在本文中，我们将看到如何做到这一点。我们将看看编写异步代码的三个主要系统，我还将分享一些使同步函数异步的方法。

# 回调函数，尝试和捕捉

在 JavaScript 的早期，连续进行多个异步操作会导致所谓的末日金字塔，如下图所示。

```
func1(function(result) {
  func2(result, function(newResult) {
    func3(newResult, function(finalResult) {
      func4(newResult, function(finalResult) {
        console.log(finalResult);
      }, failureCallback);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);
```

这种情况也被称为回调地狱。随着异步操作数量的增加，跟踪正在发生的事情很快变得非常困难。

对于更简单的情况，指定失败回调也可以使用`try`和`catch`语句来处理，这在 JavaScript 早期就已经存在了。

```
try {
  asyncFunction();
} 
catch (err) {
  console.error(err);
}
```

但是，对多个异步动作的需求会很快导致更糟糕的混乱！

```
try {
  func1();
  try {
    func2();
    try {
      func3();
    } catch {
      failureFunc1();
    }
  } catch {
    failureFunc2();
  }
} catch {
  failureFunc3();
}
```

# 承诺，然后抓住

ES6 中出现了一个重大转变，引入了一个新对象:承诺。`Promise`对象表示异步操作的完成或失败以及该操作的结果值。

可以使用`new Promise()`构造函数创建一个`Promise`。这需要一个带有两个参数的函数，如下面的例子所示:

```
const foo = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('bar');
  }, 3000);
});
```

如果我们试图在`Promise`被解决或拒绝之前调用`console.log(foo)`，我们将简单地看到`Promise {<pending>}`。

但是一旦动作完成，调用`console.log(foo)`将返回一个包含值的 Promise 对象:在本例中为`Promise {<resolved>}: "bar"`。

## 然后接住

为了对解决或拒绝的承诺执行后续操作，ES6 还引入了两个新方法:`then`和`catch`。这可能会束缚我们最初的承诺。例如，要访问上面承诺`foo`的结果，我们可以使用:

```
foo
  .then(result => console.log(result)
  .catch(err => console.error(err);
```

`then`承诺得到解决时触发，`catch`承诺被拒绝时触发。这些方法可以根据需要链接多次。例如，使用`fetch`请求 JSON 数据时的常见模式如下:

```
fetch(myRequest)
  .then(response => response.json())
  .then(data => {
    processData(data);
  });
```

在第一个`then`方法中，我们使用`json()`来读取和解析数据并将其返回。在下一个`then`方法中，我们可以处理解析后的 JSON 数据。

# 异步和等待

在 ES8 中，异步代码变得更加方便，引入了两个新的关键字:`async`和`await`。

这个系统没有引入任何新的功能。相反，它提供了一个抽象层(或“语法糖”)，允许以非常类似于同步代码的方式编写异步代码。

```
const foo = async () => {
  const result = await new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('bar');
  }, 3000)
  });
  console.log(result);
};
```

您可以使用`async function() {}`定义一个异步函数，或者像上面的例子一样，使用`const myFunctionName = async () => {}`定义一个异步函数。

在`async`函数中，您可以使用`await`关键字来暂停函数的执行，直到承诺得到解决。

这里是另一个例子，我们将使用`fetch`发出 GET 请求，使用 Github 的 API 检索用户数据。不需要显式使用`Promise`对象，因为这隐含在`fetch`方法中:

```
const getUserData = async (user) => {
  let response = await fetch(`https://api.github.com/users/${name}`);
  let data = await response.json();
  return data;
}
```

async/await 语法的唯一问题是，由于它与同步代码相似，很容易陷入同步思维模式。尤其是当我刚接触 async/await 时，我犯了一些错误，忘记了我正在处理的是承诺！

# 将同步功能变为异步功能

让我们来看一个同步函数，它返回数组中每个值的和。

```
function sum(arr) {
  return arr.reduce((x, y) => x + y);
};
```

如果我们的数组特别大，我们可能不希望这个函数阻止其他 JavaScript 代码执行。为了允许其他代码继续运行，我们需要使我们的函数异步。而要做*那个*，我们需要它来回报一个承诺。从 ES8 开始，最简单的方法就是添加`async`关键字:

```
async function sum(arr) {
  return arr.reduce((x, y) => x + y);
};
```

但是，如果我们想要更多地控制我们的承诺的执行呢？隐式地，`async`关键字将我们的函数返回的任何内容转换成一个 Promise 对象，因此下面的函数与上面的函数具有大致相同的行为:

```
const asyncSum = (arr) => {
  return new Promise((resolve, reject) => {
    resolve(arr.reduce((x, y) => x + y))
  });
};
```

然后我们可以调用这个函数，并使用上面描述的方法或关键字来定义进一步的动作，这取决于我们的承诺是否被成功返回。通常，如果我们使用 React 这样的库，我们可能希望在结果返回时更新状态:

```
asyncSum(veryLargeArray)
  .then(result => {
    *this*.setState({ sum: result });
  });
  .catch(err => console.log(err));
```

或者，对于使用`async`和`await`的相同结果:

```
(async () => {
  const result = await asyncSum(veryLargeArray);
  *this*.setState({ sum: result });
})()
```

# **奖励:异步冗余**

最后，鉴于 Redux 的流行，我想我应该提到如何将 Redux 动作——默认情况下是同步的——转换成异步的。

一旦你安装了`redux`和`react-redux`，你还需要安装中间件来允许你的动作创建者返回一个函数而不是一个动作。最受欢迎的选择是`redux-thunk`。

要将 Redux Thunk 合并到您的 Redux 存储中，您可以使用以下样板代码:

然后，您可以返回函数以及动作。下面是一个示例函数，它发出 POST 请求来创建一个项目:

多亏了 Redux Thunk，这个动作现在是‘thenable ’,这意味着我们可以执行一次进一步的动作——而且只能执行一次——结果被成功返回。我用 Redux 很少不实现 Redux Thunk！

我希望这篇文章对你有用，不管你是承诺的新手还是想要复习。如有任何问题，欢迎留言评论！