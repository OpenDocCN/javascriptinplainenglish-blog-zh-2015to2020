# 使用 JavaScript / NodeJS 掌握并发性

> 原文：<https://javascript.plainenglish.io/mastering-concurrency-with-javascript-nodejs-289816acc641?source=collection_archive---------1----------------------->

[并发](https://en.wikipedia.org/wiki/Concurrency_(computer_science))是一个程序、算法或问题的不同部分或单元不按顺序或按部分顺序执行，而不影响结果的能力。

![](img/5a133202521999079a0d0db505829a7e.png)

Photo by [Science in HD](https://unsplash.com/@scienceinhd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 引入的[承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)和[异步](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)函数在 I/O(如网络调用或使用事件循环读取文件等)期间执行代码而不阻塞线程，而不是等到操作完成。一旦操作完成，它就解决了。NodeJS 使用[**libuv**](http://docs.libuv.org/en/v1.x/)**来管理事件循环和 I/O 操作。**

**在日常生活中，我们有许多可能需要串行或并行运行这些异步函数的用例。**

# ****串行运行异步功能****

**假设我们需要对数组中的每个元素应用异步函数。**

**[https://gist.github.com/Deepak13245/c39c67c45abbf5d2a357fba5beeabbde](https://gist.github.com/Deepak13245/c39c67c45abbf5d2a357fba5beeabbde)**

**我们可以使用[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)**代替 for 循环。reduce 的一个属性是当前值依赖于以前的值，所以我们串行运行它们。****

****我们播种了`[]` ( *空数组*)的诺言。我们解析先前的值(*最初为空数组)。然后我们执行作为参数的函数`fn`。*****

****然后将函数或错误的结果推送到数组中，并返回数组。因为给 reduce 的回调是一个异步函数，它返回一个新数组的承诺。****

****让我们看一个要连续执行的样本代码。****

****让我们使用睡眠功能来等待一些东西****

****[https://gist.github.com/Deepak13245/629c8cf3d2178a5d0ce04ae8ba0fe1c7](https://gist.github.com/Deepak13245/629c8cf3d2178a5d0ce04ae8ba0fe1c7)****

****此外，我们将使用一些实用函数来演示执行的本质。****

****[https://gist.github.com/Deepak13245/129ecdbb72c9f0f81827eb421260a8da](https://gist.github.com/Deepak13245/129ecdbb72c9f0f81827eb421260a8da)****

****`measure`用来衡量一个承诺****

****`logPromise`用于记录承诺的结果****

****`chunk`用于将卡盘阵列转换成多个阵列****

****`range`是一个生成`n`数字的小工具****

****`promisify` HoF ( [*高阶函数*](https://en.wikipedia.org/wiki/Higher-order_function) )将回调时尚转换为承诺时尚。****

****`noOp`是一个空函数****

```
**// Example code to run in series
// Let's take an array
const input = range(4, 1);// This function sleep for timeout seconds
async function callback1(timeout) {
  console.log(timeout);
  await sleep(timeout * 1000);
}// Executing this array in series 
// Sleep for 1 second
// Sleep for 2 seconds
// ....
// Sleep for 4 seconds
measure(inSeries(input, callback1));// Output:  Time taken 10s
// Above function callback1 is executed in series
// Time taken = sum( time taken by each promise)**
```

# ******并行运行异步功能******

****我们有很多可能需要并行运行这些异步函数的用例。
示例:假设我们需要调用两个 API 和一个数据库调用，而这两个操作没有依赖关系，即 API 调用的结果不依赖于数据库，反之亦然。
在这种情况下，连续进行这些调用会降低应用程序的速度。我们可以利用异步函数的能力，因为它们是 I/O 操作。****

****[https://gist.github.com/Deepak13245/e49c1853ebceacbd3ab6f725c42b4ba6](https://gist.github.com/Deepak13245/e49c1853ebceacbd3ab6f725c42b4ba6)****

****我们可以使用[**map**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Map)**数组的方法来并行执行异步函数。******

******在这里，我们将元素数组映射到承诺数组，并使用[**promise . all**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/All)**方法等待所有承诺被解析。********

```
****// Example for using inParallel function
// We are using same input and callback1 function used to demonstrate inSeriesmeasure(inParallel(input, callback1));// Output: Time taken 4s
// 4s was the highest timeout and promises were executed in parallel
// Since we waited for all promises to resolve the time taken is 4s
// Time taken = max( time taken by promise promises)****
```

******`inParallel`对于有限的数据量是一个非常好的工具。让我们考虑一个例子，其中我们压缩在给定路径下的所有子路径中存在的图像。******

******[https://gist.github.com/Deepak13245/6cf01898dc91f9f814db86edde61787c](https://gist.github.com/Deepak13245/6cf01898dc91f9f814db86edde61787c)******

******上面的代码片段用于对单个图像进行优化，并将优化后的图像存储回目的地。******

******由于存在 I/O 操作，我们可以利用异步来提高速度******

```
****// Consider we run this whole process in
// globby is npm package to list files with regexp 
const globby = require('globby');async function run() {
  // List all images under images directory with ext png, jpg and jpeg
  const files = globby('./images/**/*.{png,jpg,jpeg}');

  // Process everything in parallel and store the file in same place
  await inParallel(files, file => optimizeImage(file, file));
}logPromise(run());****
```

******输出会是什么？******

******如果只有几个文件，那么它可能会成功完成操作。******

********如果有成千上万张图片会怎么样？********

****系统会因为多种原因崩溃，如*内存不足*或*打开的最大文件数*错误等。****

****这在大多数批量操作中都是一样的。API 会有速率限制，或者数据库会有连接限制(假设没有实现池)。****

****所以结论是我们总是需要控制并发性，也就是说，我们需要并行运行操作，我们可以限制可以并行运行的操作的数量。我们并行运行阵列的一个子集，每个子集串行运行。****

****[https://gist.github.com/Deepak13245/6d5aac83a4ec5a6976be06f8626552d5](https://gist.github.com/Deepak13245/6d5aac83a4ec5a6976be06f8626552d5)****

****我们可以结合上面的函数`inSeries`和`inParallel`来限制并发。
我们使用`chunk`函数将数组转换成二维数组，其中数组的每个元素都是大小为`concurrency`的数组。****

```
**// Rewriting above code to optimize the imagesasync function run() {
  // List all images under images dir with ext png, jpg and jpeg
  const files = globby('./images/**/*.{png,jpg,jpeg}');

  // Process with concurrency of 10
  measure(inParallelWithLimit(files, 10, file => optimizeImage(file, file)));
}// Output: Time taken 160s**
```

****假设我们有 200 张图片，它转换成一个 20 的数组，其中每个数组的元素大小为 10。
因此，我们最终连续执行 20 个元素(即 20 批)，其中每次执行我们并行优化 10 个图像(即每批并行)。****

******那么现在代码优化了吗？编号******

****因为在上面的`inParallel`演示中我们看到并行执行所花费的时间是`max(time taken)`。
考虑在一个批处理中，我们有一个巨大的图像，比如说大小为 *2MB* 并且所有其他文件都小于 *200KB* 并且优化图像关闭 *200KB* 所花费的时间大约为 *1 秒*，2MB 图像所花费的时间大约为 *8 秒。* 考虑我们在所有批次中都有大约 2MB 大小的巨大图像****

****我们可以很容易地得出结论，完成每一批的时间是 8s，20 批大约是 160s。由于一个批处理中的所有内容都是并行运行的，优化 9 个图像所用的时间大约是 1s，我们没有开始新的批处理，因为我们最终要为一个承诺等待 8s。
所以 9 个承诺是在浪费这些无所事事的 8 的时间。****

****我们如何优化这一渠道？
我们使用的是**推送模型**，在这里我们提供数据并等待结果。
相反，我们可以使用**拉模型**，在这里我们定义并发性，每个实体可以在空闲时拉新项目进行处理。****

****让我们称这个每个处理实体为**工作者**。
和数据队列作为**通道**。
我们开始 **x** 数量的工作线程并行使用同一个通道。
一个*工人*在空闲时从*通道*中拾取要加工的工件，并将结果放回同一个*通道。* 在这种情况下**并发=活动工作者的数量******

****[https://gist.github.com/Deepak13245/48e9d4cfd3af96d9db11921822ae7a16](https://gist.github.com/Deepak13245/48e9d4cfd3af96d9db11921822ae7a16)****

****我们定义一个阶级`Channel`。它的构造函数接受一个参数`data`即要处理的数据列表。****

****`push`运行时用于添加更多项目(作业)的方法。****

****`clear`用于清除通道中数据的方法。****

****`results` getter 用于获取已处理作业的结果。****

****`next`方法被*工人*用来从*通道获得下一个作业。*****

****`_callback`方法是一个 ***HoF*** 帮助在结果数组中存储该作业的结果。****

****[https://gist.github.com/Deepak13245/8e2653f6a298e3aef67227816d18dbbb](https://gist.github.com/Deepak13245/8e2653f6a298e3aef67227816d18dbbb)****

****`Worker`类构造函数接受两件事。
1。通道(通道对象):获取数据并存储结果
2。fn(函数):需要应用于每个项目或作业的函数。****

****`setOnStop`:停止回调时设置的帮助器方法****

****`start`:开始处理从频道中挑选的作业。此方法是一个异步函数，一旦它不再有任何要处理的作业，就会被解析。****

****`channel.next()`返回包含
1 的数组。job exists: true/false，如果不存在，只通知 worker 停止并从 start 方法返回。
2。作业数据:是一个数组，有*项*和它的*索引* 3。callback: callback 是一个函数，使用作业的结果调用它，将结果设置回通道中。****

****`stop`:阻止工人加工的帮手。****

****如果我们看到代码，我们就从通道中提取数据。****

****让我们用一个样本代码来利用 worker。****

```
**// Lets consider the same image optimization codeasync function run() {
  // List all images under images dir with ext png, jpg and jpeg
  const files = globby('./images/**/*.{png,jpg,jpeg}');

  // fn to optimize image
  const processor = file => optimizeImage(file, file); // Create new channel with all files
  const channel = new Channel(files); // Create 10 workers
  const workers = range(10)
        .map(() => new Worker(channel, processor));

  // Start all workers in parallel and wait.
  // Start method will resolve after no jobs left to execute
  measure(inParallel(workers, w => w.start()));
}// Output :  Time taken 90s**
```

****我们创建一个包含文件列表的通道。我们创建了 10 个工人，所以并发数是 10。我们并行启动所有 10 个工人，等待他们全部完成。****

****相同数量的图像花费了*90 秒*，比之前的方法花费的 160 秒快了 *~43%* 。****

****我们可以重构上面的代码，使整个过程通用化。****

****[https://gist.github.com/Deepak13245/865cc54dc2975834ab16c22296d724dc](https://gist.github.com/Deepak13245/865cc54dc2975834ab16c22296d724dc)****

****`WorkerManager`类抽象了 worker 和 channel 的创建，使我们能够在启动 worker 后将作业排队，并以最佳的并发量运行。****

****`constructor`接受三个参数。
1。并发性:在任何时刻可以存在的最大工作线程数。
2。fn:这与*工人*构造器参数相同。要应用于每个项目的函数。
3。数据:作业列表，这是可选的。因为我们甚至可以在启动管理器后添加作业。****

****`length` : getter 用于检索该时刻活动工作线程的数量。****

****`results`:它是代理 getter，用于通道检索结果。****

****`_createWorker`:创建新*工作者*并附加 onStop 回调的 Helper 方法。****

****`_createWorkers`:创建多个工人的助手。****

****`_startWorkers`:助手启动给定的工作队列****

****`_onStop`:用于在工作者停止时管理阵列中的工作者。
它接受`worker`对象作为参数。****

****`start`:启动管理器。它依次启动工人。****

****`enqueue`:向频道添加新作业。****

****`finish`:与`worker`的 start 方法不同，`manager`的 start 方法不会返回一个等待完成的承诺。
你需要等待*完成*方法来知道是否所有的工作都完成了。
它接受一个参数*等待时间*。它持续轮询活动工作进程的长度*等待时间*毫秒数。一旦发现活动的工作线程为零，它就进行解析。****

```
**// Rewriting the image optimization code with using manager class.async function run() {
  // List all images under images dir with ext png, jpg and jpeg
  const files = globby('./images/**/*.{png,jpg,jpeg}');

  // Process with concurrency of 10
  const processor = file => optimizeImage(file, file);

  // Create new manager
  const manager = new WorkerManager(10, processor, files);
  manager.start(); // Wait till all the jobs are executed
  measure(manager.finish());
}// Output:  Time taken 90s**
```

****花费的时间没有变化。这只是代码重构的一个例子，以使工人通用和可重用。****

******参考文献:******

****)(我)(们)(都)(没)(想)(到)(这)(样)(,)(我)(们)(还)(没)(想)(到)(这)(样)(,)(我)(们)(就)(没)(想)(到)(这)(样)(了)(,)(我)(们)(就)(没)(想)(到)(这)(样)(。****

****例如: WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB WEB****