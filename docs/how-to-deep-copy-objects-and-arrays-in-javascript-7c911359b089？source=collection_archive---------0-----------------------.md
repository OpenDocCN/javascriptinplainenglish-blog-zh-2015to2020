# 如何在 JavaScript 中深度复制对象和数组

> 原文：<https://javascript.plainenglish.io/how-to-deep-copy-objects-and-arrays-in-javascript-7c911359b089?source=collection_archive---------0----------------------->

## 复制对象或数组的常用方法只能进行浅层复制，因此深层嵌套引用是个问题。如果一个 JavaScript 对象包含其他对象，您需要一个深层副本。

![](img/c36a88a066f5f715d71a189c9a01d7e9.png)

Photo by [João Silas](https://unsplash.com/@joaosilas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是浅抄？

对数组或对象进行浅层复制意味着在对象内部创建对原始值的新引用，并复制它们。

这意味着对原始数组的更改不会影响复制的数组，如果只复制了对数组的引用就会发生这种情况(例如使用[赋值操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Assignment_Operators) `=`)就会发生这种情况。

浅拷贝是指只拷贝一个级别，这对于只包含原始值的数组或对象来说很好。

对于包含其他对象或数组的对象和数组，复制这些对象需要深层复制。否则，对嵌套引用所做的更改将会更改嵌套在原始对象或数组中的数据。

在本文中，我描述了用 JavaScript 制作浅层副本的 4 种方法和制作深层副本的 5 种方法。

![](img/179e42ae44c5879ae14d6f33df5c2433.png)

Photo by [Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用`…`进行浅层复制

1。[spread 操作符](https://medium.com/javascript-in-plain-english/how-to-add-to-an-array-in-react-state-3d08ddb2e1dc) ( `…`)是一种简单复制数组或对象的便捷方法——当没有嵌套时，它非常有效。

如上所示，spread 运算符对于创建新的数组实例很有用，这些实例不会由于旧的引用而出现意外行为。因此，扩展操作符对于[添加到处于反应状态](https://medium.com/javascript-in-plain-english/how-to-add-to-an-array-in-react-state-3d08ddb2e1dc)的数组是有用的。

![](img/cd57d21eca8ccf10fd0f6487cc556de6.png)

Photo by [Donald Giannatti](https://unsplash.com/@wizwow?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用`.slice()`进行浅层复制

2。特别是对于数组，使用内置的`[.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)`方法与 spread 操作符的工作原理相同——创建一个级别的浅层副本:

![](img/8d1cee78423ec9c271d4df9a87ed0298.png)

Photo by [Antonio Garcia](https://unsplash.com/@angarav?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 浅层复制使用。分配()

3。使用`[Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)`可以创建相同类型的浅拷贝，它可以用于任何对象或数组:

![](img/b785c61a748e9c47dcbeaf3fd2f7bed7.png)

Photo by [Paweł Czerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用`Array.from()`浅拷贝数组

4。复制 JavaScript 数组的另一种方法是使用`[Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)`，这也将进行浅层复制，如下例所示:

如果一个对象或数组包含其他对象或数组，浅表副本将意外工作，因为嵌套对象实际上并没有被克隆。

对于深度嵌套的对象，需要一个深度副本。我在下面解释原因。

![](img/e3e1af88c7479b7375e5b8f58a18b6ad.png)

Photo by [brabus biturbo](https://unsplash.com/@brabusbiturbo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 小心深层嵌套的陷阱！

另一方面，当 JavaScript 对象(包括数组)深度嵌套时，spread 操作符只复制第一层的新引用，但更深的值仍然链接在一起。

要解决这个问题，需要创建深层拷贝，而不是浅层拷贝。可以使用 Ramda 函数编程库中的 [lodash](https://lodash.com/) 、 [rfdc](https://github.com/davidmarkclements/rfdc) 或 [R.clone()方法](https://ramdajs.com/docs/#clone)来制作深度副本。接下来我探索深层副本。

![](img/eed581690d3c4f7d8f8153a61aba7e5d.png)

Photo by [Landon Martin](https://unsplash.com/@lbmartin12?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是深度复制？

F 或包含其他对象或数组的对象和数组，复制这些对象需要深度复制。否则，对嵌套引用所做的更改将会更改嵌套在原始对象或数组中的数据。

这与浅复制相比，浅复制对于只包含基元值的对象或数组工作良好，但是对于任何具有对其他对象或数组的嵌套引用的对象或数组都将失败。

理解`[==](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)`[和](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3) `[===](https://medium.com/better-programming/making-sense-of-vs-in-javascript-f9dbbc6352e3)`之间的区别有助于直观地看到浅拷贝和深拷贝之间的区别，因为严格相等运算符(`===`)表明嵌套引用是相同的:

我将介绍制作深度副本(或深度克隆)的 5 种方法: [lodash](https://lodash.com/) 、 [Ramda](https://ramdajs.com) 、自定义函数、`[JSON.parse()](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32)` / `[JSON.stringify()](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32)`和`[rfdc](https://www.npmjs.com/package/rfdc)`。

![](img/c0370bfa6a10899891f125e3285b6ada.png)

Photo by [mya thet khine](https://unsplash.com/@htetwai2000?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 带 lodash 的深层副本

1。库 [lodash](https://lodash.com/) 是 JavaScript 开发者进行深度复制最常见的方式。它非常容易使用:

Lodash 的名字来自于被引用为下划线(`_`)、“低破折号”或简称 lodash 的库。

![](img/0b5bc96005f04be53b709b9eff77d295.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用 Ramda 进行深度复制

2。函数式编程库 [Ramda](https://ramdajs.com) 包含`[R.clone()](https://ramdajs.com/docs/#clone)` [方法](https://ramdajs.com/docs/#clone)，对一个对象或数组进行深度复制。

注意，来自 Ramda 的`R.clone()`相当于 lodash 的`_.cloneDeep()`，因为 Ramda 没有浅层复制帮助器方法。

![](img/72286ceee67fb04cf3825e823551b945.png)

Photo by [Roi Dimor](https://unsplash.com/@roi_dimor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 具有自定义功能的深层复制

3。编写一个递归的 JavaScript 函数来深度复制嵌套的对象或数组是非常容易的。这里有一个例子:

注意，我还需要[检查空值](https://medium.com/javascript-in-plain-english/how-to-check-for-null-in-javascript-dffab64d8ed5)，因为空值`typeof`是“对象”

![](img/39706a0e6b7a819f600d10633c67bfc7.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 深度复制与[JSON.parse/stringify](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32)

4。如果你的数据符合规范(见下文)，那么`[JSON.stringify](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32)` [后面跟着](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32) `[JSON.parse](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32)`将深度复制你的对象。

> “如果你不在你的对象中使用`Date`、函数、`undefined`、`Infinity`、【NaN】、正则表达式、映射、集合、Blobs、文件列表、ImageDatas、稀疏数组、类型化数组或其他复杂类型，一个非常简单的深度克隆对象的方法是:`JSON.parse(JSON.stringify(object))`”——[丹·达斯卡雷斯库](https://medium.com/u/6717ba7c28b2?source=post_page-----7c911359b089--------------------------------) [在他的 StackOverflow 回答](https://stackoverflow.com/a/122704)

为了说明通常不推荐这种方法的原因，这里有一个使用`JSON.parse(JSON.stringify(object))`创建深层副本的例子:

自定义函数或提到的库可以进行深度复制，而不需要担心内容的类型，尽管循环引用会使它们出错。

接下来，我将讨论一个名为`rfdc`的速度极快的库，它可以处理循环引用，速度与定制的深度复制函数一样快。

![](img/0d3513a25c31cd3adf1b371ba5613b2b.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 真的快速深度复制？思考`[rfdc](https://www.npmjs.com/package/rfdc)`

5。为了获得最佳性能，库`[rfdc](https://www.npmjs.com/package/rfdc)` [(真正快速的深度克隆)](https://www.npmjs.com/package/rfdc)将比 lodash 的`_.cloneDeep`快 400%左右进行深度复制:

> "`rdfc`克隆所有 JSON 类型:`Object` `Array` `Number` `String` `null`
> 
> 附加支持:`Date`(复制)`undefined`(复制)`Function`(引用)`AsyncFunction`(引用)`GeneratorFunction`(引用)`arguments`(复制到普通对象)
> 
> 所有其他类型的输出值与`JSON.parse(JSON.stringify(o))`的输出相匹配。“— [rfdc 文档](https://github.com/davidmarkclements/rfdc)

使用`rfdc`非常简单，就像其他库一样:

`rfdc`库支持所有类型，也支持带有可选标志的循环引用，这会降低大约 25%的性能。

循环引用会破坏其他讨论过的深度复制算法。

如果你正在处理一个大型复杂的对象，比如从 3MB 的 JSON 文件中加载的对象(大小为 15MB ),这样的库会很有用。

以下是基准测试，显示在处理如此大的对象时`rfdc`快了大约 400%:

> `benchLodashCloneDeep*100: 1461.134ms
> benchRfdc*100: 323.899ms
> benchRfdcCircles*100: 384.561ms` — [rfdc 文件](https://github.com/davidmarkclements/rfdc)

*   库`rfdc`(真正快速的深度克隆)是 GitHub 上的[和](https://github.com/davidmarkclements/rfdc) [npm](https://www.npmjs.com/package/rfdc) :

[](https://github.com/davidmarkclements/rfdc) [## davidmarkclements/rfdc

### 真正快速的深度克隆。通过在 GitHub 上创建一个帐户来为 davidmarkclements/rfdc 开发做贡献。

github.com](https://github.com/davidmarkclements/rfdc) [](https://www.npmjs.com/package/rfdc) [## rfdc

### 真正快速的深度克隆

www.npmjs.com](https://www.npmjs.com/package/rfdc) ![](img/9b0096df2be0e549c5c7ac6156eb5306.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# JavaScript 复制算法的性能

在的各种复制算法中，浅层复制最快，其次是使用自定义函数或`rfdc`的深层复制:

> **“按表现深度复制:**从最好到最差排序
> 
> 重新分配"`*=*`"(仅字符串数组、数字数组)
> 
> 切片(仅字符串数组、数字数组)
> 
> 串联(仅字符串数组、数字数组)
> 
> 自定义函数:for 循环或递归复制
> 
> 【*作者注:* `*rfdc*` *会在这里，像自定义函数*一样快
> 
> jQuery 的`*$.extend*`
> 
> `*JSON.parse*`(仅字符串数组、数字数组、对象数组)
> 
> [下划线. js](https://en.wikipedia.org/wiki/Underscore.js) 的`*_.clone*`(仅字符串数组、数字数组)
> 
> 洛-达什的`*_.cloneDeep*`——[蒂姆·蒙塔古](https://medium.com/u/1972a0656243?source=post_page-----7c911359b089--------------------------------) [在他的 StackOverflow 回答](https://stackoverflow.com/users/1404726)

使用`JSON.parse` / `JSON.stringify`会产生关于数据类型的问题，所以推荐使用`rfdc`——除非你想写一个定制函数。

![](img/cec0fdba3ff1c41acdabb4b0dcdb8baf.png)

Photo by [Keila Hötzel](https://unsplash.com/@keilahoetzel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论:如何在 JavaScript 中深度复制

如果你不能将对象和数组嵌套在一起，实际上很容易避免 JavaScript 中的深度复制。

因为在这种情况下——没有嵌套，对象和数组只包含原始值——使用 spread 操作符(`…`)、`.slice()`和`.assign()`进行浅层复制都非常有效。

但是，在现实世界中，对象内部有数组，反之亦然，那么就需要使用深度拷贝。深度克隆我推荐`[rfdc](https://www.npmjs.com/package/rfdc)`。

(注意，有些人可能还会建议使用`[JSON.stringify()](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32)` [后跟](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32) `[JSON.parse()](https://medium.com/javascript-in-plain-english/how-to-use-stringify-and-parse-in-javascript-6b637b571a32)`，但这不是进行深度复制的可靠方式。)

现在出去深层复制一些嵌套的对象！

![](img/d23e78fe4c50f079d00d213f03ae5787.png)

Photo by [Kristina Evstifeeva](https://unsplash.com/@kristinaco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 进一步阅读

*   [Alligator.io](https://alligator.io/js/deep-cloning-javascript-objects/) 有一篇关于使用 lodash 进行深度克隆的很棒的文章:

[](https://alligator.io/js/deep-cloning-javascript-objects/) [## JavaScript 中的深度克隆对象(及其工作原理)

### 如果你计划用 JavaScript 编码，你需要理解对象是如何工作的。它们是最重要的…

鳄鱼. io](https://alligator.io/js/deep-cloning-javascript-objects/) 

*   [Samantha Ming](https://medium.com/u/829a804ea5da?source=post_page-----7c911359b089--------------------------------) 向解释 [dev 上的浅层和深层副本:](https://dev.to/samanthaming/how-to-deep-clone-an-array-in-javascript-3cig)

[](https://dev.to/samanthaming/how-to-deep-clone-an-array-in-javascript-3cig) [## 如何在 JavaScript 中深度克隆数组

### 有两种类型的阵列克隆:浅层和深层。浅层拷贝仅覆盖阵列的第一级，其余的…

开发到](https://dev.to/samanthaming/how-to-deep-clone-an-array-in-javascript-3cig) 

*   [Peter Tasker](https://medium.com/u/e3bdfe7f3308?source=post_page-----7c911359b089--------------------------------) 关于[开发到](https://dev.to/ptasker/best-way-to-copy-an-object-in-javascript-827)的文章引发了许多评论，包括对`JSON.parse(JSON.stringify(obj))`何时失败的解释:

[](https://dev.to/ptasker/best-way-to-copy-an-object-in-javascript-827) [## 用 JavaScript 复制对象的最好方法是什么？

### 所以这些天我一直在寻找一种尽可能使用普通 JS 的方法，我发现深度复制一个

开发到](https://dev.to/ptasker/best-way-to-copy-an-object-in-javascript-827) 

*   [James Dorr](https://medium.com/u/f9fae2b0ef24?source=post_page-----7c911359b089--------------------------------) 用简单的英语在 [JavaScript 中讲述了](https://medium.com/javascript-in-plain-english/setstate-not-working-shallow-copy-vs-deep-copy-lodash-55a015db80ff)[向一个数组添加 React 状态](https://medium.com/javascript-in-plain-english/how-to-add-to-an-array-in-react-state-3d08ddb2e1dc)(因为 React 状态是不可变的)时需要复制:

[](https://medium.com/javascript-in-plain-english/setstate-not-working-shallow-copy-vs-deep-copy-lodash-55a015db80ff) [## 。setState()不起作用—浅层复制与深层复制& Lodash

### 浅层拷贝与深层拷贝& Lodash

浅层复制 vs 深层复制& Lodashmedium.com](https://medium.com/javascript-in-plain-english/setstate-not-working-shallow-copy-vs-deep-copy-lodash-55a015db80ff) 

*   [Ramón Miklus](https://medium.com/u/4b427da842ba?source=post_page-----7c911359b089--------------------------------) 使用[immutanbility-helper](https://www.npmjs.com/package/immutability-helper)，一个用于维护数据不变性的库，而不是 lodash 到 [Codementor](https://www.codementor.io/ramnmiklus/deep-copying-an-object-in-javascript-mdlj2c318) 上的深度复制:

[](https://www.codementor.io/ramnmiklus/deep-copying-an-object-in-javascript-mdlj2c318) [## 在 JavaScript | Codementor 中深度复制对象

### 使用 spread 语法或 Object.assign()是在 JavaScript 中复制对象的标准方式。这两种方法都可以…

www.codementor.io](https://www.codementor.io/ramnmiklus/deep-copying-an-object-in-javascript-mdlj2c318) 

*   说到不变性， [Gabriel Lebec](https://medium.com/u/7b585b769590?source=post_page-----7c911359b089--------------------------------) 在 [dev.to](https://dev.to/glebec/four-ways-to-immutability-in-javascript-3b3l) 中有一个很好的指导:

[](https://dev.to/glebec/four-ways-to-immutability-in-javascript-3b3l) [## JavaScript 中实现不变性的四种方式

### 本文介绍了在 JavaScript 中不变地更新数据结构的四种不同技术:许多代码…

开发到](https://dev.to/glebec/four-ways-to-immutability-in-javascript-3b3l) ![](img/0ae92c2c9ae875309ff5d80cc27a90b8.png)

Photo by [青 晨](https://unsplash.com/@jiangxulei1990?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[Derek Austin](https://www.linkedin.com/in/derek-austin/)博士是《职业编程:如何在 6 个月内成为一名成功的 6 位数程序员 》一书的作者，该书现已在亚马逊上架。