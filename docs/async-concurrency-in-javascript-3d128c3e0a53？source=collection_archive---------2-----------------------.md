# JavaScript 中的异步并发

> 原文：<https://javascript.plainenglish.io/async-concurrency-in-javascript-3d128c3e0a53?source=collection_archive---------2----------------------->

![](img/009a48cfdf2fc0e125d90cc12f8bc4e3.png)

我最近的任务是构建一个微服务，它需要发出数千个 HTTP 请求并处理响应。为了创建一个简单的模拟场景，我将使用一个假的`request`函数，该函数接受一个任意值并返回一个`Promise`，该函数解析为一个包含值和间隔的对象:

```
**const** {promisify} = require('util')
**const** sleep = promisify(setTimeout)**const** request = **async**(data) => {
  **let** time = Math.random() * 1000
  **await** sleep(time)
  **return** {data, time}
}
```

我有一堆包含请求参数数据的记录，所以我做的第一件事就是查询这些记录。对于这个例子，我们将使用一个整数数组来模拟记录。我的第一个直觉(因为我没有经验)是遍历记录数组，发出每个 HTTP 请求并将结果推送到一个新的数组:

```
**async function** main() {
  **const** records = Array.from(**new** Array(10)).map((e, i) => i)
  **let** responses = []
  console.time('Inline')
  **for** (**let** i = 0; i < records.length; i++) {
    **let** response = **await** request(records[i])
    responses.push(response)
  }
  console.log(JSON.stringify(responses, null, 2))
  console.timeEnd('Inline')
}
```

这样做很好，但是我们必须在发出下一个请求之前等待每个响应，这意味着可能需要 10 秒钟来执行。结果将如下所示:

```
[
  {     
    "data": 0,
    "time": 48.95140139293264
  },
  {
    "data": 1,
    "time": 351.42969859007377
  },
  ...
]
Inline: 5210.4460449ms
```

5 秒钟是一段很长的时间。现在让我们看看如何并发地执行这些请求。我们可以将每个`request`调用放入一个数组，并使用 Promise.all()等待它们全部解析:

```
**async function** main() {
  **const** records = Array.from(**new** Array(10)).map((e, i) => i)
  console.time('Concurrent')
  let promises = []
  for (let i = 0; i < records.length; i++) {
    promises.push(request(records[i]))
  }
  let responses = await Promise.all(promises)
  console.log(JSON.stringify(responses, null, 2))
  console.timeEnd('Concurrent')
}
```

瞧，我们刚刚将服务速度提高了`records.length`倍！结果看起来像这样:

```
[
  {
    "data": 0,
    "time": 160.08417354131944
  },
  {
    "data": 1,
    "time": 560.08495847237463
  },
  ...,
  {
    "data": 9,
    "time": 223.39482395749209
  }
]
Concurrent: 560.08495847237463ms
```

使用这种非阻塞方法`responses`可以在所有承诺完成后立即得到它们的结果，所以总的执行时间只比最慢的 HTTP 请求多几毫秒。同样的技术可以用于任何异步操作的集合。

只是为了好玩，让我们用`Array.map:`在一行中做同样的事情

```
let responses = await Promise.all(records.map(record => request(record)))
```

希望这有所帮助，编码愉快。