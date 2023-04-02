# 什么是空间复杂性，它是如何确定的？

> 原文：<https://javascript.plainenglish.io/what-is-space-complexity-and-how-is-it-determined-fa2ab0354b?source=collection_archive---------5----------------------->

![](img/5174df0d6051072166f2d3f29f08dfeb.png)

The Carina Nebula, in outer space

当评估一个算法的效率时，很可能最初的焦点是[时间复杂度](https://en.wikipedia.org/wiki/Time_complexity):运行所花费的时间。这是自然的——人类倾向于关注时间。本杰明·富兰克林曾经说过:“时间就是金钱”。托尼·斯塔克穿越回了过去，他曾向父亲转述了父亲未来的自己对他说的话:“再多的钱也买不到一秒钟的时间”。明智！

还有另一个性能评估是时间复杂度的一部分:[空间复杂度](https://en.wikipedia.org/wiki/Space_complexity):算法运行所需的内存。从某种意义上说，空间的复杂性是最基本的考虑。即使一个算法很痛苦*s l o o o o w w w*它仍然可以运行，但是如果一个算法需要的空间超过了程序分配的内存，它就不会运行了。

空间复杂度包括两个因素:辅助空间和输入空间。辅助空间是算法执行时使用的临时空间。考虑到输入的大小，输入空间是执行期间所需的空间。有些情况下，仅使用辅助空间来评估空间复杂性，但是对于本文，将使用总空间—辅助空间和输入空间。考虑这个类比:

在我来的地方，意式美国菜无处不在。假设你和你的大家庭一起吃周日晚餐，你负责做红酱、番茄酱和肉汁。我在炉子上的荷兰搪瓷烤箱里做调味汁。荷兰烤箱类似于辅助空间。你想不用容器做调味汁吗？祝你好运。当然，要烹饪你的意大利面酱，你必须合成配料。配料的数量[此处插入*您的* **正确的**列表]类似于输入空间。大家庭？大胃王？更多配料！更多空间！

空间复杂度被认为是评估内存或数据存储的使用。算法需要使用内存来完成几件事情:

*   存储程序指令(`compiler => runtime`)
*   执行(*如*函数调用、[跳转语句](https://www.cs.auckland.ac.nz/references/unix/digital/AQTLTBTE/DOCU_075.HTM) s)
*   存储数据(*例如*常量和变量值)

虽然这些都是起作用的因素，但存储的可变数据通常是主要考虑因素。与时间复杂度一样，空间复杂度通常用大 O 符号表示，并在其评估中考虑最坏情况。让我们用几个例子来演示算法的空间复杂度是如何确定的。示例在`JavaScript`中。

## 例№1:

```
**function add(n1, n2) {
  const sum = n1 + n2
  return sum
}**// In JavaScript, a number requires 64 bits of memory.
// For simplicity, we'll translate 64 bits to 8 bytes.// The add function requires memory for 3 variables: **n1**, **n2**, & **sum**.
// This memory is related to input space.// The add function requires memory for the function call and 
// return statement. This memory is related to auxiliary space.// The required memory for the add function would be:
//  **n1** | 8 bytes
//       +
//  **n2** | 8 bytes
//       +
// **sum** | 8 bytes
//       +
// aux | 8 bytes (this is a wild guess!)
//       =
//       32 bytes// The thing to note here is that the data space and auxiliary
// space are **constant**. No matter the value of the inputs, the
// memory needs are always the same:
// **n1** + **n2** + **sum** + aux, or 8 + 8 + 8 + 8 = 32 bytes.// We can evaluate the space complexity of the add function as 
// constant or **O(1)**. 
```

## 例№2:

```
**function sum(arr) {
  const sum = 0
  for (let i = 0; i < arr.length; i++) {
    sum += arr[i]
  }
  return sum
}**// The sum function requires memory for two variables: **sum** & **i**.
// This memory is related to input space.// The sum function requires memory for the function call, the for
// loop and the return statement. This memory is related to
// auxiliary space.// The thing to note here is that the data space, or input space
// is **not** constant. It is **linear**. The required memory for the input
// space depends on the size of the data. In this case, the length
// of the Array. For example:**const arr = [1, 2, 3, 4, 5]**
// N => arr.length => 5**sum(arr)**// The required memory for the sum function would be:
//  **arr** = k * 5 | 8 bytes * 5 = 40 bytes
//                          +
//          **sum** |                8 bytes
//                          +
//            **i** |                8 bytes
//                          +
//          aux |                8 bytes (another wild guess!)
//                          =
//                              64 bytes// Suppose the length of the Array was 10 instead of 5:**const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]**
// N => arr.length => 10**sum(arr)**// The required memory for the sum function would be:
//  **arr** = k * 10 | 8 bytes * 10 = 80 bytes
//                                 +
//           **sum** |                 8 bytes
//                                 +
//             **i** |                 8 bytes
//                                 +
//           aux |                 8 bytes (another wild guess!)
//                                 =
//                               104 bytes// The needed memory for the sum function to execute 
// proportionally increases with the size of the input: 
// k * N + **sum** + **i,** for N = 10:8 * 10 + 8 + 8 + 8 = 104 bytes// We can evaluate the space complexity of the sum function as 
// linear or **O(n)**.
```

无论是在技术评估、优化程序时被要求表达算法的空间复杂性，还是制作一大罐酱料来点缀一捆干草价值的意大利面条，当谈到为什么考虑空间是一个好的实践时，你都知道一些方法。胃口好！

[github.com/dangrammer](https://github.com/dangrammer)T3[linked.com/in/danieljromans](https://www.linkedin.com/in/danieljromans/)danromans.com

## **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**