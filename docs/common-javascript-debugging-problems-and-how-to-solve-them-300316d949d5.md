# 常见的 JavaScript 调试问题及其解决方法

> 原文：<https://javascript.plainenglish.io/common-javascript-debugging-problems-and-how-to-solve-them-300316d949d5?source=collection_archive---------7----------------------->

![](img/4ca003653b03584794f6d7a25d703ec5.png)

使用任何编程语言时，都会有一个阶段你必须处理恼人的 bug 和错误。JavaScript 尤其有很多隐藏的 bug 和错误，等待最佳时机在生产中爆发。无论你是在做本地调试还是远程调试，知道如何修复这些常见的 JavaScript 错误会让你的生活更轻松。

在本文中，我们将探讨各种常见的 JavaScript 问题以及如何解决它们。

# 未捕获的引用错误:“x”未定义

这是最常见的 JavaScript 错误之一，通常发生在代码中引用了一个不存在的变量时。

变量要么没有声明好，要么在错误的范围内声明。

看看下面的代码:

```
console.log(foo) // Uncaught ReferenceError: 'foo' is not defined
```

它会抛出未定义的`Uncaught ReferenceError: ‘foo’`,因为我们没有声明`foo`。这可以通过声明变量`foo`来解决:

```
var foo = "foobar";console.log(foo) // foobar
```

另一个例子是下面的代码:

```
function add() {
  var x = 5, y = 1;
  return x + y;
}console.log(x); // Uncaught ReferenceError: x is not defined
```

这里的问题是`x`只在`add`函数的范围内可用，不能在该函数之外使用。然而，`add`函数可以访问任何全局变量及其局部变量。

要修复它，您需要将变量`x`声明为全局变量。

```
var x = 5, y = 1function add() {
  return x + y;
}console.log(x); // 5
```

这就对了。已经修好了。

# 未捕获的类型错误:无法读取未定义的属性“x”

这是另一个困扰 JavaScript 开发人员的常见恼人的 JavaScript 错误，通常发生在您试图访问一个[未定义值或变量](https://en.wikipedia.org/wiki/Undefined_variable)的属性时。

发生这种情况有很多原因。让我们看看下面的代码:

```
var x = "foobar";console.log(x.substring(1)) // oobarconsole.log(x.foobar) // undefinedconsole.log(x.foobar.substring(1))  // Uncaught TypeError: Cannot read property 'substring' of undefined
```

在上面的例子中，在第 3 行和第 4 行，我们试图访问 x 的一个未定义的值。第 3 行的`x.foobar` 将给出`undefined`，因为`x`没有属性`foobar`，并且试图访问值`undefined`的`substring`将导致一个错误。

有时，在处理 JavaScript 对象时会出现这种错误，例如:

```
const state = {
  common: {
    auth: {
      user: {
        name: "John Smith",
      },
      token: {
        jwt: "eyJAD.LAKDIDCWECUWJKDSnlwpihfwpfo",
        bearer: "LAKDIDCWECUWJKDSnlwpihfwpfo",
      }
    }
  }
}console.log(state.common.auth.user.name) // John Smithconsole.log(state.common.auth.tokne.jwt) // Uncaught TypeError: Cannot read property 'jwt' of undefined
```

在第 15 行，我们把属性名`token`打成了`tokne`，这将导致`undefined`，而访问属性`jwt`将导致`uncaught typeerror`。这可以通过修复错别字名称来解决。

```
console.log(state.common.auth.tokne.jwt)
```

# 未捕获的类型错误:无法读取 null 的属性“x”

这与前面的错误类似，但是当您试图访问空值或空变量的属性时，会出现这种情况。

```
var x = null;console.log(x.substring()) // Uncaught TypeError: Cannot read property 'substring' of null
```

另一件事是，例如，当从 ajax 或 api 加载数据时，如果一个变量、一个对象在初始时被设置为`null`，并且你试图在数据被加载之前访问它。

```
const data = {
  messages: null
};fetch("https://medrum.herokuapp.com/messages/birthday_message/")
.then((r) => r.json())
.then((d) => {
  data.messages = d
})setTimeout(() => {
  console.log(data.messages.messages)
}, 1500)console.log(data.messages.messages) // Uncaught TypeError: Cannot read property 'messages' of null
```

在上面的代码中，我们希望从端点获取数据到`data`对象的`messages`属性中。由于还没有数据，`messages`首先被设置为`null`。

如果您试图在数据未准备好时访问它，您将得到错误`Uncaught TypeError: Cannot read property ‘messages’ of null`。要解决这个问题，您可以检查一下`messages`是否不为空，然后访问它。

```
if(data.messages){
  console.log(data.messages.messages)
}
```

# 未捕获的类型错误:x 不是函数

这也是当你试图调用一个非函数/方法变量时经常发生的另一个错误。

```
const add = 1 + 2;console.log(add); // 3console.log(add()) // Uncaught TypeError: add is not a function
```

如果您试图将`add`作为`add()`函数或方法调用，第 3 行肯定会抛出一个错误。

# 对此的错误引用

这不是一个错误，但更像是一个可能导致各种错误的 bug。大多数 JavaScript 代码在`top-level`方法(不同于顶级函数)中引用`this`时通常会出现这种错误。JavaScript 中的顶级方法可以是函数、对象、类等等中的嵌套函数。

`this`是一种很好的自引用对象的方式，每次调用函数时，它的作用域可能不同，在回调和闭包中调用的方式也可能不同。

```
function mainFunction() {
  this.someMethod1 = function() {
    var elementBtn = document.getElementById('myBtn');
    elementBtn.onclick = function() {
      this.someMethod2(); 
      //Uncaught TypeError: undefined is not a function
    };
  };
  this.someMethod2 = function() {
    alert('OK');
  };
}
```

每当`myBtn`被点击时，它会尝试调用`this.someMethod2()`，它当前引用的是`elementBtn`对象，而不是`mainFunction`。`this`的范围在`onclick`方法中已经改变为当前元素对象。

要解决这个问题，您需要保存一个对`mainFunction`的引用，并用它来调用`someMethod2()`。

```
function mainFunction() {
  const _this = this;
  this.someMethod1 = function() {
    var elementBtn = document.getElementById('myBtn');
    elementBtn.onclick = function() {
      _this.someMethod2();
    };
  };
  this.someMethod2 = function() {
    alert('OK');
  };
}
```

另一个例子是:

```
function App() {
  this.data = null
}App.prototype.getLocalData = function () {
  const data = {
    user: {
      name: "Smith"
    }
  }
  return data
}App.prototype.setData = function (data) {
  this.data = data
}App.prototype.loadApp = function () {
  const data = this.getLocalData();
  this.load = setTimeout(function() {
    this.setData(data);
  }, 0);
}App.prototype.loadApp()
```

在上面的例子中，您得到了错误，因为您正在调用`setTimeout`中的`this.setData`，它与`window.setTimeou` t 是一回事，并且有它自己的`this`引用。

你所说的`this`的语境已经不在`loadApp`中，而是在`window`的语境中。

要解决这个问题，您需要像我们之前做的那样保存一个`this`的引用:

```
App.prototype.loadApp = function () {const _this = thisconst data = this.getLocalData()this.load = setTimeout(function() {_this.setData(data);}, 0);}
```

或者，您可以调用`bind()`方法在现代浏览器中传递正确的`this`引用:

```
App.prototype.loadApp = function () {const data = this.getLocalData()this.load = setTimeout(this.reset.bind(this, data), 0);}App.prototype.reset = function(data){this.setData(data)}
```

# 未捕获的范围错误:超出了最大调用堆栈大小

这是最不希望出现的错误之一，通常是由于处理递归或超范围操作时出现的错误。根据您执行的操作，会出现各种类型的未捕获的 RangeError 错误。

最常见的是`Maximum call stack size exceeded`，它通常出现在非终止递归函数中。参见下面的代码:

```
function iRecurse(){iRecurse();}iRecurse(); // Uncaught RangeError: Maximum call stack size exceeded
```

上面的例子是一个非终止的[递归](https://javascript.info/recursion)，并在一个无限循环中不断调用自己，直到达到最大调用堆栈。

即使我们添加了终止递归的条件，也有很多方法可以引入错误:

```
x = 1function iRecurse(){x+=2;if(x%2==0) return;iRecurse();}iRecurse(); // Uncaught RangeError: Maximum call stack size exceeded
```

上面的例子不会终止，因为`x`模数`2`的每一个增量都不会导致`0`，因为`x`最初是`1`并且每一个增量都将是`odd`个数字。

```
x = 0function iRecurse(){x+=2;if(x%2==0) return;iRecurse();}iRecurse();
```

通过将`x`设置为`0`，我们能够修复这个 bug。

其他范围误差包括:

```
// Arrayvar arrEx = new Array(2) // okvar arrExErr = new Array(-2) // Uncaught RangeError: Invalid array length// Numbervar num = Number(5.45);console.log(num.toFixed(20)) // 5.45000000000000017764console.log(num.toFixed(120)) // Uncaught RangeError: toFixed() digits argument must be between 0 and 100console.log(num.isPrecision(20)) // "5.4500000000000001776"console.log(num.isPrecision(120)) // Uncaught RangeError: isPrecision() digits argument must be between 1 and 100console.log(num.toExponential(20)) // "5.45000000000000017764e+0"console.log(num.toExponential(120)) // Uncaught RangeError: toExponential() argument must be between 0 and 100
```

# 未捕获的类型错误:将循环结构转换为 JSON

这也是处理 JSON 数据时另一个常见的 JavaScript 错误和 bug。如果您正在处理 JSON 数据，就有可能遇到这种类型的错误。

这通常发生在你试图`JSON.stringify`相互循环引用的对象时。

例如:

```
var a = {}var b = {a:b}a.b = bb.a = a
```

如果你尝试这样做:

```
JSON.stringify(b) // Uncaught TypeError: Converting circular structure to JSON
```

您将得到错误:`Uncaught TypeError: Converting circular structure to JSON`。由于`a`和`b`都有相互引用，b 或 a 都不能转换成 JSON。

解决这个问题最简单的方法是在`JSON.stringifying`你的对象之前检查并防止对象的循环引用。您还可以指定一个自定义序列化程序函数来检测和清理循环引用。

你真的不需要自己写，在 [Github](https://github.com/isaacs/json-stringify-safe) 或 [Rookout](https://www.rookout.com/) 上有这样的解决方案，可以帮助识别循环引用。

# 结论

如果您不知道错误来自哪里以及如何解决它，JavaScript 错误/bug 可能会令人讨厌、困惑和耗时。我们已经能够探索一些基本的 JavaScript 错误以及如何修复它们。

当调试 JavaScript 错误时，我建议你打开[严格模式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)，因为它有助于更快地处理错误，并使用 [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools) 。