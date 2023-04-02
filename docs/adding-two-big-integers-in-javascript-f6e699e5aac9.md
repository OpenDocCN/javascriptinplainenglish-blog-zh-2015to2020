# 用 JavaScript 将两个大整数相加

> 原文：<https://javascript.plainenglish.io/adding-two-big-integers-in-javascript-f6e699e5aac9?source=collection_archive---------3----------------------->

## **最近在解决 LeetCode 中的一个问题时，我遇到了一个问题，解决方案的一部分需要将两个非常大的整数相加，这些整数最大化了 53 位的大小**

![](img/d57d74d2fa55d2dfe23193a65038dd4f.png)

Photo by [Hasan Albari](https://www.pexels.com/@hasanalbari?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/silver-and-black-laptop-computer-1229861/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

因为它是一个大于 53 位的整数，所以不可能使用传统的+运算符进行加法运算。对我来说，我避开了传统的方法，通过将这两个大数字转换成字符串来将它们相加。

我遵循的步骤是—

1.  将这些大数字转换成字符串。
2.  通过向较小的字符串添加零来确保两个字符串具有相同的长度。
3.  将两个字符串转换为大小相等的单独数组。
4.  以相反的顺序遍历数组，从两个数组中各加一个数，当和超过 10 时，将进位值加 1，然后从和中减去 10，就像我们在初级类中所做的一样。
5.  将每次迭代的总和推入另一个数组。
6.  最后，将数组转换成字符串并返回该字符串。

[https://gist.github.com/ANUPAMCHAUDHARY1117/8fdce7ee0c72c4d44402b12602a670de](https://gist.github.com/ANUPAMCHAUDHARY1117/8fdce7ee0c72c4d44402b12602a670de)

就这样，我们只用 JavaScript 就完成了两个非常非常大的整数的相加。