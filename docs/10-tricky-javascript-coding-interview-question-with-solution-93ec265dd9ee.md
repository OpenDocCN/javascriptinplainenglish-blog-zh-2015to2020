# 10 个棘手的 JavaScript 编码面试问题

> 原文：<https://javascript.plainenglish.io/10-tricky-javascript-coding-interview-question-with-solution-93ec265dd9ee?source=collection_archive---------0----------------------->

![](img/45f1270cc615e1149f8ca7b66fd06660.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你将要面对的编码面试中的一些棘手问题。这些问题看起来很简单，但其中有猫腻。所以今天我将向你展示 10 个你应该在去编码面试前看到的棘手问题。

## 1.给定一个字符串，颠倒句子中的每个单词

```
var string = "Welcome to this Javascript Guide!";

// Output becomes !ediuG tpircsavaJ siht ot emocleW
var reverseEntireSentence = reverseBySeparator(string, "");

// Output becomes emocleW ot siht tpircsavaJ !ediuG
var reverseEachWord = reverseBySeparator(reverseEntireSentence, " ");

function reverseBySeparator(string, separator) {
  return string.split(separator).reverse().join(separator);
}
```

## 2.JavaScript 中如何清空数组？

```
var arrayList =  ['a', 'b', 'c', 'd', 'e', 'f'];
```

## 方法 1

```
arrayList = [];
```

上面的代码将变量`arrayList`设置为一个新的空数组。如果您在其他地方没有对原始数组 `arrayList`的**引用，那么建议这样做，因为它实际上会创建一个新的空数组。**

## 方法 2

```
arrayList.length = 0;
```

上述代码将通过将现有数组的长度设置为 0 来清除该数组。这种清空数组的方式也更新了所有指向原数组的引用变量。

## 方法 3

```
arrayList.splice(0, arrayList.length);
```

上面的实现也将完美地工作。这种清空数组的方式也将更新原始数组的所有引用。

## 3.你如何检查一个数字是否是整数？

检查一个数是小数还是整数的一个非常简单的方法是看你除以 1 时是否还有余数。

```
function isInt(num) {
  return num % 1 === 0;
}

console.log(isInt(4)); // true
console.log(isInt(12.2)); // false
console.log(isInt(0.3)); // false
```

## 4.解释什么是回调函数，并提供一个简单的例子。

`callback`函数是作为参数传递给另一个函数的函数，在某个操作完成后执行。下面是一个简单的回调函数的例子，它在完成一些操作后记录到控制台*。*

```
function modifyArray(arr, callback) {
  // do something to arr here
  arr.push(100);
  // then execute the callback function that was passed
  callback();
}

var arr = [1, 2, 3, 4, 5];

modifyArray(arr, function() {
  console.log("array has been modified", arr);
});
```

## 5.给定两个字符串，如果它们是彼此的变位组合，则返回 true

一个**字符串**的**变位词**是包含相同字符的另一个**字符串**，只是字符的顺序不同。例如，“abcd”和“dabc”是彼此的**变位词**。

```
var firstWord = "Mary";
var secondWord = "Army";

isAnagram(firstWord, secondWord); // true

function isAnagram(first, second) {
  // For case insensitivity, change both words to lowercase.
  var a = first.toLowerCase();
  var b = second.toLowerCase();

  // Sort the strings, and join the resulting array to a string. Compare the results
  a = a.split("").sort().join("");
  b = b.split("").sort().join("");

  return a === b;
}
```

## 6.以下代码的输出会是什么？

```
var y = 1;
if (function f() {}) {
  y += typeof f;
}
console.log(y);
```

上面的代码会给出输出`1undefined`。If 条件语句使用`eval` so `eval(function f() {})`求值，后者返回`function f() {}`true，所以在 if 语句代码内执行。`typeof f`返回未定义，因为如果语句代码在运行时执行，那么`if`条件内的语句在运行时求值。

## 7.下面的代码会输出什么？

```
(function() {
  var a = b = 5;
})();

console.log(b);
```

上面的代码将输出 5，尽管看起来变量是在函数中声明的，不能在函数外访问。这是因为

```
var a = b = 5;
```

是这样解释的:

```
var a = b;
b = 5;
```

但是在带有`var`的函数中没有声明`b`，所以在*全局作用域*中它被设置为等于 5。

## 8.下面的代码会输出什么？

```
for (var i = 0; i < 4; i++) {
  setTimeout(() => console.log(i), 0)
}
```

这里的经典陷阱是**零延迟**。`setTimeout(callback, 0)`并不意味着回调会在零毫秒后被触发。

事件循环端发生的情况如下:

1.  当前调用堆栈被设置为第一个 setTimeout()。
2.  windows.setTimeout()被认为是一个 Web APIs(为了更好的**非阻塞 I/O** )。因此调用堆栈将这部分代码发送给正确的 Web APIs。0 毫秒后，回调(这里是一个匿名函数)将被发送到队列(而不是调用堆栈)。
3.  由于调用堆栈是空闲的，for-loop 可以继续到第二个 setTimeout …(在我们满足这个条件后重复 i < 4)…
4.  Now the loop is over and 【 . JS can now execute the callback queue one by one. Each console.log(i) will print the 4.

## 9\. Palindrome Problem

A palindrome is a word, sentence, or other types of character sequence that reads the same backward as forward. For example, “racecar” and “Anna” are palindromes. “Table” and “John” aren’t palindromes, because they don’t read the same from left to right and from right to left.

```
const palindrome = str => {
  // turn the string to lowercase
  str = str.toLowerCase()
  // reverse input string and return the result of the
  // comparisong
  return str === str.split('').reverse().join('')
}
```

## 10\. Find the Vowels

This is probably one of the less challenging challenges (no pun intended) — in terms of difficulty — but that doesn’t detract from the fact that you could come across it during a job interview. It goes like this.

```
const findVowels = str => {
  let count = 0
  const vowels = ['a', 'e', 'i', 'o', 'u']
  for(let char of str.toLowerCase()) {
    if(vowels.includes(char)) {
      count++
    }
  }
  return count
}
```

That’s all for today. Thanks everyone for reading it till the end. Keep learning :)

# **来自简单英语团队的注释**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****