# JavaScript 和获取 API

> 原文：<https://javascript.plainenglish.io/javascript-and-the-fetch-api-98fdad173b85?source=collection_archive---------7----------------------->

最近我一直在学习 JavaScript 和 React，我不得不使用[获取 API](https://fetch.spec.whatwg.org/) 。我意识到我并没有很好地理解我正在使用的工具，并决定我需要挖掘得更深一点。在[我之前的博文](https://medium.com/@reginafurness/javascript-promises-c89572ea034f)中，我回顾了[承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)，它们是 Fetch API 的基本构件。在我解释 Fetch API 的时候，我假设你已经很好地理解了承诺。

# 什么是 Fetch API？

来自 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#:~:text=The%20Fetch%20API%20provides%20a,resources%20asynchronously%20across%20the%20network.) :

> [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 提供了一个 JavaScript 接口，用于访问和操作 HTTP 管道的各个部分，比如请求和响应。它还提供了一个全局的`[fetch()](https://developer.mozilla.org/en-US/docs/Web/API/GlobalFetch/fetch)`方法，该方法提供了一种简单、逻辑的方式来通过网络异步获取资源

简而言之，Fetch API 为我们提供了一个干净的接口，用于发送 HTTP 请求和接收它们的后续响应。`.fetch()`是它提供的发出这些请求的方法。关于`.fetch()`的几件事:

*   它返回一个承诺，该承诺解析为一个[响应](https://developer.mozilla.org/en-US/docs/Web/API/Response)对象。
*   它需要至少一个参数，要么是我们获取的资源的 URL，要么是一个[请求](https://developer.mozilla.org/en-US/docs/Web/API/Request/Request)对象。
*   当给定一个 URL 作为第一个参数时，还可以提供可选参数的第二个参数。比如 [HTTP 方法](https://pokeapi.co/api/v2/pokemon/1)，头或者体。

注意:`.fetch()`的 URL 和可选参数与给请求构造函数的参数完全相同。这就是为什么我们可以给`.fetch()` 一个请求对象作为参数。

# 响应对象

响应对象有一些属性，包括`Response.ok`，它返回一个布尔值，指示响应是否成功。以及返回响应的[状态码](https://www.restapitutorial.com/httpstatuscodes.html)的`Response.status`。您可以在这里找到响应对象属性的完整列表[，但是对于这个博客来说，响应的状态/状态代码是最重要的。这是因为从`.fetch()`返回的承诺不会以 HTTP 错误状态拒绝。这意味着即使响应的状态为 404，它也不会触发`.catch()`或拒绝回调。](https://developer.mozilla.org/en-US/docs/Web/API/Response)

回应也伴随着身体。您可以将正文视为响应中返回的内容。响应体附带了一些解析内容的方法。每个方法都返回一个解析为相应格式的承诺。这些方法是:

*   `Body.json()`
*   `Body.text()`
*   `Body.formData()`
*   `Body.arrayBuffer()`
*   `Body.blob()`

现在我们知道了如何访问和解析在响应对象中接收的数据。我们需要想出如何发出请求。

# 发送请求

发出获取请求的最简单方法是只提供一个 URL。

```
fetch('[https://pokeapi.co/api/v2/pokemon/1](https://pokeapi.co/api/v2/pokemon/1)')
```

因为`.fetch()`带有默认参数，这相当于向[https://pokeapi.co/api/v2/pokemon/1](https://pokeapi.co/api/v2/pokemon/1)发送一个`GET`请求。让我们看看写出默认参数后的 fetch 请求是什么样子的:

```
//note: you can write this inline
let options = {
    method: 'GET',
    mode: 'cors',
    cache: 'default',
    credentials: 'same-origin',
    headers: {
      'Content-Type': 'application/json'
   },
    redirect: 'follow',
    referrerPolicy: 'no-referrer'
}fetch('[https://pokeapi.co/api/v2/pokemon/1](https://pokeapi.co/api/v2/pokemon/1)', options)
```

你可以看到`.fetch()`的简单可以掩饰很多幕后发生的事情。让我们花点时间来分解这些可选参数。

## 方法

`method`是请求的 HTTP 方法。你可以在这里阅读 HTTP 请求方法[。](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

## 方式

`mode`用于确定跨来源响应是否有效，以及响应的哪些方面是可读的。为了更好地理解这个话题，你可能需要阅读一下跨来源资源共享( [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) )。模式选项包括:

*   `same-origin`不允许外部来源的请求。
*   `no-cors`限于`HEAD`、`GET`或`POST`。将集管限制为`Accept`、`Accept-Language`、`Content-Language`和`Content-Type`。无法使用 JavaScript 访问响应。
*   `cors`允许跨来源请求，且正文可读。

## 隐藏物

`cache` 确定请求与浏览器缓存的交互方式。如果您想了解更多，我建议您阅读 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/API/Request/cache)。请求的这个属性在模式上有很多微妙的不同。

## 资格证书

`credentials`确定请求将如何处理 cookies。不同的选项有:

*   `omit`不要发送或接收饼干。
*   `same-origin`如果请求是针对同一来源的网址，则包括 cookies。
*   `include`包括 cookies，即使是跨来源请求。

## 再直接的

`redirect`顾名思义，决定您的请求如何处理重定向。三种不同的方案是:

*   `follow`您的请求将遵循重定向。
*   `error`重定向时会返回错误。
*   `manual`返回重定向的网址，响应的重定向属性为真。如其所示，允许您手动处理重定向。

## referenrerpolicy

`referrerPolicy` / `referrer` 将决定您的请求如何处理它的“引用者”头。这通常设置为发出请求的 URL。它可以更改为空字符串(`''`)或当前来源中的另一个 URL。您可以在这里更多的了解这个[。](https://javascript.info/fetch-api#referrer-referrerpolicy)

## 头球

最后但同样重要的是`headers`。类似于响应和请求，有一个带有构造函数的 Headers 对象。如果需要，您可以提供自己唯一的标题。您也可以在这里设置几个重要的标题。例如:

*   `content-type`用于指示响应体中内容的类型。因此，如果你正在发送 JSON，你可以将其设置为`content-type: 'application/json'.`
*   `accepts`解释客户端将接受哪种内容类型。
*   `Authorization`应该包含授权用户的凭证。

您可以在这里了解更多类型的标题。

## 请求正文

所以我们查看了一个`GET` 请求，但是如果我们正在发出一个`POST`、`PATCH`或`PUT`请求呢？这就是请求体出现的地方。您使用请求主体通过请求发送内容/数据，类似于使用主体从响应接收内容/数据。通过身体，我们能够发送:

*   `formData` —用于发送文件。
*   `Blob` —用于发送二进制数据。
*   一根绳子
*   `[URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)`

值得注意的是，字符串、`formData`和`Blob`会自动设置`content-type`头。当发送 JSON 时，您需要首先对它进行字符串化，因为您不能通过 fetch 请求发送 JSON。

# 把所有的放在一起

我们已经看到了如何发出请求和处理响应。是时候把它们放在一起，实际发出一些获取请求，并解析这些请求的响应了！有两种方法来处理获取请求。我们可以使用`await`或`.then()`。让我们分别执行一个`GET`请求。

## 等待

```
let response = await fetch('[https://pokeapi.co/api/v2/pokemon/1'](https://pokeapi.co/api/v2/pokemon/1'))response.headers.get('content-type') => "application/json; charset=utf-8"let pokemon = await response.json()
pokemon.name => 'bulbasaur'
```

好吧，我们来分析一下。首先，我们声明一个变量 response，作为对`GET 'https://pokeapi.co/api/v2/pokemon/1'`请求的返回值。我们知道`.fetch()`请求返回一个承诺，该承诺解析为一个响应对象，该响应对象的主体包含我们请求的内容。为了访问这个主体，我们需要调用一个方法来将它解析成我们可以使用的东西。您可以看到，当我们从响应对象获得`content-type`头时，它是`application/json`，因此我们可以调用`.json()`将主体数据转换成 JSON。在那里，我可以看到解析后的 JSON 响应主体数据中的 name 属性。

## 。然后()

```
let pokemonfetch('[https://pokeapi.co/api/v2/pokemon/1'](https://pokeapi.co/api/v2/pokemon/1'))
.then(resp => resp.json())
.then(json => pokemon = json)pokemon.name => "bulbasaur"
```

现在让我们看看如何发出一个`POST`请求。

## 等待发布请求

```
let pokemonObj = {name: 'pikachu', type: 'electric'}let options = {
  method: "POST",
  headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json'
    },
  body: JSON.stringify(pokemonObj)
}let response = await fetch('http://localhost:3000/pokemon', options)
let pokemon = await response.json()
pokemon.name => 'pikachu'
```

你可以看到我们已经包括了我们的`method`，和一些`headers`。因为我们发送的是字符串化的 JSON，所以我们需要包含`content-type`头。我还包含了`accept`头文件，表明我期待 JSON 的回归。

## 。then()发布请求

```
let pokemon
let pokemonObj = {name: 'pikachu', type: 'electric'}let options = {
  method: "POST",
  headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json'
    },
  body: JSON.stringify(pokemonObj)
}fetch('http://localhost:3000/pokemon', options)
.then(resp => resp.json())
.then(json => pokemon = json)pokemon.name => 'pikachu'
```

# 最后的想法

Fetch API 使得通过异步网络请求发送和接收数据变得非常简单。以至于一开始我没有意识到正在发生的一切。关于`.fetch()`、异步 JavaScript、网络请求等等，还有很多东西需要学习。然而，我觉得在深入研究之后，我可以更自信地在我的代码中使用这些工具。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)