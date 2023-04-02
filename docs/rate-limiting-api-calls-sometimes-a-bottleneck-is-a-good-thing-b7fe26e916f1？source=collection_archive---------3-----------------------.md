# 速率限制 API 调用——有时瓶颈是一件好事

> 原文：<https://javascript.plainenglish.io/rate-limiting-api-calls-sometimes-a-bottleneck-is-a-good-thing-b7fe26e916f1?source=collection_archive---------3----------------------->

## 什么是瓶颈，为什么我的编码生涯中需要它？

如果你曾经使用过第三方 API，你会遇到这样的问题:你对一个 API 进行了大量的调用，但它并没有完成你想要的。您可能会得到一个有用的错误，如 429——太多的请求或一些不太有用的错误，如 ECONNRESET

不管是哪种情况，作为 API 的消费者，你只能在一定的时间内发出一定数量的请求，或者你可以发出的并发请求的数量是有限的。

在 JavaScript 中，您的代码可能如下所示:

```
const axios = require('axios');async function getMyData(data){
  const axiosConfig = {
    url: '[https://really.important/api'](https://really.important/api'),
    method: 'post',
    data
  }
  return axios(axiosConfig)
}async function getAllResults(){const sourceIds = []// Just some code to let us create a big dataset
  const count = 1000000;
  for(let i = 0; i < count; i++){
    sourceIds.push({
      id: i
    });
  }// Map over all the results and call our pretend API, stashing the promises in a new array
  const allThePromises = sourceIds.map(item => {
    return getMyData(item);
  })try{
    const results = await Promise.all(allThePromises);
    console.log(results);
  }
  catch(err){
    console.log(err);
  }}
```

这里将要发生的是，代码将尽可能快地调用 1000000 次，所有请求将在很短的时间内发生(在我的 MacBook Pro 上是< 700ms)

Understandably, some API owners might be a little upset by this as it’s creating a heavy load.

## What do we need to do?

We need to be able to limit the number of requests we’re making, potentially both in terms of the number of API calls in a space of time and in terms of the number of concurrent requests.

I’d encourage you to attempt to roll your own solution as a learning exercise. For example, there is a reasonably simple solution that can get you out of a hole using setInterval. What I think you’ll find is that building a reliable solution that limits rate and concurrency is actually trickier than it looks and requires you to build and manage queues. It’s even more complicated if you’re clustering.

We can instead turn to a gem of a package on NPM — Bottleneck
[https://www.npmjs.com/package/bottleneck](https://www.npmjs.com/package/bottleneck)

作者将此描述为:

*瓶颈是 Node.js 和浏览器的轻量级、零依赖的任务调度器和速率限制器。*

你要做的是创建一个“限制器”，并用它来包装你想要速率限制的函数。然后，您只需调用受限版本。

我们之前的代码变成了:

```
const axios = require('axios');
const Bottleneck = require('bottleneck');const limiter = Bottleneck({
  minTime: 200
});async function getMyData(data){
  const axiosConfig = {
    url: '[https://really.important/api'](https://really.important/api'),
    method: 'post',
    data
  }
  return axios(axiosConfig)
}const throttledGetMyData = limiter.wrap(getMyData);async function getAllResults(){const sourceIds = []// Just some code to let us create a big dataset
  const count = 1000000;
  for(let i = 0; i < count; i++){
    sourceIds.push({
      id: i
    });
  }// Map over all the results and call our pretend API, stashing the promises in a new array
  const allThePromises = sourceIds.map(item => {
    return throttledGetMyData(item);
  })try{
    const results = await Promise.all(allThePromises);
    console.log(results);
  }
  catch(err){
    console.log(err);
  }}getAllResults()
```

如您所见，我们已经创建了一个带有 minTime 属性的限制器。这定义了请求之间必须经过的最小毫秒数。我们有 200 个，所以我们每秒发出 5 个请求。

然后，我们使用限制器包装我们的函数，并调用包装版本:

```
const throttledGetMyData = limiter.wrap(getMyData);
...
  const allThePromises = sourceIds.map(item => {
    return throttledGetMyData(item);
  })
```

如果您的请求有可能花费的时间超过 minTime，您也可以通过如下设置来限制并发请求的数量:

```
const limiter = Bottleneck({
 minTime: 200,
 maxConcurrent: 1,
});
```

这里我们将确保一次只有一个请求被提交。

## 它还能做什么？

设置瓶颈函数有许多选项。您可以使用库选项在一段时间内限制速率，例如，每 60 秒最多发送 100 个请求。或者，发送第一批请求，然后每隔 x 秒发送一批请求。

NPM 的文档非常好，所以我建议你阅读它，以充分了解这个软件包的功能，以及当事情不像你预期的那样运行时的问题。

## 包扎

如果您曾经需要一个高度灵活的包来处理如何限制对 API 的调用，那么瓶颈就是您的朋友。

![](img/054c91610ccaa528d1c1cc1cfeb349bb.png)

## **简明英语团队的一份说明**

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****