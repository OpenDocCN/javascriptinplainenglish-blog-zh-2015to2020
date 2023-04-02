# 脸书编码面试问题

> 原文：<https://javascript.plainenglish.io/facebook-coding-interview-questions-9e40bdbbec35?source=collection_archive---------2----------------------->

## 通过每天解决一个问题，变得非常擅长编写面试代码

![](img/d75523dd4f3fea51cf211b2064387d68.png)

*(2020 年 4 月 28 日更新)*

# 日常编码问题

它们是受真实编程面试启发的各种各样的问题，带有深入的解决方案，清晰地带您了解每个核心概念。

> 通过每天解决一个问题，变得格外擅长编写面试代码。

我们将一起使用 JavaScript 解决这些问题。

# 问题#1

## 问题

```
Given the mapping a = 1, b = 2, … z = 26, and an encoded message, count the number of ways it can be decoded.
```

例如，消息`‘111’`将给出`3`，因为它可以被解码为`‘aaa’, ‘ka’, and ‘ak’`。

您可以假设这些消息是可解码的。比如`‘001’`是不允许的。

## 解决办法

首先，我们定义函数 isDecodable `(message)`,如果消息可解码，则返回 1，否则返回 0。

```
function isDecodable(message) {
    for (let i = 0; i < message.length; i++) {

        var parsed = parseInt(message[i], 10)

        if (isNaN(parsed)) {
            return false
        }

        if ((parsed > 0 && parsed < 27) != true) {
            return false
        }
    }
    return true
}
```

接下来，我们假设消息是可解码的。为了解决这个问题，我们使用了基于递归的解决方案。

如果一个字符串的长度是 1，总有一种方法可以解码，所以这是我们的基本情况。

```
'1':
    ['1']

----------
F('1') = 1if (message.length <=1) {
    return 1
}
```

如果长度是 2，我们总是有 1 路分别处理所有数字，如果一个数字小于`26`，则加 1，我们也使用这一路作为基本情况。

```
'12':
    ['1', '2']
    ['12']
---------------------
F('12') = f('12') + 1
```

如果长度为 3，我们可以使用之前计算的结果，因为我们已经知道如何处理更短的字符串。

```
F('123') = f('1') * F('23') + F('12') * f('3') = 3
```

最后，所有接下来的情况都可以用这种方法计算。

```
F('4123') = f('4') * F('123') + f('41') * F('23') = 3
```

这个问题的最终源代码

```
function decode_cnt_no_zero(message) {

    let length = message.length

    if (length <=1) {
        return 1
    }

    if (length >=2) {

        var parsed = parseInt(message.substring(0,2),10)

        if (parsed >=0 && parsed <= 26) {
            return (decode_cnt_no_zero(message.substring(1,length)) 
            + decode_cnt_no_zero(message.substring(2, length)))
        }
        return decode_cnt_no_zero(message.substring(1, length))

    }
}
```

让我们尝试探索我们的解决方案。

*   解码 1 字符串的结果是 1。

```
decode_cnt_no_zero(“1”)
<< 1
```

*   解码 12 字符串的结果是 2。

```
decode_cnt_no_zero(“12”)
<< 2
```

*   解码 123 字符串的结果是 3。

```
decode_cnt_no_zero(“123”)
<< 3
```

*   解码 4123 字符串的结果是 3。

```
decode_cnt_no_zero(“4123”)
<< 3
```

# 问题#2

## 问题

给定一个太大而无法存储在内存中的元素流，以均匀的概率从流中选取一个随机元素。

## 解决办法

返回一个函数，该函数为每个元素流选择一个随机元素。

```
const selectRandomizer = function () {
    let streamCount = 0
    let selected

    const rand = function (stream) {
        for (let i = 0; i < stream.length; i++) {
            streamCount++
            if (streamCount === 0) selected = stream[i]
            else if (Math.random() <= 1 / streamCount) {
                selected = stream[i]
            }
        }
        return selected
    }

    return rand
}
```

# 问题#3

## 问题

一个建筑商想要建造一排 N 栋房子，它们可以有 K 种不同的颜色。他的目标是最小化成本，同时确保没有两个相邻的房子是相同的颜色。

给定一个 N 乘 K 的矩阵，其中第 N 行和第 K 列表示建造第 N 个颜色的房子的成本，返回实现这个目标的最小成本。

## 解决办法

返回用 K 种不同颜色建造 N 栋房屋的最小成本，其中没有两栋相邻的房屋颜色相同。

这里有一个解，时间复杂度为 O(NKK)，空间复杂度为 O(NK)。

```
const minCostHouseColorInitialSolution = function(costs) {
    if (costs.length === 0) return 0

    const n = costs.length
    const k = costs[0].length

    // 2d array with n length and k rows
    const dp = [...Array(n)].map(() => Array(k))

    // fill first row
    for (let col = 0; col < k; col++) {
      dp[0][col] = costs[0][col]
    }

    for (let row = 1; row < n; row++) {
      // Finding the lowest costs for each column in this row
      for (let col = 0; col < k; col++) {
        dp[row][col] = Number.MAX_SAFE_INTEGER

        for (let numCol = 0; numCol < k; numCol++) {
          // we dont want the same column, as that is 2 houses of same color
          if (numCol !== col) {
            dp[row][col] = Math.min(
              dp[row][col],
              dp[row - 1][numCol] + costs[row][col]
            );
          }
        }
      }
    }

    let minCost = Number.MAX_SAFE_INTEGER;
    // get minimum of last row in dp table
    for (let col = 0; col < k; col++) {
      minCost = Math.min(minCost, dp[n - 1][col])
    }

    return minCost
  }
```

接下来，仍然是`O(NKK)`时间复杂度，而是`O(K)`空间复杂度。

```
const minCostHouseColorII = function (costs) {
    if (costs.length === 0) return 0

    const n = costs.length
    const k = costs[0].length

    const dp1 = [] // the last row
    const dp2 = [] // the current row

    // start the first row of costs, as the last row, and build from the second row onwards
    for (let i = 0; i < k; i++) {
        dp1[i] = costs[0][i]
    }

    for (let row = 1; row < n; row++) {
        for (let j = 0; j < k; j++) {
            // Finding the lowest costs for each column in this row
            dp2[j] = Number.MAX_SAFE_INTEGER;

            for (let m = 0; m < k; m++) {
                if (m !== j) {
                    dp2[j] = Math.min(dp2[j], dp1[m] + costs[row][j])
                }
            }
        }

        // copy all the current row to last row
        for (let j = 0; j < k; j++) {
            dp1[j] = dp2[j]
        }
    }

    let minCost = Number.MAX_SAFE_INTEGER;
    for (let i = 0; i < k; i++) {
        minCost = Math.min(minCost, dp1[i])
    }
    return minCost
}
```

最后，这里是时间复杂度为 O(NK)，空间复杂度为 O(1)的最佳解。

```
const minCostHouseColorBest = function (costs) {
    if (costs.length === 0) return 0

    const n = costs.length
    const k = costs[0].length

    let min1 = 0
    let min2 = 0
    let idx1 = -1

    for (let i = 0; i < n; i++) {
        let m1 = Number.MAX_SAFE_INTEGER
        let m2 = Number.MAX_SAFE_INTEGER
        let idx2 = -1

        for (let j = 0; j < k; j++) {
            let cost = costs[i][j]
            cost += j === idx1 ? min2 : min1

            if (cost < m1) {
                m2 = m1
                m1 = cost
                idx2 = j
            } else if (cost < m2) {
                m2 = cost
            }
        }

        min1 = m1
        min2 = m2
        idx1 = idx2
    }

    return min1;
}
```

很简单，对吧？

我会更新脸书周刊在这篇文章中提出的新问题🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读😘，再见👋，别忘了👏最多 50 次并跟随！