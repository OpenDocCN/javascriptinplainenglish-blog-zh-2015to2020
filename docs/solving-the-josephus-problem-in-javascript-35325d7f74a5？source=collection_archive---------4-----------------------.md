# 用 JavaScript 解决约瑟夫问题

> 原文：<https://javascript.plainenglish.io/solving-the-josephus-problem-in-javascript-35325d7f74a5?source=collection_archive---------4----------------------->

## 我们会活下来的！

![](img/c3c992043699f8d6833f6e7b127779ca.png)

Photo by [Artem Maltsev](https://unsplash.com/@art_maltsev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是约瑟夫斯问题？

在计算机科学和数学中，约瑟夫斯问题是一个理论问题。

以下是问题陈述:

```
There are n people standing in a circle waiting to be executed. The counting out begins at some point in the circle and proceeds around the circle in a fixed direction. In each step, a certain number of people are skipped and the next person is executed. The elimination proceeds around the circle (which is becoming smaller and smaller as the executed people are removed), until only the last person remains, who is given freedom. Given the total number of persons n and a number k which indicates that k-1 persons are skipped and kth person is killed in circle. 
```

任务是在最初的圈子里选择一个地方，这样你就是最后一个留下来的人，从而生存下来。

# 故事的起源

有 9 个犹太人与约瑟夫斯和他的朋友藏在一个洞里。这 9 个犹太人决定宁死也不被敌人抓住，于是以自杀的方法决定，11 个人排成一个圈，第一个人报出人数。每个号码报给第三个人后，这个人必须自杀。然后从下一个开始再数，直到所有人都自杀。但是约瑟夫斯和他的朋友们不想服从。约瑟夫斯要求他的朋友假装服从，他自己安排了朋友。在 2 号位和 7 号位，他们躲过了这场死亡游戏。

# 我们将如何在 JavaScript 中解决它？

一般来说，经典的约瑟夫问题有两个参数:N 和 K，形成一个 N 人的圈子，依次选出每第 K 个人进行淘汰。随着人们被杀死，圆圈缩小，目标是确定最后幸存的位置。

我们可以用一个数组来解决 N 个元素的集合的约瑟夫问题。该数组被视为一个环。

首先，我们定义一个位置数组— `m` (N 个元素)，初始值为 0。

```
var man = new Array()
    for (var i = 0; i < N; i++)
        man[i] = 0
```

接下来，我们确定被杀者的位置——`pos`变量的值。

```
do {
    pos = (pos + 1) % N // Ring
    if (man[pos] == 0)
        i++
    if (i == K) {
        i = 0
        break
    }
} while (true) 
```

然后，我们更新`m`阵中的被杀者，进入下一回合。

```
man[pos] = count
count++
```

我们继续循环，直到到达 n。最后，这是我们的完整解决方案。

```
const joseph = function (N, M) {
    var man = new Array()
    for (var i = 0; i < N; i++)
        man[i] = 0
    var count = 1
    var i = 0, pos = - 1
    while (count <= N) {
        do {
            pos = (pos + 1) % N // Ring
            if (man[pos] == 0)
                i++
            if (i == K) {
                i = 0
                break
            }
        } while (true)
        man[pos] = count
        count++
    }
    return man
}
```

尝试测试这个解决方案。

```
var game = Joseph (11, 3)<< [ 4, **10**, 1, 7, 5, 2, **11**, 9, 3, 6, 8 ]
```

其中 N = 20，M = 3。在第 13 或第 20 个位置，你将逃离这个死亡游戏。

```
var game = Joseph (20, 3)[ 7, 18, 1, 12, 8, 2, 15, 17, 3, 9, 13, 4, **19**, 10, 5, 16, 14, 6, 11, **20** ]
```

很简单，对吧？

感谢阅读😘，再见👋，别忘了👏最多 50 次并跟随！