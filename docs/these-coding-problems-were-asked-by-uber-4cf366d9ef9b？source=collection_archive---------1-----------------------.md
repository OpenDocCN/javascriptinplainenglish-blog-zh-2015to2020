# 优步编码面试问题

> 原文：<https://javascript.plainenglish.io/these-coding-problems-were-asked-by-uber-4cf366d9ef9b?source=collection_archive---------1----------------------->

## 通过每天解决一个问题，变得非常擅长编写面试代码

![](img/442cf12ac30ebe93fc054e1622219927.png)

*(2020 年 4 月 7 日更新)*

# 日常编码问题

它们是受真实编程面试启发的各种各样的问题，带有深入的解决方案，清晰地带您了解每个核心概念。

> 通过每天解决一个问题，变得格外擅长编写面试代码。

我们将一起使用 JavaScript 解决这些问题。

# 问题#1

## 问题

不用除法，在 O(n)时间内求解。

```
Given an array of integers, return a new array such that each element at index i of the new array is the product of all the numbers in the original array except the one at i. 
```

例如，如果我们的输入是[1，2，3，4，5]，那么预期的输出将是[120，60，40，30，24]。如果我们的输入是[3，2，1]，那么预期的输出将是[2，3，6]。

```
input = [3,2,1]
output = [2x1,3x1, 3x2] 
<< [ 2, 3, 6]input = [1,2,3,4,5]
output = [2x3x4x5,1x3x4x5,1x2x4x5,1x2x3x5,1x2x3x4] 
<< [ 120, 60, 40, 30, 24]
```

## 解决办法

有人能想到的最简单的方法是，对于索引 I 处的任何数组元素，取数组中除当前索引之外的所有其他元素的乘积，并存储当前索引的乘积。这种方式的时间复杂度是 O(n)+O(n*(n-1)) —一个不好的解。

```
// bad solutionfunction solution(list_of_numbers) {
    var cache = new Array()
    for (let i = 0; i < list_of_numbers.length; i++) {
        var item = 1
        for (let j = 0; j <  list_of_numbers.length ; j++) {
            if (j != i) {
                item = item * list_of_numbers[j]
            } else {
            }
        }
        cache.push(item)
    }
    return cache
}

var test_list = [3,2,1]

console.log(solution(test_list))
<< [ 2, 3, 6 ]
```

我们需要在 O(n)时间内想出一个不使用除法的更好的方法。

在不使用除法的情况下，你必须创建一个(k，v)字典来存储索引和我们在第一次扫描中看到的每个 num 左边的乘法结果。然后在第二次运行时从右向左创建新列表。同时，需要存储向右的乘法运算。

```
function solution(list_of_nums) {

    var retval = []
    var mul_left = {}
    var mul_right = {}

    mul_left[-1] = 1

    for (let i=0; i < (list_of_nums.length - 1); i++) {
        mul_left[i] = list_of_nums[i] * mul_left[i-1]
    }

    mul_right[list_of_nums.length] = 1

    for (let j=list_of_nums.length - 1; j > 0 ; j--) {
        mul_right[j] = list_of_nums[j] * mul_right[j+1]
    }

    for (let k = 0; k < list_of_nums.length; k++) {
        retval[k] = mul_left[k-1]*mul_right[k+1]
    }
    return retval

}

var test_list = [1,2,3,4,5]

console.log(solution(test_list))
<< [ 120, 60, 40, 30, 24 ]
```

# 问题#2

## 问题

一个规则看起来像这样:

```
A NE B
```

这意味着 A 点位于 b 点的东北方向。

```
A SW C
```

意味着 A 点在 c 的西南方。

给定一个规则列表，检查规则的总和是否有效。例如:

```
A N B
B NE C
C N A
```

不验证，因为 A 不能同时在 c 的北面和南面。

```
A NW B
A N B
```

被视为有效。

## 解决办法

首先我们需要定义一个`delta`数组(key:方向，value:位置)。

```
const delta = {
    "N": [0, 1], 
    "NE": [1, 1], 
    "E": [1, 0],
    "SE": [1, -1], 
    "S": [0, -1], 
    "SW": [-1, -1],
    "W": [-1, 0], 
    "NW": [-1, 1]
}
```

那么，这就是我们的解决方案。

```
const isValid = function(rules) {

    var retval = true
    // point_name = [x_coord, y_coord]
    var points = {}

    // go through all rules
    for (let index = 0; index < rules.length; index++) {

        const rule = rules[index]

        let ruleAnalyzer = rule.split(" ")

        // destination "d", location "loc", source "s" from current rule
        let d = ruleAnalyzer[0]
        let loc = ruleAnalyzer[1]
        let s = ruleAnalyzer[2]   

        // new souces
        if (!Object.keys(points).includes(s)) {
            points[s] = [0, 0]
        } 

        // get coords of source
        let x_s = points[s][0]
        let y_s = points[s][1]

        // locate the distance of destination from source by direction
        let delta_x = delta[loc][0]
        let delta_y = delta[loc][1]

        // calculate destination coords from rule
        x_d = x_s + delta_x 
        y_d = y_s + delta_y

        // check valid

        // new destination
        if (!Object.keys(points).includes(d)) {
            points[d] = [x_d, y_d]
            continue
        } 

        // this destination appeared before 

        // get destination coords calculated from previous rules
        let real_x_d = points[d][0]
        let real_y_d = points[d][1] 

        // coords from previous rule are different from calculation of current rule 
        if ((real_x_d !== x_d) && (real_y_d !== y_d)) {
            retval = false
        }

    }

    return retval
}
```

最后，我们测试这个解决方案。

```
isValid([“A N B”, “B NE C”, “C N A”])
**<< false**isValid([“A NW B”, “A N B”])
**<< true**
```

# 问题#3

## 问题

给定一个排序后的数组`arr[]`和一个数字`x`，写一个函数计算`arr[]`中`x`的出现次数。

预期时间复杂度为`O(Logn)`。

例如，如果 arr[] = {1，1，2，2，2，2，3}且 x = 2，则输出为 4。

## 解决办法

线性搜索 x，计算 x 的出现次数并返回结果。

```
var count_occurrences = function(arr, x) {
    var n = arr.length
    var res = 0; 
    for (let i=0; i<n; i++) 
        if (x == arr[i]) 
          res++; 
    return res; 
}
```

很简单，对吧？

[](https://medium.com/javascript-in-plain-english/coding-interview-questions-6b539371a028) [## 科技巨头的面试问题编码

### 通过每天解决一个问题，变得非常擅长编写面试代码

medium.com](https://medium.com/javascript-in-plain-english/coding-interview-questions-6b539371a028) 

我会更新优步周刊在这篇文章中提出的新问题🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读😘，再见👋，别忘了👏最多 50 次并跟随！