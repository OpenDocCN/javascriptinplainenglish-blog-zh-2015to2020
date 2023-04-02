# 解决回文算法问题的最佳(也是最差)方法

> 原文：<https://javascript.plainenglish.io/the-best-and-worst-way-of-solving-the-palindrome-question-4b7d2f9ada06?source=collection_archive---------10----------------------->

![](img/009d4daa61acae289efaa4a8d8038642.png)

Some silly palindromes

我最近参加了一些技术面试，问了一些不同的问题，或者实际上包括回文问题。这就是为什么我认为有必要介绍一下我是如何学会解决这个面试问题的，希望有人可以通过阅读我是如何做的来了解如何解决这个问题。

据我所知，有 4 种方法可以解决这个问题，但当我提到最好或最差的方法时，我指的是在时间和空间复杂度上最好和最差的 big-O 符号。我还应该指出，我将在 JavaScript 中解决这个问题。

# 问题是

一个典型的字符串操作问题，回文问题指出，如果给定一个不为空的字符串，编写一个函数来确定单词是否前后拼写相同。

确定这一点的最终输出可能是真或假，因为 JavaScript 中的任何东西都有真或假的值。然而，这并不意味着每次都必须返回布尔值 true 或 false。换句话说，如果您试图测试一个函数中的 if 条件，说它需要返回某种特定的答案，但作为参数给出的输入不可能返回该答案，那么如果在 JavaScript 控制台中测试，它将返回 false。

# 最糟糕的方法——制作一个新的反向字符串

**方法总结:**最糟糕的方法是创建一个新字符串，从原始字符串开始反向遍历每个字母，将每个字母反向放置，然后比较两个字符串。

**如何求解:**我们将从定义函数的名字开始，这个函数确定一个字符串是否是回文，因此它被相应地命名。每次调用函数时，我们的输入都是字符串。

```
function isPalindrome(string) {
```

接下来，我们将为最终反转的字符串设置一个变量:

```
const reversedString = ‘’
```

现在，为了遍历字符串本身以连接反转的字符串，我们将创建一个 for 循环，但它将从字符串的最后一个字母开始(让 i = string.length -1)，然后它将继续前进，直到到达输入的第一个字母(i>=0)，I 代表每个索引，它将在每次运行时减少(i -)。

```
for (let i = string.length -1; i>=0; i — ){
```

现在，为了将内容放入循环中，我使用+=操作符将每个字母连接到我的空字符串中，这将表示我的第一个字符串向后。String[i]代表每个字母，记住 reversedString += string[i]与 reversed string = reversed string+string[I]相同是很重要的。

```
 reversedString += string[i];
 }
```

下一行将返回一个真值，其中原始字符串与反转后的字符串相同。

```
 return string === reversedString
}
```

总的来说，它看起来如下:

```
function isPalindrome(string) {
 const reversedString = ‘’
 for (let i = string.length -1; i>=0; i — ){
 reversedString += string[i];
 }
 return string === reversedString
}
```

这个解决方案不是最好的地方是，它有一个时间复杂度为 O(n^2 的大 o 符号，在图表上不是最好的，而空间复杂度为 O(n)，非常好。然而，这与第二个解决方案相比，清楚地显示了另一个解决方案具有更好的时间和空间复杂性，我将很快进入。重要的是，这个解决方案的时间复杂度很高，主要是因为程序需要创建一个全新的字符串，这需要更长的时间。

# 最佳方法——使用指针系统比较最左侧和最右侧

**方法总结:**另一方面，解决这个问题的最好方法是定义一个左右指针，并比较每个指针所指向的字母。然后使用 while 循环，当右指针在右边，而左指针在左边时，如果在任何时间点指针指向的字母不相同，则返回 false。否则，返回 true。

**如何解决:**要开始解决这个问题，它以与上一个相同的方式开始，建立一个函数，我们选择一个适当的名称，输入仍然是给定的字符串。

```
function isPalindrome(string) {
```

然而这次我们将设置两个变量来表示左右指针。左指针将表示从 0 开始的字符串的第一个索引。为了补充左边的，我们设置了一个右指针，等于字符串的长度-1，代表右边最远的数字，或者最后一个索引。

```
let leftPointer = 0
let rightPointer = string.length-1
```

现在使用 while 循环，我们会说，当左指针相对于右指针位于左侧时，我们希望运行下面的逻辑。

```
while (leftPointer < rightPointer){
```

我们的逻辑将包含一个 if 条件，如果第一次循环时左字母与右字母不同，则返回 false。这意味着如果我们的字符串是“abcbz ”,如果“a”与“z”不同，那么它将在这一点上返回 false。

```
if(string[leftPointer] !== string[rightPointer]) return false;
```

假设我们的字符串是" abcba"。两端的字母“a”和字母“a”是相同的，因此它将通过返回错误行区域，并转到下一行，该行将根据它是什么而递增或递减。如果它是一个左指针，它将更多地向右移动，反之亦然，这样指针就可以比较字符串的两边。

```
leftPointer++;
 rightPointer — ;
 }
```

现在，一旦指针位于中间的同一点，程序将跳出 while 循环，并运行到 return true 语句，因为这将表明它从未使两边不相同的 if 条件为真。这意味着这是一个回文。

```
return true
}
```

该解决方案的最终结果如下所示:

```
function isPalindrome(string) {
 let leftPointer = 0
 let rightPointer = string.length-1

 while (leftPointer < rightPointer){
 if(string[leftPointer] !== string[rightPointer]) return false;
 leftPointer++;
 rightPointer — ;
 }
 return true
}
```

最终，我们得到的时间复杂度为 O(n ),空间复杂度为 O(1)。为什么这种方法有更好的时间和空间复杂度？这是因为它不需要创建一个新的字符串，程序所要做的只是使用一些简单的指针来处理相同大小的输入，并最终返回 true 或 false，这比第一个解决方案花费的时间要少。

回文问题很好地测试了开发人员对如何操作字符串的基本理解。这是一个很容易解决的问题，有更清晰的机会去理解 big-O 符号背后的原因，开发者可以在以后更复杂的问题中使用这些符号。我希望这篇更短的博客足够简单，可以帮助你理解如何通过最终分解每个解决方案的各个部分来解决这个问题，并对如何给出 big-O 符号给出一些见解。下次见！