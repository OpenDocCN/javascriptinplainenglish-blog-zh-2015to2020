# JavaScript 让你疯狂的 9 种方式

> 原文：<https://javascript.plainenglish.io/9-ways-javascript-can-drive-you-crazy-b7627532fc4c?source=collection_archive---------10----------------------->

## 这就是编程语言的灵活性会让你困惑的地方。

![](img/ce29ec3ab7ba9e09e230e3e3640d9b45.png)

Photo by [Afif Kusuma](https://unsplash.com/@findracadabra?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最常用的编程语言。它涉足大多数平台。从前端到后端，桌面到移动，常规应用到视频游戏等。

所以，又一篇让 JavaScript 成为超级英雄的文章？

不，我们已经称赞得够多了。来说说这次是怎么让我们困惑的吧。

## 1.“南”是一个数字

```
let typeOfNotANumber = typeof NaN;
console.log(typeOfNotANumber); // number
```

## 2.一个数字可以等于一个字符串

```
let value = (28 == ‘28’);
console.log(value); // true
```

实际上，我们应该使用`===`来产生预期的结果。那么，为什么不干脆去掉`==`？两个都有太让人困惑了。

## 3.甚至使用`===`有时也会给我们错误的结果

```
let value = (2020 === 2020.00000000000002021);
console.log(value); // true
```

为什么是真的？这几个肯定不是同一个号吧？

## 4.`NaN`不等于`NaN`

南:咚，咚！

我:谁在那里？

南:南。

我:南谁？

南:南，我不是我。

我:？？？

```
let value = (NaN === NaN);
console.log(value); // false
```

是的，`NaN`不是`NaN`。为什么？我不知道。

## 5.您可以将数字和字符串相加

```
let value = 20 + ‘20’;
console.log(value); // 2020
```

为什么一个数和一个字符串的和是合法的？

## 6.无法将字符串数组转换为数字数组

```
let strings = [‘28’, ‘43’, ‘12’, ‘54’];
let numbers = strings.map(parseInt);
console.log(numbers); // [28, NaN, 1, NaN]
```

我想把一个字符串数组转换成一个数字数组，但是好像行不通。我做错什么了吗？

## 7.`++`操作符将把一个字符串当作一个数字

```
let string = ‘13’;
string += string;
console.log(string); // 1313
```

`+=`表现如我所料。

然而:

```
let string = ‘13’;
console.log(++string); // 14
```

为什么`++`运算符把一个字符串当作一个数字？

## 8.在一个数的开头附加一个 0 会将它转换成八进制数

```
console.log(010 === 8); // true
```

但是:

```
console.log(010 === 10); // false
```

因为当你在 JavaScript 中的任何数字前添加`0`时，它被认为是八进制数字系统中的一个数字。小心点。

## 9.Null 是一个对象…有时

`null`有时是一个对象。

```
console.log(typeof null); // object
```

但其他时候，却不是。

```
console.log(null instanceof Object); // false
```

那么，`null`到底是不是对象？

## 结论

有时候我不明白 JavaScript 中的事情是如何发生的。我还没有得到答案。

我只知道一件事，JavaScript 仍在进化中。越来越好。 [ES6 就是一个很好的证据](https://medium.com/javascript-in-plain-english/9-es6-features-every-javascript-developer-should-know-b1f2915e7add)。

希望有一天所有的困惑都能消除。

那么，JavaScript 是如何迷惑你的呢？请在下面的评论中告诉我。

希望你喜欢这篇文章。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/how-to-control-this-better-in-javascript-dcacd54bcf97) [## 如何在 JavaScript 中更好地控制“this”

### 使用 call()、apply()和 bind()方法

medium.com](https://medium.com/javascript-in-plain-english/how-to-control-this-better-in-javascript-dcacd54bcf97)