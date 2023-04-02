# 你需要知道的 JavaScript 面试问题—第二部分

> 原文：<https://javascript.plainenglish.io/javascript-interview-questions-you-need-to-know-part-ii-e1e49ea64bc3?source=collection_archive---------1----------------------->

## 这里还有另外 5 个难度不等的问题。

***查看第一部分*** [***此处***](https://medium.com/p/javascript-interview-questions-you-need-to-know-26fbdda23d6e)

![](img/5f34793c512398feba723858113cf89d.png)

Image from Tutorialrepublic

# 容易的

## 问:什么是回调函数，并提供一个简单的例子

答:回调函数是在另一个函数执行完之后执行的函数。另一种描述方式是，回调函数是作为参数传递给另一个函数的函数，并在某个操作完成后执行。

```
function writeBlog(topic, **callback**) {
  alert(`Starting my ${topic} blog.`);// then execute the callback function that was passed
  **callback()**;
}

writeBlog('JS'**, function() {
  alert('Finished my blog!');
}**);
```

如果您运行此代码，您将得到两个警告。第一天“开始我的 JS 博客”第二个会说“写完了我的博客！”

# 简单/中等

## 问:什么是原语？JavaScript 中的原始值有哪些？

答:原语不是对象，也没有自己的方法。所有原语都是不可变的。JS 中有六种类型的原语:

1.  **布尔型** —真或假
2.  **空值** —没有值
3.  **未定义**——一个已声明但尚未赋值的变量
4.  **数字** —整数、浮点数等。
5.  **字符串** —“”内的任何内容
6.  **符号** —不等于任何其他值的唯一值(在 ES6 中引入)

```
Symbol('x') === Symbol('x') // false
```

# 中等

## 问:如何检查一个数字是否是整数？

答:一个快速的方法是将这个数除以 1，看看是否有余数。

```
function isAnInt(num) {
  return num % 1 === 0;
}

console.log(isInt(10)); // true
console.log(isInt(1.1)); // false
console.log(isInt(0.5)); // false 
```

# 困难的

## 问:关键字“this”是如何工作的？什么是例子？

答:*这个*关键字指的是一个对象。具体来说，它是执行当前 JS 代码的对象。这意味着每个 JS 函数都有一个对其执行上下文的引用(函数是如何调用的)，称为 *this* 。

```
function state() { console.log(this.name);} var name = "NY";var obj1 = { name: "IN", state: state };var obj2 = { name: "CA", state: state }; state();           // "NY"obj1.state();      // "IN"obj2.state();      // "CA"
```

`state()`功能的作用是控制台记录`this.name`。这意味着它试图打印当前执行上下文的`name`属性的值(`*this*` 对象)*。*

**** *这个*** *关键词要深入得多，有默认和隐式绑定，也有显式和固定绑定。阅读更多关于这方面的内容以获得更好的理解。***

## 问:这个代码片段返回什么？真的还是假的？

答:见下文

```
Number.MIN_VALUE > 0;
```

答案是真的！你可能认为最小值是 0，但实际上`Number.MIN_VALUE`是`5e-234`。这是可以表示为浮点数的最小正数。基本上，它是你能得到的最接近 0 的值。那么最小值是多少呢？`Number.NEGATIVE_INFINITY`。

如果你们遇到了一些有趣的 JS 问题，请在评论中分享。也许我可以写一个用户分享问题的帖子！

HAPPY NEW YEAR! 2020 😎🥳!