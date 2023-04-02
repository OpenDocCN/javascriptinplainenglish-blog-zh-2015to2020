# JavaScript 时间复杂度实验

> 原文：<https://javascript.plainenglish.io/experiments-with-time-complexity-in-javascript-56643e20ca12?source=collection_archive---------6----------------------->

![](img/8e933ec624609b933617a3b338138423.png)

Photo by [Icons8 Team](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/timer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你像我一样，在通过非传统途径找到这个领域后，正在面试你的第一个软件工程角色，你很快就会达到需要对大 O 符号和性能有坚实理解的地步。使用大 O 符号，理解会深入许多层。有对“哦，是的，就是那个有很多 O 和 n 的”的表层理解，更深层次的学术理解，以及对 Big O 中提出的概念如何影响你的代码的更具体的理解。

如果你正在寻找对大 O 和时间复杂性的更深入的学术理解，这不是你想要的文章。有很多精彩的资源可以涵盖这个主题。在本文中，我将提供一个简单的 DIY 实验，帮助我更具体地理解如何使用 performance.now()对不同输入大小的代码进行计时来测量性能。

[面试蛋糕](https://www.interviewcake.com/article/java/big-o-notation-time-and-space-complexity)给出了大 O 批注的伟大定义:

“使用大 O 表示法，我们将运行时表示为，当输入变得任意大时，它相对于输入增长得有多快。”

我喜欢面试蛋糕对大 O 的解释的原因是，它也告诫我们在分析业绩时不要只看大 O。

“大 O 忽略常数，但有时常数很重要。如果我们有一个运行时间为 5 小时的脚本，将运行时间除以 5 的优化可能不会影响 big O，但它仍然会为您节省 4 小时的等待时间……一个伟大的工程师(启动或其他)知道如何在运行时间、空间、实现时间、可维护性和可读性之间取得正确的平衡。”

既然您已经有了定义并链接到一个很棒的资源来了解更多，我们如何在实际代码上运行实验来测试这些概念呢？这就是 performance.now()在使用 JavaScript 时发挥作用的地方。使用 performance.now()，我们可以创建一个简单的计时器来测量代码执行需要多少毫秒。

首先，让我们看看基本设置:

```
//make sure you're able to access performance.now()
const {performance} = require('perf_hooks');//write out your function
const functionName = () => {
   //code goes here}//time your function
const startTimer = performance.now();
functionName();
const endTimer = performance.now();//log how many milliseconds it took your function to execute
console.log(`This function took ${endTimer - startTimer} milliseconds`);
```

你可以把你的计时器的开始和结束放在任何地方，所以这也是测试你的函数的一个方便的方法，看看是什么拖了你的后腿。

让我们用一个实际的例子来实践这一点，使用两种不同的方法解决 HackerRank 的“中等”[级匹配字符串](https://www.hackerrank.com/challenges/sparse-arrays/problem?h_r=profile)问题。

HackerRank 的匹配字符串问题要求我们创建一个带有两个参数的函数:“Strings”，一个要搜索的字符串数组，和“queries”，一个查询数组。有了这两个参数，我们需要创建一个函数，返回一个整数数组，表示每个查询在 strings 数组中出现的频率。

我们可以使用嵌套循环来解决这个问题。嵌套循环可以(但不总是)在 O(n)时间或二次时间内运行。如果一个函数在二次时间内运行，传入 1 个输入，它将运行 10 次。如果传入 10 个输入，它将运行 100 次。你可以看到时间复杂度会变得非常快。

如果我使用嵌套循环解决了匹配字符串问题，我的解决方案可能如下所示:

```
const matchingStrings = (strings, queries) => {
  const results = new Array(queries.length).fill(0);
  for (let i = 0; i < strings.length; i++) {
    for (let j = 0; j < queries.length; j++) {
      if (strings[i] === queries[j]) {
        results.splice(j, 1, results[j] + 1)
      }
    }
  }
  return results;
};
```

现在，让我们稍微优化一下这个解决方案，将可能的字符串匹配存储在一个对象中，而不是使用嵌套循环，然后将我们的查询与对象键进行比较，并将值(表示查询在字符串数组中出现的次数)推入结果数组。

```
const fasterMatchingStrings = (strings, queries) => { 
  const condensedStrings = {};
  const results = [];
  for (let string of strings) {
    condensedStrings[string] = condensedStrings[string] + 1 || 1;
  }
  for (let query of queries) {
    if (condensedStrings.hasOwnProperty(query)) {
      results.push(condensedStrings[query]);
    } else {
      results.push(0);
    }
  }
  return results;
};
```

现在，使用我们计时器的时候到了。首先，让我们来看看 HackerRank 使用的一个较小的测试。

```
//First attempt at Matching Strings
const startTime = performance.now();
matchingStrings(['def', 'de', 'fgh'], ['de', 'lmn', 'fgh']);
const endTime = performance.now();console.log(`Time to run was ${ endTime - startTime} milliseconds`);//Second attempt at Matching Strings
const fasterStartTime = performance.now();
fasterMatchingStrings(['def', 'de', 'fgh'], ['de', 'lmn', 'fgh']);
const fasterEndTime = performance.now();console.log(`Time to run was ${ fasterEndTime - fasterStartTime} milliseconds`);
```

如果我运行三次(在使用 performance.now()时，您不应该只依赖一个结果)，我会得到以下(四舍五入)结果。

**第一个匹配字符串解(ms):** 7.326，7.846，7.870

**第二匹配字符串解(ms):** 0.158，0.152，0.160

哇，差别真大！但是请记住，Big O 中的时间复杂度不是关于一个输入大小如何执行，而是关于当它变得任意大时，运行时相对于输入增长得有多快。让我们尝试一个稍微大一点的输入，看看这在实际代码中意味着什么。

```
//First attempt at Matching Strings
const startTime = performance.now();
matchingStrings(['def', 'de', 'fgh', 'aba', 'bb', 'tig', 'app', 'ghe', 'igh', 'ii'], ['de', 'lmn', 'fgh', 'ii', 'bbb', 'ba', 'tig', 'pa', 'pap']);
const endTime = performance.now();console.log(`Time to run was ${ endTime - startTime} milliseconds`);//Second attempt at Matching Strings
const fasterStartTime = performance.now();
fasterMatchingStrings(['def', 'de', 'fgh', 'aba', 'bb', 'tig', 'app', 'ghe', 'igh', 'ii'], ['de', 'lmn', 'fgh', 'ii', 'bbb', 'ba', 'tig', 'pa', 'pap']);
const fasterEndTime = performance.now();console.log(`Time to run was ${ fasterEndTime - fasterStartTime} milliseconds`);
```

如果我运行三次，我会得到:

**第一个匹配字符串解(ms):** 9.412，8.194，8.102

**第二个匹配字符串解(ms):** 0.271，0.256，0.232

第一个解决方案只增加了几个输入，就增加了大约 0.3 毫秒到 1 毫秒以上，而我们的第二个解决方案增加了大约 0.1 毫秒。想象一下，随着输入任意增大，这种增长差异会如何继续下去！

随着您进一步优化这个解决方案，并随着输入大小的增加继续计时您的函数，您将会看到“最慢”和“最快”函数之间的差异越来越大。这可能是一种有趣且令人满意的方式，有助于具体理解代码中的不同选择如何影响性能！