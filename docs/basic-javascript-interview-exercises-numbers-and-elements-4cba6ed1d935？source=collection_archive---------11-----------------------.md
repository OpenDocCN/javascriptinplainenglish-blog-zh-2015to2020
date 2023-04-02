# 基本 JavaScript 面试练习—数字和元素

> 原文：<https://javascript.plainenglish.io/basic-javascript-interview-exercises-numbers-and-elements-4cba6ed1d935?source=collection_archive---------11----------------------->

![](img/f4355a956cdc01bbc483f49a64eba735.png)

Photo by [Nick Hillier](https://unsplash.com/@nhillier?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了得到一份前端开发人员的工作，我们需要搞定编码面试。

在这篇文章中，我们将看看一些每个人都应该知道的快速热身问题。

# **写一个计算 n 的斐波那契数的函数。**

我们可以通过 JavaScript 的循环和析构语法来实现。

我们如下创建`fib`函数:

```
const fib = (n) => {
  let i = 1,
    a = 1,
    b = 1;
  while (i < n) {
    [a, b] = [b, a + b];
    i++;
  }
  return a;
}
```

在上面的代码中，我们将`b`设置为`a`，将`b`设置为`a + b`，以便在我们增加`i`时用新的斐波那契值更新数字。

然后，当我们完成循环时，我们返回`a`。

# **写一个函数，接受一个字符串并返回一个字符串字符频率的映射。**

我们可以使用一个 JavaScript `Map`来做这件事。

我们通过将字符串拆分成一个字符数组来计算字符串中字符的频率，然后按如下方式计算字符的频率:

```
const count = str => {
  const chars = str.split('');
  const frequencies = new Map();
  for (let c of chars) {
    const freq = frequencies.get(c) || 0;
    frequencies.set(c, freq + 1);
  }
  return frequencies;
}
```

在代码中，我们使用了`Map`的`get`方法来获取频率，我们将它加 1，然后设置它。

一旦我们完成了循环，我们就返回`Map`。

# 写一个函数，接受一个数字并检查它是否是一个质数。

我们可以通过检查一个数是 2 还是奇数来判断它是否是质数。如果是奇数，那么我们尝试将数字除以给定数字除以 2 的底数。

例如，我们可以编写以下函数来检查质数:

```
const isPrime = (num) => {
  if (num % 2 === 0 && num !== 2) {
    return false;
  }
  for (let i = 3; i <= Math.ceil(num / 2); i++) {    
    if (num % i === 0) {
      return false;
    }
  }
  return true
}
```

我们检查它是否是偶数，而不是 2。如果是，我们返回`false`。

否则，我们运行循环来检查一个数是否能被 3 整除，直到`num`除以 2 的上限。

如果它能被其中任何一个整除，那么我们返回`false`。

否则，我们返回`true`。

# **编写一个函数，接受一个 DOM 元素和一个字符串，并打印包含该字符串的类名的任何直接子元素。**

我们可以使用 element 对象的`getElementsByClassName`方法来获取具有给定 class 属性的子元素。

例如，我们可以编写以下函数:

```
const getChildrenWithClass = (el, className) => {
 const children = el.getElementsByClassName(className);
  for (let c of children) {
   console.log(c);
  }
}
```

上面的代码调用`el`元素对象上的`getElementsByClassName`并打印出具有给定类名的子元素。

同样，我们可以使用`querySelectorAll`方法来做同样的事情。

我们可以这样写:

```
const getChildrenWithClass = (el, className) => {
  const children = el.querySelectorAll(`.${className}`);
  for (let c of children) {
    console.log(c);
  }
}
```

唯一的区别是我们用模板字符串``.${className}``调用了`querySelectAll`。

![](img/8aac432f6bc672aa4ae72658a4cec17c.png)

Photo by [Sinjin Thomas](https://unsplash.com/@sinjin_thomas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **编写一个函数，接受一个 DOM 元素和一个字符串，如果它的任何父节点包含带有该字符串的类，则打印该函数。当没有父元素时，它应该停止。**

我们可以递归地使用`parentNode`属性，直到到达根元素或者找到我们想要的带有`className`的元素。

为此，我们创建一个`getParentWithClassName`如下:

```
const getParentWithClassName = (el, className) => {
  let currentEl = el.parentNode;
  while (currentEl) {
    if (currentEl.className === className) {
      return currentEl;
    }
    currentEl = currentEl.parentNode;
  }
  return undefined;
}
```

在上面的代码中，我们将`currentEl`设置为`el`的`parentNode`。

然后我们使用一个`while`循环来搜索带有给定`className`的`parentNode`，直到找到一个带有给定名称的。

如果我们找到一个给定的`className`，我们返回这个元素。

否则，我们通过将`currrentel`设置为`currentEl.parentNode`来继续。

如果我们循环到根节点，仍然没有找到具有给定`className`的元素，我们返回`undefined`。

# 结论

我们使用`parentNode`属性来查找子节点的父节点。

为了获得带有给定 CSS 选择器的子节点，我们可以调用任何 DOM 导航方法来查找带有给定选择器的元素。

要创建一个字典，我们可以使用`Map`构造函数。

我们可以像处理斐波那契数列一样，使用析构语法轻松地重新分配多个事物。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****