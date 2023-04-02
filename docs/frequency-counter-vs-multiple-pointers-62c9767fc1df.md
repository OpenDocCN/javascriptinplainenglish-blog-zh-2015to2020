# 频率计与多指针

> 原文：<https://javascript.plainenglish.io/frequency-counter-vs-multiple-pointers-62c9767fc1df?source=collection_archive---------8----------------------->

![](img/09a8d87c732d03e11c201084cfeffe22.png)

[https://www.fm-magazine.com/news/2019/jan/culture-of-blameless-problem-solving-201920306.html](https://www.fm-magazine.com/news/2019/jan/culture-of-blameless-problem-solving-201920306.html)

学习数据结构和算法是成为软件工程师的重要部分。简而言之，数据结构是我们组织数据的结构，而算法是执行任务的指令集。

在过去的一周里，我决定是时候深入这个话题，为未来的面试做准备了。我在 Udemy 上买了 [**柯尔特·斯蒂尔**](https://www.udemy.com/user/coltsteele/) 指导的 **JavaScript 算法和数据结构大师课**来帮我完成目标。您可以在此找到关于本课程[的更多信息。](https://www.udemy.com/course/js-algorithms-and-data-structures-masterclass/)

这不是我第一次尝试学习数据结构和算法。我总是被数学吓倒，发现自己在头 5 分钟内就放弃了。斯蒂尔在向初学者解释概念方面做得很好。他列出了一个坚实的解决问题的方法和几个解决问题的模式，我觉得这将有助于解决任何面试问题。我想回顾一下我发现非常有用并且这周一直在使用的两个模式。

## 频率计数器和多个指针

频率计数器的想法是使用一个对象来收集值/值的频率。这可以用来避免嵌套循环的需要。(O(n))。

多指针模式创建与索引或位置相对应的“指针”,并根据条件向开头、结尾或中间移动。对于多个指针，你需要确保你正在处理的任何数据集都是经过排序的。

我想分享一个简单的例子，说明我们如何使用这些模式中的任何一种来解决这个问题。

# 面试问题

**实现一个名为 checkForDuplicates 的函数，它接受可变数量的参数(任意数量的参数),并检查传入的参数中是否有重复的。**

## 多指针方法

1.  我喜欢做的第一件事是用我自己的话重述这个问题。对于这个例子，我可以这样重述:给定一个参数，检查是否有两个相同的参数。如果为真，则返回真，否则返回假。
2.  接下来，我想举一些具体的例子。

```
checkForDuplicates([1,4,7,7]) => true
checkForDuplicates(['a', 'b', 'c'])=>false
checkForDuplicates([2,2,4])=>true
```

3.在可视化一些可能的结果后，我开始伪代码。

```
//sort the given arguements . I prefer ascending orders
//create a starting point. Lets start at the beginning index of the data
//create a second pointer. For this example I will create it at index 1
//create a while loop that will run while the second pointer is less than the length of the arguement
//compare the two values that the pointers are currently pointing at. If these two values are the same, return true
//increment the 2 pointers until the condition is met
//if no pairs are equal, return false
```

4.写出我的思考过程后，我开始编码并解决问题

```
**function checkForDuplicates(...args){**//sort the given arguements . I prefer ascending order
**args.sort((a,b)=> a > b);**//create a starting point. Lets start at the beginning index of the data
//create a second pointer. For this example I will create it at index 1
**let start=0;
let next=1;**//create a while loop that will run while the second pointer is less than the length of the arguement
**while(next < args.length){**//compare the two values that the pointers are currently pointing at. If these two values are the same, return true
**if(args[start]===args[next]){
return true;
}**//increment the 2 pointers until the condition is met **start++
next++
}**//if no pairs are equal, return false
**return false;
}** //below is the solution without the pseduo comments **function checkForDuplicates(...args) {
  args.sort((a,b) => a > b);
  let start = 0;
  let next = 1;
  while(next < args.length){
    if(args[start] === args[next]){
        return true
    }
    start++
    next++
  }
  return false
}**
```

## 频率计数器方法

现在我将向您展示如何用不同的方法解决这个问题。类似于我如何用多个指针来处理问题，我用自己的话重新表述了问题，然后创造了一些具体的例子。由于我使用的是相同的问题，所以您可以查看我的思考过程和上面的步骤，了解我将如何首先处理这个问题。让我们直接进入这种方法的伪代码-

```
//create an empty object that will be used as a counter 
//loop through the set of data. If the data is not yet added to our object, add it and set it equal to a count of 1\. If the data is found in the object, increment the count by 1.
//loop through our created object to see if there are any values that are greater than 1.
//if any keys have a value greater than 1, return true. If no duplicates are found, return false
```

接下来让我们对这个解决方案进行编码-

```
**function checkForDuplicates(){**//create an empty object that will be used as a counter
**let counter={}**//loop through the set of data. If the data is not yet added to our object, add it and set it equal to a count of 1\. If the data is found in the object, increment the count by 1.
**for(let val in arguments){
counter[arguments[val]]= (counter[arguments[val]] || 0)
}**//loop through our created object to see if there are any values that are greater than 1.
**for(let key in counter){**
**if(counter[key] > 1){**//if any keys have a value greater than 1, return true. If no duplicates are found, return false
**return true;
}**
**return false;
}**//below is the solution without the pseudo-code**function checkForDuplicates(){
let counter={}
for(let val in arguments){
counter[arguments[val]] = (counter[arguments[val]] || 0)
}
for(let key in counter){
if(counter[key] > 1){
return true: 
}
return false;
}**
```