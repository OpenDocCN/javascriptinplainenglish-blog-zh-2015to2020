# 亚马逊编码面试问题

> 原文：<https://javascript.plainenglish.io/amazon-coding-interview-questions-187eaf91f194?source=collection_archive---------2----------------------->

## 通过每天解决一个问题，变得非常擅长编写面试代码

![](img/4441b6d3fcd0e6ac61a885c754862106.png)

Photo by [Bryan Angelo](https://unsplash.com/@bryanangelo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 日常编码问题

它们是受真实编程面试启发的各种各样的问题，带有深入的解决方案，清晰地带您了解每个核心概念。

> 通过每天解决一个问题，变得格外擅长编写面试代码。

我们将一起使用 JavaScript 解决这些问题。

# 问题#1

## 问题

游程编码是一种快速简单的字符串编码方法。基本思想是将重复的连续字符表示为单个计数和字符。

例如，字符串`“AAAABBBCCDAA”`将被编码为`“4A3B2C1D2A”`。

实现游程编码和解码。您可以假设要编码的字符串没有数字，只包含字母字符。您可以假设要解码的字符串是有效的。

## 解决办法

**编码字符串**

以下是我的解决方案。这对任何人来说都很容易理解。

```
const encoding_string = function (string) { if (string.length === 0) return ''

    let currChar = string.charAt(0)
    let count = 1
    let encoding = ''

    for (let i = 1; i < string.length; i++) {
        const char = string.charAt(i)
        if (char === currChar) count++
        else {
            encoding += count + currChar

            // reset
            count = 1
            currChar = char
        }
    }

    encoding += count + currChar

    return encoding

}
```

**解码字符串**

我们需要实现两个实用函数。

*   检查字符是否为数字？

```
const isNumber = function(str) {
    return /^\d+$/.test(str)
}
```

*   返回字符串末尾字符的 X 计数。

```
const addCountAmount = function (string, char, count) {
    for (let i = 1; i <= count; i++) {
        string += char
    }

    return string
}
```

最后，这是我的解决方案。

```
const decoding_string = function (string) {
    if (string.length === 0) return ''
    let currCount = 0
    let i = 0
    let decoding = ''

    while (i < string.length) {
        const char = string.charAt(i)
        if (isNumber(char)) {
            const num = Number(char);
            currCount = currCount * 10 + num
        } else {
            decoding = addCountAmount(decoding, char, currCount)
            currCount = 0
        }

        i++
    }

    return decoding
}
```

尝试测试我们的解决方案。

*   字符串`“AAAABBBCCDAA”`将被编码为`“4A3B2C1D2A”`

```
encoding_string(“AAAABBBCCDAA”)
<< 4A3B2C1D2A
```

*   等等

```
decoding_string(“4A3B2C1D2A”)
<< AAAABBBCCDAA
```

# 问题#2

## 问题

这里有 N 级楼梯，你可以一次爬上 1 级或 2 级。给定 N，写一个函数，返回你爬楼梯的唯一方式的数量。步骤的顺序很重要。

例如，如果 N 是 4，那么有 5 种独特的方式:

*   1, 1, 1, 1
*   2, 1, 1
*   1, 2, 1
*   1, 1, 2
*   2, 2

如果你能从一组正整数 X 中爬出任意数，而不是一次只能爬 1 或 2 步，会怎么样？例如，如果 X = {1，3，5}，你可以一次爬 1、3 或 5 级台阶。

## 解决办法

返回一次爬 1 或 2 级楼梯的独特方式的数量。

```
const climb_stairs_1 = function (stairs) {
    if (stairs === 1) return 1
    if (stairs === 2) return 2

    let prev = 1
    let curr = 2
    for (let i = 2; i < stairs; i++) {
        const sum = prev + curr
        prev = curr
        curr = sum
    }
    return curr
}
```

从一组正整数中返回任意数字的唯一爬楼梯方式的数量。

```
const climb_stairs_2 = function (stairs, nums) {
    const dp = Array(stairs + 1).fill(0)

    for (let i = 1; i <= stairs; i++) {
        // get the total f(i - x) where x is every number in nums
        let total = 0
        for (let j = 0; j < nums.length; j++) {
            const num = nums[j]

            // check in range
            if (i - num > 0) total += dp[i - num]
        }
        dp[i] += total

        // if i is in nums, then increment
        if (nums.indexOf(i) !== -1) dp[i] += 1
    }
    // get last number in dp
    return dp[dp.length - 1]
}
```

尝试测试我们的解决方案。

```
climb_stairs_1(4
<< 5climb_stairs_2(4, [1, 3, 5])
<< 3
```

# 问题#3

## 问题

给定一个整数 k 和一个字符串 s，找出最多包含 k 个不同字符的最长子字符串的长度。

例如，给定`s = “abcba”`和`k = 2`，具有 k 个不同字符的最长子串是`“bcb”`。

## 解决办法

以下是我的解决方案。

```
const k_longest_substring = function(string, k) {
    let l = 0
    let r = 0
    const charCount = new Map()
    let longestSubstring = ''

    while (r < string.length) {
      const char = string.charAt(r)

      // update count
      if (charCount.has(char)) {
        charCount.set(char, charCount.get(char) + 1)
      } else {
        charCount.set(char, 1)
      }

      // size of the char count is equal to k
      // start moving left pointer until the window substring has at most distinct k characters
      if (charCount.size > k) {
        while (charCount.size > k && l < r) {
          const leftChar = string.charAt(l)
          const count = charCount.get(leftChar)

          if (count === 1) charCount.delete(leftChar)
          else charCount.set(leftChar, count - 1)

          l++
        }
      }

      const substring = string.substring(l, r + 1)
      longestSubstring =
        substring.length > longestSubstring.length ? substring : longestSubstring
      r++
    }
    return longestSubstring
}
```

尝试测试我们的解决方案。

```
k_longest_substring("abcba", 2)
<< bcb
```

很简单，对吧？

[](https://medium.com/javascript-in-plain-english/coding-interview-questions-6b539371a028) [## 科技巨头的面试问题编码

### 通过每天解决一个问题，变得非常擅长编写面试代码

medium.com](https://medium.com/javascript-in-plain-english/coding-interview-questions-6b539371a028) 

我会更新亚马逊在这篇文章中提出的新问题🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读😘

## **用简单英语写的 JavaScript 笔记**

我们一直对帮助推广高质量内容感兴趣。如果您有一篇要提交给 JavaScript 的文章，请用您的中用户名在[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)发邮件给我们，我们会将您添加为作者。

我们还推出了三种新出版物！通过以下方式表达对我们新出版物的热爱: [**通俗易懂的 AI**](https://medium.com/ai-in-plain-english)、 [**通俗易懂的 UX**、](https://medium.com/ux-in-plain-english)、[、**通俗易懂的 Python**、](https://medium.com/python-in-plain-english)、**、**——谢谢您，继续学习！