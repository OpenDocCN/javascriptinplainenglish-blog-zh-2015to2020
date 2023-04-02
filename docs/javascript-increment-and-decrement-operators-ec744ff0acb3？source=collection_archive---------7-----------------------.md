# JavaScript 递增和递减运算符

> 原文：<https://javascript.plainenglish.io/javascript-increment-and-decrement-operators-ec744ff0acb3?source=collection_archive---------7----------------------->

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

two monitors with no code

# 简短有用的 JavaScript 课程。让它变得简单

问题:

做++x 或者 x++是一样的吗？快速回答:是也不是。

## 句法

*   **增量:x++或++xx**
*   **减量:x- —或——x**

## 例子

```
**Case 1**
1\. let x1 = 0;
2\. x1++;
3\. x1++;
4\. x1++;
5\. console.log(x1); //3  ok**Case 2**
6\. let x2= 0;
7\. ++x2;
8\. ++x2;
9\. ++x2;
10\. console.log(x2); //3  ok**Case 3**
11\. let x3= 1;
12\. console.log(**x3++**);    // 1 !!! **What happens?**
13\. console.log(x3);      // 2 ok**Case 4**
11\. let x4= 1;
12\. console.log(++**x4**);    // 2 ok
13\. console.log(x4);      // 2 ok
```

案例 3 第 12 行发生了什么？

这里，当我们显示 x3 的值时，两者都没有改变。这是因为在操作数被改变之前，操作数的原始值被返回。

在步骤 12 的下一行:“console.log(x3)”中，操作数在前面的步骤中已经改变，并且当我们显示它时，它的值增加了。

相反，在第 4 种情况下，第 12 行的值首先递增，然后显示结果。这里，“console.log(++x4)”显示增加的操作数的值。

减量操作也是如此:

```
**Case 5**
14\. let x5 = 1;
15\. console.log(**x5--**);    // 1 !!! **What happens?**
16\. console.log(x5);      // 0**Case 6**
17\. let x6 = 1;
18\. console.log(--**x6**);    // 0 ok
19\. console.log(x6);      // 0 ok
```

## 如果这对你有帮助，请点击下面的拍手按钮。非常感谢！