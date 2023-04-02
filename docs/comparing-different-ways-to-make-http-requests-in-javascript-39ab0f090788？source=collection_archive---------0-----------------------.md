# 比较 2020 年用 Javascript 发出 HTTP 请求的不同方式

> 原文：<https://javascript.plainenglish.io/comparing-different-ways-to-make-http-requests-in-javascript-39ab0f090788?source=collection_archive---------0----------------------->

## 简洁而有用——让它变得简单。

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

Image of two monitors with code in their screens

最近，我不得不决定一个大型 javascript 项目使用什么技术来进行 ajax 调用。如果您使用 JavaScript，您有不同的机会发出呼叫请求。

最初，有一些方法可以在不刷新页面的情况下从服务器中提取数据，但是它们通常依赖于笨拙的技术。微软开发了 XMLHttpRequest 浏览器来替代他们的 Outlook 电子邮件客户端。XMLHttpRequest 在 2006 年成为 web 标准。

Fetch API 是 ES6 在 2015 年推出的。通用的请求和响应接口提供了一致性，而 Promises 允许更简单的链接和异步/等待，无需回调。Fetch 干净、优雅、易于理解，但是还有其他好的替代方法，我们将在本文中简要介绍它们。

为了简单起见，我将把重点放在它们是什么、它们的语法以及每种选择的优缺点上。

*   XMLHttpRequest
*   JQuery.ajax
*   Qwest
*   超级代理
*   Http 客户端
*   Axios
*   取得
*   既然已经弃用，我就不说了。

下面的代码显示了使用不同替代方法的基本 HTTP GET 和 POST 示例。那我们开始吧。

# XMLHttpRequest

XMLHttpRequest 对象可用于从 web 服务器请求数据。它是这种比较的最古老的方法，尽管其他选项超过了它，但由于它的向后兼容性和成熟度，它仍然是有效和有用的。

得到

```
var req = new XMLHttpRequest();//The onreadystatechange property
//specifies a function to be 
//executed every time the status
//of the XMLHttpRequest changes
req.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
       //The responseText property
       //returns a text string           
       console.log(xhttp.responseText)
       //Do some stuff
    }
};req.open("GET", "http://dataserver/users", true);
req.send();
```

邮政

```
var formData = new FormData();
formData.append("name", "Murdock");
var req = new XMLHttpRequest();
req.open("POST", "http://dataserver/update");
req.send(formData);
```

赞成的意见

*   在所有浏览器中工作
*   是一个本地浏览器 API
*   没有必要从外部源加载它
*   向后兼容性
*   成熟/稳定

骗局

*   笨拙而冗长的语法
*   支持回调地狱
*   Fetch 会自动替换它

# JQuery.ajax

直到不久前，库还被广泛用于进行 HTTP 异步请求。

jQuery 的所有 Ajax 方法都返回 XMLHTTPRequest 对象的超集

得到

```
$.ajax({
    url: 'http://dataserver/data.json'
  }).done(function(data) {
    // ...do some stuff whith data
  }).fail(function() {
    // Handle error
});
```

邮政

```
$.ajax({
  type: "POST",
  url: 'http://dataserver/update',
  data: data,
  success: successCallBack,
  error: errorCallBack,
  dataType: dataType
});
```

赞成的意见

*   良好的支持和文档
*   可配置对象
*   很多项目都是如此
*   低学习曲线
*   您可以中止请求，因为它返回一个 XMLHttpRequest 对象。

骗局

*   没有什么是本土
*   有必要从外部源加载它
*   添加了所有的 JQuery 功能，而不仅仅是那些进行 HTTP 请求所必需的功能。

## Qwest

Qwest 是一个基于 promises 的简单 ajax 库，支持 XmlHttpRequest2 独特的数据，如 ArrayBuffer、Blob 和 FormData。

得到

```
qwest.get('http://dataserver/data.json')
     .then(function(xhr, response) {
        // ...do some stuff whith data
     });
```

邮政

```
qwest.post('http://dataserver/update', {
        firstname: 'Murdock',       
        age: 30
     })
     .then(function(xhr, response) {
        // Make some useful actions
     })
     .catch(function(e, xhr, response) {
        // Process the error
     });
```

赞成的意见

*   您可以建立一个请求限制
*   基于承诺的

骗局

*   XmlHttpRequest2 并非在每个浏览器上都可用
*   没有什么是本土
*   有必要从外部源加载它

## 超级代理

SuperAgent 是为灵活性、可读性和低学习曲线而创建的 ajax API。它也适用于 Node.js

得到

```
request('GET', 'http://dataserver/data.json').then(
success, failure);
```

的。query()方法接受对象，这些对象在与 GET 方法一起使用时将形成一个查询字符串。下面将产生路径/dataserver/search？名字=曼尼&姓氏=佩克&订单=desc。

```
request
   .get('/dataserver/search')
   .query({ name: 'Templeton' })
   .query({ lastname: 'Peck' })
   .query({ order: 'desc' })
   .then(res => {console.dir(res)}
});
```

邮政

```
request
   .post('http://dataserver/update')
   .send({ name: 'Murdock' })
   .set('Accept', 'application/json')
   .then(res => {
      console.log('result' + JSON.stringify(res.body));
   });
```

赞成的意见

*   基于承诺的
*   适用于 Nodejs 和浏览器
*   若要中止请求，请调用 request.abort()方法
*   社区里有名的图书馆
*   无缝接口进行 HTTP 请求
*   出现故障时支持重试请求

骗局

*   它不支持像 XMLHttpRequest 那样监控加载进度
*   没有什么是本土
*   有必要从外部源加载它

## Http 客户端

Http-client 允许您使用 JavaScript 的 fetch API 构建 Http 客户端。

得到

```
//using ES6 modules
import { createFetch, base, accept, parse } from 'http-client'const fetch = createFetch(
  base('http://dataserver/data.json'),  
  accept('application/json'),     
  parse('json')                      
)fetch('http://dataserver/data.json').then(response => {
  console.log(response.jsonData)
})
```

邮政

```
//using ES6 modules
import { createFetch, method, params } from 'http-client'const fetch = createFetch(
  params({ name: 'Murdock' }),
  base('http://dataserver/update')
)
```

赞成的意见

*   它在浏览器和节点上都可以工作
*   由服务人员使用
*   基于承诺的
*   为更好的 CORS 安全性提供头部防护

骗局

*   没有什么是本土
*   有必要从外部源加载它

# Axios

基于 Promise 的 HTTP 库，用于在浏览器和 Nodejs 上执行 HTTP 请求。

得到

```
axios({
  url: 'http://dataserver/data.json',
  method: 'get'
})
```

邮政

```
axios.post('http://dataserver/update', {
    name: 'Murdock'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

赞成的意见

*   使用承诺来避免回调地狱
*   它在浏览器和节点上都可以工作
*   支持上传进度
*   可以设置响应超时
*   通过简单地传递一个配置对象来配置您的请求
*   Axios 已经实施了可取消承诺提案
*   自动将数据转换成 JSON

骗局

*   没有什么是本土
*   有必要从外部源加载它

# 取得

Fetch 是一个本地浏览器 API，用来发出一个请求，代替 XMLHttpRequest。Fetch 使得网络请求比 XMLHttpRequest 更容易。Fetch API 使用承诺来避免 XMLHttpRequest 回调地狱。

得到

```
//With ES6 fetch
fetch('http://dataserver/data.json')
  .then(data => {
    // ...do some stuff whith data
  }).catch(error => {
    // Handle error
});
```

邮政

```
fetch('http://dataserver/update', {
  method: 'post',
  headers: {
    'Accept': 'application/json, text/plain, */*',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({name: 'Murdock'})
}).then(res=>res.json())
  .then(res => console.log(res));//OR with ES2017 for example(async () => {

  const response = await fetch(‘http://dataserver/update’, {
    method: ‘POST’,
    headers: {
      ‘Accept’: ‘application/json’,
      ‘Content-Type’: ‘application/json’
    },
    body: JSON.stringify({name:’Murdock’})
  });const result = await response.json();console.log(result);
})();
```

赞成的意见

*   是一个本地浏览器 API
*   Fetch 基本上是 XMLHttpRequest 做对了
*   不需要从外部源加载它
*   使用承诺来避免回调地狱
*   不需要更多的依赖
*   友好且易于学习
*   兼容最近常用的大多数浏览器
*   是本机 XMLHttpRequest 对象的自然替换
*   低学习曲线

骗局

*   在处理 JSON 数据时，这是一个两步过程。第一个是发出请求，第二个是调用。响应上的 json()方法。在 Axios 上，默认情况下会得到 JSON 响应。
*   从 Fetch()返回的承诺仅在网络故障或任何阻止请求完成的情况下被拒绝。即使响应是 HTTP 404 或 500，也不会拒绝 HTTP 错误状态
*   缺少其他库的一些有用的特性，例如:取消请求
*   默认情况下，Fetch 不会发送或接收来自服务器的 cookies，如果站点依赖于维护用户会话，则会导致未经验证的请求。但是您可以通过添加{
    凭证来启用:“同源。”
    }

## 结论

Fetch 是新标准，新版本的 Chrome 和 Firefox 都支持它，不需要使用任何额外的库。

建议花些时间了解 Axios、SuperAgent 或其他库的特征，因为它们都有适当的文档，易于使用，并且它们的学习曲线不会太长。在某些情况下，它们提供没有 Fetch 的特性。

在我的例子中，我将使用 Fetch，因为我不需要特殊的功能，Fetch 在 JavaScript 中是原生的，对我的项目来说足够了。

如果你喜欢这篇文章，可以考虑通过我的[个人资料](https://kesk.medium.com/membership)订阅 Medium。谢谢大家！