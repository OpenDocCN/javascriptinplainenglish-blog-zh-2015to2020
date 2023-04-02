# 3 个最常见的实习生编码挑战问题以及如何解决它们

> 原文：<https://javascript.plainenglish.io/the-3-most-common-intern-coding-challenge-questions-and-how-to-solve-them-25be4f20abe6?source=collection_archive---------1----------------------->

![](img/19d4b099d4a4d966a130c198bd4d3c22.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

视频教程:

那么，你已经得到了你梦想中的暑期实习的面试机会，太棒了！招聘人员只是告诉你期待编码挑战/技术面试(有时是“技术筛选”)。

无论是白板式面试，你可以直接与其他工程师交谈，还是简单的黑客挑战，做好准备都是一个好主意。

如果这是你第一次面试，不要惊慌。很可能，你已经在你的课程中遇到过类似的编码问题。这只是一个微调你解决问题的技巧和语言特定语法的问题。

通常，面试官会让你选择你想用哪种语言来解决问题，并会给你一个三到七种语言的列表供你选择。想想——Java、JavaScript、Python、C++、C 和 Ruby——你的体验会有所不同。此外，如果他们在中提出 SQL、JSON 解析或数据库架构问题，也不要感到惊讶。

如果你在进行白板式面试，你的面试官可能更关心你如何表达你的问题和解决方案，而不是你的答案有多“闪亮”。

这是我在实习和面试实习生时见过的最常见的三个编码挑战问题。我将用伪代码回答每个问题(白板面试官可能会要求你这样做)，然后用 JavaScript 给出实际的解决方案。我喜欢使用 JavaScript，因为这种语言提供了大量的[内置对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)，可以让你走一些很棒的捷径。我喜欢这样。

# 1.检查一个字符串是否是回文(即。反转字符串)

> 假设你得到一个字符串数组，arr，对于每个字符串，我们想确定它是否是一个回文。为每一项返回“真”或“假”。

## 伪代码:

所以我们得到了下面的数组:

```
arr = [“The quick brown fox jumped over the sleeping dog",
       "Oozy rat in a sanitary zoo",
       "Carla loves chocolate"]
```

我们首先需要一个 for()或 forEach()循环来接触数组中的每一项。

在每次迭代中，我们需要对整个字符串 str 使用 toLowercase()来匹配大小写。这一点很重要，因为“赛车”和反过来的“赛车”不一样。

接下来，需要从字符串 str 中删除空格。这可以通过。replace()内置对象。

现在，我们需要创建一个新的字符串，newStr，它等于我们的 edited 的值(小写，没有空格)，但是取反了。这可以通过。reverse()内置对象。

最后，比较 str 和 newStr 的值。如果相等，则返回 True，否则返回 False。

## **代号**:

```
function palindromeChecker(arr){ arr.forEach(element => { element = element.toLowerCase();
  element = element.replace(/\s/g, ‘’);
  newElement = element.split(‘’).reverse().join(‘’); if(newElement == element){
    console.log(“it is a palindrome”)
    return true
  }
  else{
    console.log(“it is not a palindrome”)
    return false
  }
 });
}arr = [“The quick brown fox jumped over the sleeping dog”,”Oozy rat in a sanitary zoo”,”Carla loves chocolate”]palindromeChecker(arr)
```

# 2.查找数组中最频繁出现的项目

> 假设给你一个整数和字符的数组，找出哪个整数/字符出现的最频繁。

## 伪代码:

假设我们有以下数组:

```
arr1 = [2, 'b', 1, 'a', 2, 6, 'a', 3, 'b', 2, 9, 3,2];
```

我们需要找到数组中最频繁出现的项，我们可以通过两个嵌套的 For 循环来实现。

第一个 For 循环将遍历数组中的每一项并获取其值。接下来，我们将需要另一个循环(在第一个循环中)来比较数组中的第一项和后续项，并查看它等于数组中未来项的次数。

## 代码:

```
function mostFrequent(arr){
  var mf = 1; //most frequent occurrence count (count)
  var m = 0; // current count
  var item; for (var i = 0; i < arr.length; i++) {
    for (var j = i; j < arr.length; j++) {
      if (arr[i] == arr[j]){
        m++;
      }
      if (mf < m){
        mf = m;
      }
    item = arr[i];
    }
   }
   m = 0;
  }console.log(item + " "+ mf + " times");}arr = [2, 'b', 1, 'a', 2, 6, 'a', 3, 'b', 2, 9, 3,2];mostFrequent(arr)
```

# 3.使用递归打印斐波那契数列

> 给定一个数字 n，打印斐波那契数列的第 n 个元素。如果 n = 5，打印第五个斐波那契数。

## 伪代码:

斐波纳契数列中前两个数字之后的每个数字都是前两个数字之和。

斐波那契数列看起来像:1，1，2，3，5，8，13，21，34…

所以，为了递归地做这件事，我们需要这样想:

因此，假设 n=5:

```
Step 1: fibonacci(4) + fibonacci(3) 
Step 2: (fibonacci(3) + fibonacci(2)) + (fibonacci(2) + fibonacci(1)) 
Step 3: ((fibonacci(2) + fibonacci(1)) + 1) + (1 + 1) 
Step 4: ((1 + 1) + 1) + (1 + 1)Answer = 5
```

## 代码:

```
function fibonacci(n) {     if (n <= 2) return 1; 
    return fibonacci(n — 1) + fibonacci(n — 2);
}n=5console.log(fibonacci(n))
```

# 结论

祝你面试好运！好好学习，你会做得很好的！

## **简明英语团队的一份说明**

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****