# Airbnb 编码面试问题

> 原文：<https://javascript.plainenglish.io/airbnb-coding-interview-questions-4818501a7c80?source=collection_archive---------3----------------------->

## 通过每天解决一个问题，变得非常擅长编写面试代码

![](img/4231897496e2e98fa7a0ffb2434eb119.png)

*(2020 年 4 月 6 日更新)*

# 日常编码问题

它们是受真实编程面试启发的各种各样的问题，带有深入的解决方案，清晰地带您了解每个核心概念。

> 通过每天解决一个问题，变得格外擅长编写面试代码。

我们将一起使用 JavaScript 解决这些问题。

# 问题#1

## 问题

给定一个整数列表，写一个函数，返回非相邻数的最大和。数字可以是 0 或负数。例如，`[2, 4, 6, 2, 5]`应该返回`13`，因为我们选择了 2，6，而`[5, 1, 1, 5]`应该返回`10`，因为我们选择了 5 和 5。

你能在 O(N)时间和常数空间做到这一点吗？

## 解决办法

以下是我的解决方案。这对任何人来说都很容易理解。

```
const i1 = [2, 4, 6, 2, 5];
const i2 = [5, 1, 1, 5];

const nonAdjSum = (input) => {
  let incl = 0;
  let excl = 0;
  let new_excl;
  input.forEach( (el) => {
    new_excl = excl > incl ? excl : incl;
    incl = excl + el;
    excl = new_excl;
  })

  return excl > incl ? excl : incl;

}

nonAdjSum(i1)
<< **13**nonAdjSum(i2)
<< **10**
```

# 问题#2

## 问题

柯拉茨猜想的这些规则。

*   如果一个数是奇数，下一个变换是 3*n+1
*   如果一个数是偶数，下一个变换是 n/2
*   这个数字最终被转换成 1。
*   步骤是一个数变成 1 需要多少次变换。

给定一个整数 n，输出将[1，n]中的数变换为 1 的最大步骤。

## 解决办法

首先，我们编写`findSteps(num)`函数。

```
const findSteps = function (num) {
    if (num <= 1) return 1
    if (num % 2 == 0)
        return 1 + findSteps(num / 2)
    return 1 + findSteps(3 * num + 1)
}
```

然后，我们通过实现`findLongestSteps(num)`函数找到要转换的最大步骤。

```
const findLongestSteps = function (num) {

    if (num < 1) return 0

    var res = 0
    for (let i = 1; i <= num; i++) {
        t = findSteps(i)
        res = Math.max(res, t)
    }

    return res
}
```

最后，我们尝试测试这个解决方案。

```
console.log(findLongestSteps(37))
<< **112**console.log(findLongestSteps(101))
<< **119**
```

# 问题#3

## 问题

用多个数组实现一个队列，其中每个数组都有固定的大小，`MAX_QUEUE_SIZE`。

## 解决办法

从队列的一般描述中，我们知道我们至少需要:

*   `offer(num)`功能
*   `poll()`功能。
*   `isEmpty()`和`isFull()`功能。

简单队列的数组实现所需的数据片段是:一个`array`和一个`count`。这些东西够了吗？让我们看一个例子来找出答案。我们将从一个包含 3 个元素的队列开始。

```
queue (made up of 'contents' and 'count')
-----------------   -----
| a | b | c |   |   | 3 |
-----------------   -----
  0   1   2   3     count
contents
```

其中`a`在`front`处`c`在`rear`处。现在，我们用`queue.offer('e')`添加一个新元素。

```
queue (made up of 'contents' and 'count')
-----------------   -----
| a | b | c | d |   | 4 |
-----------------   -----
  0   1   2   3     count
contents
```

一切似乎都很好。如果我们用`queue.poll()`去掉一个元素呢？

```
queue (made up of 'contents' and 'count')
-----------------   -----   -----
|   | b | c | d |   | 3 |   | a |
-----------------   -----   -----
  0   1   2   3     count    ch
contents
```

不幸的是，我们有一个问题，因为队列的`front`不再位于数组位置 0。一种解决方案是将所有元素下移一位，给出。

```
queue (made up of 'contents' and 'count')
-----------------   -----
| b | c | d |   |   | 3 |
-----------------   -----
  0   1   2   3     count
contents
```

但是我们拒绝了这种解决方案，因为每次删除一个元素时将所有内容都下移太昂贵了。相反，我们是否可以使用额外的信息来跟踪前方？

是啊！我们可以使用前面元素的索引，给出。

```
queue (made up of 'contents', 'front' and 'count')
-----------------   -----   -----
|   | b | c | d |   | 1 |   | 3 |
-----------------   -----   -----
  0   1   2   3     front   count
contents
```

现在，如果我们输入另一个元素:`queue.offer('e')`？目前，队列的尾部保存着`'d'`，位于数组的末尾。我们将把`'e'`放在哪里？我们已经说过把所有东西都搬下来太贵了。另一种方法是在`circular fashion`中使用数组。换句话说，当我们到达数组的末尾时，我们绕回并使用开头。现在，选择输入`'e'`，字段看起来像这样:

```
queue (made up of 'contents', 'front' and 'count')
-----------------   -----   -----
| e | b | c | d |   | 1 |   | 4 |
-----------------   -----   -----
  0   1   2   3     front   count
contents
```

最后，之后会是什么样子:`queue.poll()`？

```
queue (made up of 'contents', 'front' and 'count')
-----------------   -----   -----   -----
| e |   | c | d |   | 2 |   | 3 |   | b |
-----------------   -----   -----   -----
  0   1   2   3     front   count    ch
contents
```

正如我们所见，队列所需数据集的一个选择是:一个`array`、一个`front index`和一个`count`。

还有一种可能。相反，我们可以用车尾的位置来代替计数，这样就可以使用以下数据:an `array`，a `front index`，a `rear index`。

然后，这些数据将以如下方式反映队列的状态…从一个有 4 个元素的队列开始…

```
queue (made up of 'contents', 'front' and 'rear')
-----------------   -----   -----
| a | b | c | d |   | 0 |   | 3 |
-----------------   -----   -----
  0   1   2   3     front   rear
contents
```

现在，用`queue.poll()`移除一个，给出:

```
queue (made up of 'contents', 'front' and 'rear')
-----------------   -----   -----
|   | b | c | d |   | 1 |   | 3 |
-----------------   -----   -----
  0   1   2   3     front   rear
contents
```

现在，添加一个带有`queue.offer('e')`的，给出:

```
queue (made up of 'contents', 'front' and 'rear')
-----------------   -----   -----
| e | b | c | d |   | 1 |   | 0 |
-----------------   -----   -----
  0   1   2   3     front   rear
contents
```

可以用 JavaScript 实现这个算法吗？

很简单，对吧？

我会更新 Airbnb 在这篇文章中提出的新问题🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读😘

## **用简单英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。