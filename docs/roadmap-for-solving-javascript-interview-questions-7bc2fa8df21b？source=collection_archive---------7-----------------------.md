# 解决 JavaScript 面试问题的路线图

> 原文：<https://javascript.plainenglish.io/roadmap-for-solving-javascript-interview-questions-7bc2fa8df21b?source=collection_archive---------7----------------------->

## 一个简单的模板，帮助您浏览代码挑战和其他面试问题

![](img/81ff48ee986625dc2c479a19b1e24332.png)

Photo by [Fabian Grohs](https://unsplash.com/@grohsfabian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

大多数编码挑战文章都会提出一个问题，并向您展示如何解决它。如果您已经很好地掌握了您正在使用的语言和基本编程概念的知识，这是非常好的。然而，开始会很困难，如果你以前没有经验，制定一个解决问题的计划本身就是一项巨大的任务。我想创作的内容不仅向你展示如何解决一个问题，而且在你着手解决一个新问题时，给你一个模板来形成一个计划。一个伟大的程序员最重要的技能之一就是解决问题。难的是没有很多资源可以教你如何成为一个更好的问题解决者。为了提高你解决问题的技巧，你需要在写一行代码之前制定一个计划。这篇文章将帮助你把一个问题分解成四个不同的部分。问题描述、头脑风暴、代码——sudo 代码和你的实际代码，然后总结你是如何解决问题的。这将给你一个很好的技巧来解决任何编码挑战或面试问题，我希望你能从中得到乐趣！我还想包括一个比我想出的更好的解决方案，以显示解决一个问题有许多不同的方法。

# 问题描述

数字`89`是第一个超过一位数的整数，它满足了这个形的标题中部分介绍的属性。说“尤里卡”有什么用？因为这个和给出的是同一个数。

生效日期:`89 = 8^1 + 9^2`

拥有这份财产的下一个号码是`135`。

再看这个属性:`135 = 1^1 + 3^2 + 5^3`

我们需要一个函数来收集这些数字，该函数可以接收两个整数`a`、`b`，这两个整数定义了范围`[a, b]`(包含在内)，并输出满足上述属性的范围内的已排序数字的列表。

我们来看一些案例:

```
sumDigPow(1, 10) == [1, 2, 3, 4, 5, 6, 7, 8, 9]
// 1^1, 2^1sumDigPow(1, 100) == [1, 2, 3, 4, 5, 6, 7, 8, 9, 89]
```

如果在范围[a，b]中没有这种数字，函数应该输出一个空列表。

```
sumDigPow(90, 100) == []
```

# 我是怎么解决的。

1.  首先，我创建了一个 numberArray 来保存我所有的最终值
2.  然后，我创建了一个 for 循环，初始值设置为“a”，并在整个范围内循环，直到“I”小于或等于“b”。
3.  然后，我创建了一个条件来查看这个数字是否需要分成两个独立的数字。
4.  如果这个数字大于或等于 10，我就把它分成几个独立的数字。
5.  然后我创建了一个新的数组来保存我所有的分割数字
6.  然后，我遍历 splitDigit 数组并使用 Math.pow()将数组中的第一个数字设置为 1 的幂，然后将数组中的第二个数字设置为 2 的幂，依此类推。
7.  然后，我使用 array.reduce 找到数组的和，如果和等于 I，我就把它和其余的值一起放入数字数组
8.  如果数字小于 10，我就使用 1 的幂的 Math.pow()来查看数字是否等于 I。
9.  如果数字相等，我把它放入数字数组
10.  然后我返回数字数组。

# 我的代码

```
**Sudo Code** // get both integers 
// a = a, b = b
// (1,10)
// for(var i = a; i < b; ++i) {
//    var numberArray = [];
//    if integer > 1 split the integer
//       1 > 1 no
//       1 times power of 1
//       1^1 = 1 yes
//       push 1 into numberArray
//        
//       10 > 1 yes...
//       split 10 into two digits
//       digitArray = [];
//       sumArray = [];
//         digitArray = [1,0]
//       1 and 0
//       for(var i = 0;  i < digitArray.length; ++i) {
//          digitArray[i]^i+1 
//          1^1 = 1 and 0^2 = 0 
//          push digits into sum array
//          then add the two digits from sum array
//          sumArray = [1,0]
//          sumArray.reduce ? === 1
//          check does 1 === 10 no
//          don't push 10
//        finally return numberArray
//       }
//        }**Code**function splitToDigit(n){
  return [...n + ''].map(Number)
}function sumDigPow(a, b) {

  var numberArray = [];

  for(var i = a; i <= b; ++i) {
    if(i >= 10) {

      var splitDigit = splitToDigit(i);

      var newDigit = [];

      for(var j = 1; j < splitDigit.length + 1; ++j) {
        newDigit.push(Math.pow(splitDigit[j-1], j))
      }
          if(newDigit.reduce((a, b) => a + b, 0) === i){
            numberArray.push(i)
          }
      }
    if(i < 10){
      if(Math.pow(i, 1) === i)
      numberArray.push(i)
    }
  }
  return numberArray;
}console.log(sumDigPow(1,100));
```

# 我是怎么解决的。

1.  首先，我创建了一个 numberArray 来保存我所有的最终值
2.  然后，我创建了一个 for 循环，初始值设置为“a”，并在整个范围内循环，直到“I”小于或等于“b”。
3.  然后，我创建了一个条件来查看这个数字是否需要分成两个独立的数字。
4.  如果这个数字大于或等于 10，我就把它分成几个独立的数字。
5.  然后我创建了一个新的数组来保存我所有的分割数字
6.  然后，我遍历 splitDigit 数组并使用 Math.pow()将数组中的第一个数字设置为 1 的幂，然后将数组中的第二个数字设置为 2 的幂，依此类推。
7.  然后，我使用 array.reduce 找到数组的和，如果和等于 I，我就把它和其余的值一起放入数字数组
8.  如果数字小于 10，我就使用 1 的幂的 Math.pow()来查看数字是否等于 I。
9.  如果数字相等，我把它放入数字数组
10.  然后我返回数字数组。

# 最佳解决方案

```
function sumDigPow(a, b) {
  var ans = [];
  while(a <= b){
    if(a.toString().split('').reduce((x,y,i)=>x + +y ** (i + 1),0) == a)
      ans.push(a);
    a++;
  }
  return ans;
}
```

感谢阅读！我希望这篇文章能让你很好地理解如何真正地分解一个问题。一个好的进攻计划是解决任何问题的关键。非常感谢您的反馈！

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****