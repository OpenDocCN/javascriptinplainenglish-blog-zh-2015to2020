# 算法—使用 JavaScript 的对角差分解法:HackerRank

> 原文：<https://javascript.plainenglish.io/algorithms-diagonal-difference-solution-using-javascript-hackerrank-58ebac6e2686?source=collection_archive---------1----------------------->

## 给定一个正方形矩阵，计算其对角线之和的绝对差。

例如，正方形矩阵如下所示:

```
1 2 3
4 5 6
9 8 9
```

从左到右的对角线=1 + 5 + 9 = 15。右至左对角线= 3 + 5+ 9 = 17。它们的绝对差值为**| 15-17 | = 2。**

## **功能描述**

在下面的编辑器中完成 ***对角线差异*** 功能。它必须返回一个表示绝对对角线差的整数。

对角线差异采用以下参数:

*   arr:整数数组。

## **输入格式**

第一行包含单个整数， ***n*** ，矩阵中的行数和列数 ***arr*** 。

***n*** 的每一行都描述了一行 ***arr[i]*** ，并且由 ***n*** 空格分隔的整数 ***arr[i][j]*** 组成。

## **约束条件**

***。—100*≤*arr[I][j]*≤100**

## **输出格式**

将矩阵两条对角线之和的绝对差值打印为一个整数。

**样品输入**

```
 11 2 4
4 5 6
10 8 -12
```

**样品输出**

```
15
```

## **说明**

主对角线为:

```
11
   5
     -12
```

主对角线上的总和:11+5–12 = 4

次要对角线为:

```
4
   5
10
```

次对角线之和:4 + 5 + 10 = 19
差:| 4–19 | = 15

**注:** |x|是 x 的[绝对值](https://www.mathsisfun.com/numbers/absolute-value.html)

# 解决办法

使用 JavaScript:

```
**function** diagonalDifference(arr) { **var** n = arr.length; 
    **var** d1 = 0;
    **var** d2 = 0; **for**(**var** i=0; i<n; i++){ **for**(**var** j=0; j<n; j++){
       *// finding the sum of primary diagonal* **if**(i === j) {
           d1 += arr[i][j];
         }
       *// finding the sum of secondary diagonal* **if**(i + j === n - 1){
            d2 += arr[i][j];
         } } }
  **return** Math.abs(d1 - d2);
}
```

*更多内容参见*[](http://plainenglish.io)