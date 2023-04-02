# 直到你能打败这个游戏，你才知道 JavaScript

> 原文：<https://javascript.plainenglish.io/you-dont-know-javascript-until-you-can-beat-this-game-aa7fd58befb?source=collection_archive---------0----------------------->

## 每次把钱押在获胜的马上，你就可以称自己是真正的 JavaScript ***鉴赏家***

![](img/09506a1b4ad7781b6a626de06af0e4df.png)

Photo by [Pietro Mattia](https://unsplash.com/@pietromattia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

前几天，我在对一个中级开发人员职位进行例行电话筛选时，问了以下问题:

> 我:JavaScript 是同步的还是异步的？
> 
> 考生:是异步的。
> 
> 我:你确定？JavaScript 本身？
> 
> 候选人:100%。

这个答案让我担心。这让我很担心，因为我知道他有大约 3 年的编写生产 JavaScript 代码的经验。他是一个相当有能力的开发人员，并且正确地回答了我的大部分问题。然而，他的回答立即引起了我的怀疑。(**注:**有人在说这是个技巧性的问题。要明确的是，目标不是欺骗候选人。目标是以开放式的方式引发关于该主题的进一步对话，看看他们熟悉什么。我担心的是，如果有人用一个词明确地回答是/否，而不去深究*为什么*他们会给出这样的答案。我绝不会仅仅因为候选人在这里的回答就取消他的资格)。

我认为这不是他的错。如果您只是编写 JavaScript 代码，而没有先了解它的历史和内部工作原理，您可能会认为它是异步的。我想建议这是*错的。*至少，这是一种误导。

在我看来，JavaScript 是一种同步语言，并且一直如此。当我们想到在某些环境中运行 JavaScript 时(浏览器或带有节点的服务器端)，就会产生困惑。这是因为这些环境让我们可以访问异步功能，比如经典的`setTimeout`函数。值得注意的是，`setTimeout`函数本身是**而不是**原生 JavaScript 的一部分，而是浏览器 API 的一部分。(你*可能*认为既然承诺现在是 ECMAScript 的一部分，既然 JavaScript 是 ECMAScript 的“方言”,那么它本质上也一定是异步的。这是一个有趣的观点，但在我看来最终没有抓住要点，所以我不会在这里讨论它)。

这种语言看似模棱两可的本质经常会在我们的日常代码(和工作面试)中导致许多意想不到的后果。更糟糕的是，我们并不总是意识到这些误解，所以我们不知不觉地将它们作为绝对真理传递给其他人，放大了手头的问题。

因此，我认为设计各种场景来检查和进一步剖析由于将同步 JavaScript 代码与各种 JavaScript 环境提供的额外异步功能混合在一起而产生的复杂性将是有趣且有用的。

为了让它更有趣一点，我将把各种情况/脚本呈现和游戏化为比赛，把运行的函数呈现和游戏化为马。游戏的目标是正确推断每场比赛中马匹的最终排名。如果你把排名搞错了，或者只是想进一步理解，我还会补充一个解释部分。

**免责声明:**我假设你已经对事件循环有些熟悉了。如果你不是，我建议你去看看这篇演讲，它会比我解释得更好。

现在，让我们从一个琐碎的热身比赛开始。

# **热身赛**

```
console.log("Horse A");
console.log("Horse B");
console.log("Horse C");
```

**结果**

1.  马 A
2.  马 B
3.  马 C

**解释**

JavaScript 是同步的！

# **第一场比赛**

```
setTimeout(() => console.log("Horse A"), 1000)
console.log("Horse B")
```

**结果**

1.  马 B
2.  马 A

**解释**

啊，JavaScript 的经典入门。这里最常见的错误是认为 JavaScript 会在进入下一行之前等待`setTimeout`完成。这是有道理的。毕竟 JavaScript 是同步的。然而，回想一下,`setTimeout`本身并不是 JavaScript 固有的。它是浏览器 API 的一部分。

这里发生的事情是 JavaScript 将把`setTimeout`推到调用堆栈上。然后，它将从堆栈中弹出，并发送到浏览器 API，在后台执行其工作。一旦 1000 毫秒过去，工作就完成了，现在它将被发送到一个称为**作业/任务队列**的数据结构。一旦它在这里，一个简单的(读起来:神奇的)机制称为**事件循环**将检查调用栈是否为空。如果是，那么它将取出任务队列中的第一项，并将其推送到堆栈中，在那里它将最终被执行。否则，它将继续执行调用堆栈中的函数。当然，当马 A 做所有这些事情的时候，马 B 被推到调用堆栈上并被执行，因此已经完成了比赛。

如果你不熟悉这些概念，那么一下子可能会消耗很多。别担心，网上有无数的资源，比我资源丰富得多，可以教你所有关于事件循环的知识。[这里](https://www.youtube.com/watch?v=cCOL7MC4Pl0)又是一个关于这个话题的大谈。

# 第二场比赛

```
setTimeout(() => console.log("Horse A"), 0)
console.log("Horse B")
```

**结果**

1.  马 B
2.  马 A

**解释**

这是一个经典的面试问题。注意，除了现在`setTimeout`的持续时间是 0 毫秒而不是 1000 毫秒之外，这与比赛 1 的情况相同。大多数人错误地认为，既然`setTimeout`现在只需要 0 毫秒，那么在我们有机会移动到下一行之前，它的回调就会立即执行。他们忽略的事实是，JavaScript 处理这些外部异步函数的方式实际上没有任何变化。`setTimeout`方法仍然被推送到调用堆栈，然后被发送到浏览器 API，接着进入任务队列。在任务队列中，事件循环查看我们的调用堆栈，发现`console.log("Horse B")`已经被压入其中。因此，它不能从任务队列中出列马 A(记住，只有当调用栈为空时，事件循环才会从任务队列中出列)，直到`console.log("Horse B")`已经被执行。

# 比赛 3

```
setTimeout(() => console.log("Horse A"), 0);wait60Seconds(); //some incredibly expensive synchronous computationconsole.log("Horse B")
```

**结果**

1.  马 B
2.  马 A

**解释**

你可能会错误地认为由于`wait60Seconds`花费了很长的时间，那么`setTimeout`会在马 B 被打印之前完成并被推入堆栈。我希望现在已经很清楚，这种情况永远不会发生。在 JavaScript 中，同步操作**总是**优先。当有任何类型的同步函数被压入堆栈时，无论花费多长时间，事件循环都不会将异步操作出队。

# 比赛 4

```
const asyncOperation = fetch('something.com');asyncOperation.then((resolvedValue) => console.log("Horse A"));console.log("Horse B")
```

**结果**

1.  马 B
2.  马 A

**解说**

这是人们经常犯的错误。一个常见的误解是，对 Promise 调用`.then`会导致 JavaScript 立即调用`.then`函数内部的函数。毕竟，在调用`console.log("Horse B")`之前，你显然可以访问`resolvedValue`，或者至少在我们的脚本中是这样的顺序。它甚至被命名为一旦你有了你的承诺对象，你就可以*然后*看到里面是什么。但这是误导。`.then`方法真的应该改名为`.wheneverThisValueGetsResolved`(可惜没那么性感)。

JavaScript 真正做的是:当我们调用`fetch`时，我们得到一个承诺对象。这个承诺对象实际上有两个隐藏属性，`value`和`onFulfilled`。当我们将`fetch`推送到调用堆栈时，它被发送到浏览器 API 并在后台完成工作，类似于`setTimeout`方法。当它完成这项工作时，所有后续的同步操作都会像以前一样执行。

但是等等，在进入`console.log`之前，我们明确地调用了我们的承诺`.then`！嗯…这就是两个隐藏属性`value`和`onFulfilled`发挥作用的地方。

当我们的后台操作完成后，它更新我们的承诺的`value`属性。发生这种情况时，该值会自动传递到`onFulfilled`属性中。好吧，但是`onFulfilled`有什么特别的？答案是，您传递给`.then`方法的函数实际上被推到了承诺的`onFulfilled`属性上。换句话说，`console.log("Horse A")`实际上被推到了我们承诺的`onFulfilled`属性上。当承诺最终实现时，(即，我们得到我们的值)，那么`onFulfilled`中的函数将被执行。这就是为什么`.then`真的应该被命名为`.wheneverThisValueGetsResolved`。

TL；DR:您在`.then`方法中编写的函数只有在承诺本身被解决时才会被执行。不要一接到电话就马上答应`.then`。

# 比赛 5

```
setTimeout(() => console.log("Horse A"), 0);const promise = Promise.resolve(); 
promise.then(() => console.log("Horse B"));console.log("Horse C")
```

**结果**

1.  马 C
2.  马 B
3.  马 A

**解释**

等等，什么？马 B 是怎么在马 A 之前完成的！？`setTimeout`明明跑在诺言被叫之前！WTF JavaScript？

基于我之前对 JavaScript 工作原理的解释，这些结果肯定是错误的。在引擎盖下，我们知道`setTimeout`被推到任务队列中，后面是承诺(我们知道队列如何工作),所以结果应该是马 C、马 A 和马 b。嗯……这实际上是错误的。

我之前没有提到的是，实际上还有另一个队列叫做**微任务队列**(最初叫做`PromiseJobs`)专门管理承诺。您猜对了:这个队列优先于常规任务队列(有时通过 V8 称为宏任务队列)。确切地说，简化的算法看起来像这样:

1.  运行首先从任务队列(宏任务队列)中出列的任务
2.  运行微任务队列中的所有任务
3.  等待另一个宏任务出现在任务队列中
4.  从步骤 1 开始重复

注意，我们任务队列中的第一个宏任务实际上是整个脚本本身。例如:

步骤 1:运行队列中的第一个宏任务。在这种情况下，脚本本身就是宏任务。这看起来有点像:执行`console.log("Horse C")`，0 毫秒后将`setTimeout`排入宏任务队列，将`promise`排入微任务队列。

第二步:运行微任务队列中的所有内容。在我们的例子中，这是`promise`的`.then`方法中的函数，即:`() => console.log("Horse B")`。

步骤 3:看到宏任务队列中的第一个任务是`setTimeout`。

步骤 4:重复步骤 1(运行现在从任务队列中出列的`setTimeout`方法)

# 第六场

```
setTimeout(() => console.log("Horse A"), 5)const promise = new Promise((resolve) => {
   setTimeout(() => resolve(), 10) 
})
promise.then(() => console.log("Horse B"))console.log("Horse C")
```

**结果**

1.  马 C
2.  马 A
3.  马 B

**解释**

如果您弄错了，可能是因为您记得微任务队列优先于宏任务队列，也就是说，您认为马 B 将在马 A 之前完成，因为马 B 与一个承诺相关联。但是，请注意`setTimeout`在解决承诺之前 5ms 结束。这意味着当事件循环去检查微任务队列(步骤 2)时，它将一无所获(持续 10ms ),并在 5ms 后转移到宏任务队列，该队列上有`setTimeout`方法。

# 决赛

```
setTimeout(() => console.log("Horse A"), 0);const promise = fetch("someUrl") // takes 100ms
promise.then(x => console.log("Horse B"));waitFor200ms();
console.log("Horse C");
```

**结果**

1.  马 C
2.  马 B
3.  马 A

**解说**

如果你答对了，你就是真正的神。这是一个受 Will Sentance(CodeSmith 的 CEO 和 JavaScript:Hard Parts 的讲师)启发的例子，我第一次看到它时就弄错了。

乍一看，很像第六场比赛。唯一真正的区别是增加了`waitFor200ms`函数，理论上它作为同步函数在主线程上运行 200 毫秒。

这次马 B 在马 A 之前完成的原因是，当在调用栈上执行`waitFor200ms`函数时，我们的`fetch`调用(花费 100 毫秒)已经被解析，并因此在当前一轮事件循环中被推到微任务队列上，即，在我们到达上述算法中的步骤 2 之前。然后，事件循环将执行微任务队列中的所有任务，然后移动到最新的宏任务，得到结果。

# 有奖竞赛

```
async function someFn() {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Horse A"), 1000)
  });

  let result = await promise; 
  console.log(result); 
}setTimeout(console.log("Horse B"), 0)
someFn();
console.log("Horse C");
```

**结果**

1.  马 B
2.  马 C
3.  马 A

**解释**

啊，我骗了你！注意，`setTimeout`方法是直接执行`console.log("Horse B")`，而不是采用回调函数(即`() => console.log("Horse B")`)。我想我会把这个放在这里，因为我经常看到一些开发人员(包括我自己)犯这个小错误。

最后，`async/await`会让事情变得更好。可以理解，你可能会认为我们将在记录马 C 之前“等待”马 A 完成，但是同样地，**同步代码将总是获得优先级**。

# 结论

恭喜你，你让我的唠叨结束了！如果你猜对了所有比赛的结果，那么恭喜你，你是一个比我好得多的开发者！

无论如何，我希望你从这篇文章中学到了一些东西，或者至少练习了你的 JavaScript 技能。感谢您的阅读！

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**