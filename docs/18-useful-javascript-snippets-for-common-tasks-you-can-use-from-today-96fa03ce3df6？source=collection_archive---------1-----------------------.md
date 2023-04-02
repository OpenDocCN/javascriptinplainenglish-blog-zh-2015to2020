# 18 个有用的 JavaScript 代码片段，你可以从现在开始使用

> 原文：<https://javascript.plainenglish.io/18-useful-javascript-snippets-for-common-tasks-you-can-use-from-today-96fa03ce3df6?source=collection_archive---------1----------------------->

## 生命是短暂的，不要浪费时间一遍又一遍地写同样的代码。

![](img/785eefd753d3790de6aaa6cb87c8ef1a.png)

Photo by [Gabriel Crismariu](https://unsplash.com/@momentsbygabriel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果有人问我作为初学者应该学习什么编程语言，我会告诉他们是 JavaScript。这种强大的语言几乎涉及编程的每个方面——前端、后端、web 应用、桌面应用、移动应用。这样的例子不胜枚举。

在今天的故事中，我将向您展示我经常使用的 18 个有用的 JavaScript 片段。他们帮助我节省了执行常见开发任务的时间。让我们开始吧。

# 1.maxItemOfArray

这个函数从一个数组中返回最大值。

```
const maxItemOfArray = (arr) => […arr].sort((a, b) => b — a).slice(0, 1)[0];let maxItem = maxItemOfArray([3, 5, 12, 5]);
```

# 2.areAllEqual

这段代码检查数组中的所有元素是否相等。

```
const areAllEqual = array => array.every(item => item === array[0]);let check1 = areAllEqual([3, 5, 2]); // false
let check2 = allEqual([3, 3, 3]); // true
```

# 3.平均

这个函数返回给定数字的平均数。

```
const averageOf = (…numbers) => numbers.reduce((a, b) => a + b, 0) / numbers.length;let average = averageOf(5, 2, 4, 7); // 4.5
```

# 4.反向限制

此代码片段反转一个字符串。

```
const reverseString = str => […str].reverse().join(‘’);let a = reverseString(‘Have a nice day!’); // !yad ecin a evaH
```

# 5.苏莫夫

这个函数返回给定数字的和。

```
const sumOf = (…numbers) => numbers.reduce((a, b) => a + b, 0);let sum = sumOf(5, -3, 2, 1); // 5
```

# 6.findAndReplace

这段代码在字符串中找到一个给定的单词，并用另一个替换。

```
const findAndReplace = (string, wordToFind, wordToReplace) => string.split(wordToFind).join(wordToReplace);let result = findAndReplace(‘I like banana’, ‘banana’, ‘apple’); // I like apple
```

# 7.RGBToHex

这个程序将 RGB 模式下颜色转换为十六进制。

```
const RGBToHex = (r, g, b) => ((r << 16) + (g << 8) + b).toString(16).padStart(6, ‘0’);let hex = RGBToHex(255, 255, 255); // ffffff
```

# 8.洗牌

你想知道任何音乐播放器如何随机播放项目吗？这个片段将向您展示如何操作。

```
const shuffle = ([…array]) => {
  let m = array.length;

  while (m) {
    const i = Math.floor(Math.random() * m--);
    [array[m], array[i]] = [array[i], array[m]];
  } return array;
};shuffle([5, 4, 3, 6, 20]);
```

# 9.移除错误值

该代码片段从数组中删除 false 值，包括 false、undefined、NaN 和 empty。

```
const removeFalseValues = arr => arr.filter(item => item);let arr = removeFalseValues([3, 4, false, ‘’, 5, true, undefined, NaN, ‘’]); // [3, 4, 5, true]
```

# 10.移除重复值

这个函数从数组中移除重复的项。

```
const removeDuplicatedValues = array => […new Set(array)];let arr = removeDuplicatedValues([5, 3, 2, 5, 6, 1, 1, 6]); // [5, 3, 2, 6, 1]
```

# 11.getTimeFromDate

这段代码从 Date 对象中返回字符串形式的时间。

```
const getTimeFromDate = date => date.toTimeString().slice(0, 8);let time = getTimeFromDate(new Date()); // 09:46:08
```

# 12.大写 AllWords

这个将字符串中所有单词的第一个字母大写。

```
const capitalizeAllWords = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());let str = capitalizeAllWords(‘i love reading book’); // I Love Reading Book
```

# 13.getDayDiff

这个函数返回两个日期之间的天数差。

```
const getDayDiff = (date1, date2) => (date2 — date1) / (1000 * 3600 * 24);let diff = getDayDiff(new Date(‘2020–04–01’), new Date(‘2020–08–15’)); // 136
```

# 14.辐射度

这个把角度从弧度转换成度数。

```
const radianToDegree = radian => (radian * 180.0) / Math.PI;let degree = radianToDegree(2.3); // 131.78
```

# 15.isValidJSON

这个代码片段检查给定的字符串是否是有效的 JSON。

```
const isValidJSON = string => {
  try {
    JSON.parse(string);
    return true;
  } catch (error) {
    return false;
  }
};let check1 = isValidJSON(‘{“title”: “javascript”, “price”: 14}’); // true
let check2 = isValidJSON(‘{“title”: “javascript”, “price”: 14, subtitle}’); // false
```

# 16.toWords

这个函数用于将一个给定的字符串转换成一个单词数组。

```
const toWords = (string, pattern = /[^a-zA-Z-]+/) => string.split(pattern).filter(item => item);let words = toWords(‘I want to be come a great programmer’); // [“I”, “want”, “to”, “be”, “come”, “a”, “great”, “programmer”]
```

# 17.scrollToTop

如果你在一个长页面的底部，你想快速向上滚动到顶部，这个片段可以帮助你顺利。

```
const scrollToTop = () => {
  const t = document.documentElement.scrollTop || document.body.scrollTop; if (t > 0) {
    window.requestAnimationFrame(scrollToTop);
    window.scrollTo(0, t — t / 8);
  }
};
```

# 18.isValidNumber

这个用来验证一个数字是否有效。

```
const isValidNumber = n => !isNaN(parseFloat(n)) && isFinite(n) && Number(n) === n;let check1 = isValidNumber(10); // true
let check2 = isValidNumber(‘a’); // false
```

就是这样。然而，这还不是全部。如果你有自己的有用片段列表，请在下面的评论中告诉我，我会把它们添加到故事中。如果这个故事成为有用的 JavaSnippets 的最终列表，那就太好了。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/15-simple-coding-techniques-to-get-your-tasks-done-with-shorter-code-in-javascript-59d46801db0) [## 15 种简单的编码技术，用更短的 JavaScript 代码完成任务

### 不要浪费时间写长代码，而你可以把它写得更短，更清晰，更易读。

medium.com](https://medium.com/javascript-in-plain-english/15-simple-coding-techniques-to-get-your-tasks-done-with-shorter-code-in-javascript-59d46801db0)