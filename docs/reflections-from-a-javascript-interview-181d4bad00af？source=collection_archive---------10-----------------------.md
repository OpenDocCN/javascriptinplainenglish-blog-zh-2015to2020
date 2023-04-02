# JavaScript 采访的反思

> 原文：<https://javascript.plainenglish.io/reflections-from-a-javascript-interview-181d4bad00af?source=collection_archive---------10----------------------->

![](img/e8b1aac2ac4d9dffbc27209fc2d4444b.png)

Photo by [Steve Halama](https://unsplash.com/@steve3p_0?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我已经接受了一些关于 JavaScript 的采访。在我等待回音和评估的时候，我想写下我学到的东西和一些提示，给那些期待 JavaScript 面试的人。

**技术问题**

我有几个技术问题要和大家分享。这是我被要求解决的问题之一(我记得)。

```
Take a string such as "supercalifragilisticexpialidocious" and return an object that holds the letter and the frequency of each letter.For example, if the string = "puppy" the returned object would be
    obj = {
       p: 3,
       u: 1,
       y: 1
    }const string = "supercalifragilisticexpialidocious"function createObj(string){
     const obj = {}
     const array = string.split("")
     array.forEach(letter => {
          if (obj[letter]){
             obj[letter]++
          } else {
             obj[letter] = 1
          }
     })
    return obj
}createObj(string)
```

> [链接到回复](https://repl.it/@KelseyShiba/JavascriptObjectFromString#index.js)

**倒影:**

*大声说话*

在这种情况下，面试官真的提供了很多信息。当我创建解决方案时，我大声说出来，我认为这也很重要。我最初没有从 forEach 迭代器开始。我让面试官知道，我的直觉告诉我“forEach”会起作用，但如果我要用蛮力求解，我会用“for”循环。他立即附和说，forEach 是正确的方式，所以我继续下去。

我真的很关注最终的结果。我带着紧张进入这个问题，但我知道我需要归还一些东西。我会把这一点记在笔记里，然后向面试官重复一遍。

```
//function takes a string
//returns an OBJECT
//constraints: word would not exceed 1,000 letters
```

*开始&结束*

然后我会写下我的开始和结束，并构思出解决方案:

```
function createObj(string) {
     const obj = {}
     return obj
}
```

*虚心学习新信息*

我的采访者还提到这段代码有一个问题，因为我们没有先检查对象是否已经有了字母。

```
if (obj[letter]) 
```

我的面试官向我介绍了一种新的 JavaScript 对象方法——hasOwnProperty。定义[此处](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)。如果对象具有该属性(true)或不具有该属性(false)，则返回布尔值。下面是被利用的重构代码。

```
const string = "supercalifragilisticexpialidocious"function createObj (string){
     const obj = {}
     const array = string.split("")
     array.forEach(letter => {
          if (obj.hasOwnProperty(letter)){
              obj[letter]++
          } else {
             obj[letter] = 1
          }
     })
    return obj
}createObj(string)
```

这就像圣诞节得到一辆新自行车。我甚至不知道这一点的存在，我准备并愿意从中学习。太棒了。书呆子气——但很棒。

**另一个简短的技术示例**

我被要求为这个函数调用编写函数:

```
multiply(4)(5)
```

所以——我一边自言自语一边写了出来:

```
//needs to return the integer 20 when called
//function name needs to be multiply
//appears to be a double function needed
```

写了我的开始和结束(它需要什么和它返回什么)

```
function multiply(num1){
     return num1 * num2
}
```

然后加入逻辑，因为我知道为了调用第二个数字，我需要另一个函数和另一个参数。

```
function multiply(num1){
     (num2) => {
         return num1 * num2
     }
}
```

然而，我错过了一件经常被遗忘的事情。为了包含第二个参数，我需要先返回一个函数。代码如下:

```
function multiply(num1){
     return (num2) => {
        return num1 * num2
     }
}multiply(4)(5)
```

**倒影:**

*重构！*

别忘了在*之后重构*。箭头函数有隐式返回，所以我可以把这个函数做得更短:

```
function multiply(num1){
     return (num2) => num1*num2
}
```

然而，我需要看到两个回报，以确保我明白发生了什么引擎盖下。

**不要跳过基本面**

我一直在努力研究算法，创建自己的投资组合，以至于忘了回去做一些研究。我被问到一些非常基本的问题，但我无法在面试时用一种令我满意的方式表达出来。

其中一个问题是:**“ES6 在 JavaScript 中代表什么？”**

我真的不知道。我知道今年是 2015 年，还有一年即将到来。我猜“S”代表“语法”，6 代表版本。事实证明，ES 是 ECMAScript，而且是第 6 版。

让它渗透一段时间后，我意识到这是一个多么有趣的面试问题。在我未来的工作中，我真的会在日常工作中被问到这个问题吗？也许——不太可能。然而，这表明我已经足够好奇，想要了解 JavaScript。接触过它或者想知道它是如何工作的，它从何而来。这似乎更像是一个性格问题，而不是技术问题。

另一个问题:**类和原型继承有什么区别？**

我有点明白这一点，但我不能清楚地表达出来。我觉得在面试的这一部分我有点昏过去了。关键是——我后来为此痛苦不堪。这里有一个关于它的很棒的博客。

另一个问题:**箭头函数有什么特别/独特之处？**

我记得 arrow 函数的几个具体用例，它经常与 JavaScript 中的 DOM 事件一起使用，因为它可以将关键字 *this* 绑定到创建该函数的任何对象。我忘记了箭头函数也有隐式回报——我认为这是向我的面试官强调的重要一点。

技术问题:**如果我在一个数组中存储了 100 个整数，那么返回一个没有重复的有序数组的最佳方式是什么？这个问题也是以口头回答的形式提出来的，而不是用代码写出来的。**

我对这件事有点僵住了。我提到了内置函数排序，但是让它唯一或者没有重复有点让我犹豫了。以下是更多相关信息。

**倒影:**

*打开一个代码区*

这些问题很多都不是来自正式的技术面试，更多的是“了解你”面试。我也是一个视觉型的人，所以当我试图在阵列上工作来回答问题时，有一个打开的编码沙箱来工作是很好的，这样我就可以通过我的答案来说话。

*了解 ES6 内置功能*

当考虑创建没有重复的数组时，有很多方法可以处理。然而，在所有这些关注 ES6 的问题之后，我意识到可能有一个答案可以展示我的 ES6 能力和知识。ES6 中有一个 [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) 对象，它返回一个唯一的集合(没有重复)。因此，对于最后一个技术问题，我可以使用或尝试这样的方法:

```
const array = [1,5,4,2,1,1]function sortNoDups(array) {
     const newArr = array.sort()
     return new Set(newArr)
}sortNoDups(array)
```

*使用和学习专业术语*

在面试中，我努力用正确的术语来描述事情，这是我想改进的地方。我认为有些描述比其他的更有效，当然，这取决于你的个人喜好。例如，当回答关于 JavaScript 中类和原型之间的区别的问题时，我可以学习和练习说这样的话，“类就像是对象和方法的蓝图，用于创建对象。”

或者关于 JavaScript 中的*这个*——我真的很难找到合适的词来形容它。但是，它似乎最常被称为“这个关键字”，我可以用它来扩展我的能力，让我清楚地表达它是做什么的，以及它是如何工作的。

在讨论 arrow 函数时，像“隐式返回”这样的其他术语也很方便，但是我想让我能够马上谈论这样的功能。

## **总结**

我希望你接受这篇文章，并把它用在你的优势上。我从面试中学到了很多，我很感激我获得的所有经验。祝你们所有人好运。

代码打开！~凯尔西