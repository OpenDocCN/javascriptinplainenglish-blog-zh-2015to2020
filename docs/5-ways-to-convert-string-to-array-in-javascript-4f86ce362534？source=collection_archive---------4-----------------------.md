# 在 JavaScript 中将字符串转换为数组的 5 种方法

> 原文：<https://javascript.plainenglish.io/5-ways-to-convert-string-to-array-in-javascript-4f86ce362534?source=collection_archive---------4----------------------->

![](img/bd7637c10190121af7fe497930b57c40.png)

Photo by [Denis Tuksar](https://unsplash.com/@dtuksar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 中的字符串可以用 5 种不同的方式转换成数组。我们将使用`split`、`Array.from`、`spread`、`Object.assign`和`for`循环。让我们简单讨论一下所有的方法。

# 1.split()方法

此方法用于根据提供的分隔符拆分字符串，并返回子字符串数组。

```
let str = 'Tiger,Horse,Elephant,Wolf';
let arr = str.split(','); // split string by comma

console.log(arr);
// ["Tiger", "Horse", "Elephant", "Wolf"]
```

如果你想按每个字符分割字符串，那么你可以指定一个空字符串(`“”`)作为分隔符。

```
let str = 'jscurious';
let arr = str.split('');
console.log(arr); 
// ["j", "s", "c", "u", "r", "i", "o", "u", "s"]
```

`split()`方法接受第二个可选参数，该参数设置了分割的限制。这个限制值决定了返回的数组中将包含多少个元素。

```
let str = 'Cricket | Hockey | Football | Tennis';
let arr = str.split(' | ', 2);
console.log(arr); 
// ['Cricket', 'Hockey']
```

# 2.Array.from()方法

方法从任何 iterable 对象中返回一个数组。我们可以在`from()`方法中传递一个字符串值来获得一个字符数组。

```
let str = 'jscurious';
let arr = Array.from(str);
console.log(arr); 
// ["j", "s", "c", "u", "r", "i", "o", "u", "s"]
```

该方法还接受两个可选参数。一个是调用数组每个元素的`map`函数，另一个是在执行`map`函数时用作`this`的值。

```
let str = 'jscurious';
let arr = Array.from(str, (val, index) => val + index);
// adding index value to each element of array

console.log(arr); 
// ["j0", "s1", "c2", "u3", "r4", "i5", "o6", "u7", "s8"]
```

# 3.扩展运算符

扩展运算符(…)提取并扩展字符串中的每个字符。我们可以把所有这些字符放在一个数组中，从字符串中创建一个新的数组。

```
let str = 'jscurious';
let arr = [...str];
console.log(arr); 
// ["j", "s", "c", "u", "r", "i", "o", "u", "s"]
```

# 4.Object.assign()方法

此方法用于将值和属性从一个或多个源对象复制到目标对象。我们可以提供一个字符串作为源，一个空数组作为目标，从字符串创建一个数组。

```
let str = 'jscurious';
let arr = Object.assign([], str);
console.log(arr); 
// ["j", "s", "c", "u", "r", "i", "o", "u", "s"]
```

# 5.For 循环

我们可以使用 for 循环遍历字符串中的每个字符，并将该字符推送到一个空数组中，以从该字符串创建一个数组。

```
let str = 'jscurious';
let arr = [];
for(let i of str) {
    arr.push(i);
}
console.log(arr); 
// ["j", "s", "c", "u", "r", "i", "o", "u", "s"]
```

# 你可能也喜欢

*   [节省您时间的 JavaScript 速记编码技巧](https://jscurious.com/20-javascript-shorthand-techniques-that-will-save-your-time/)
*   [寻找数组中元素的 6 种方法](https://jscurious.com/how-to-find-elements-in-array-in-javascript/)
*   [使用 JavaScript 中的通知 API 发送推送通知](https://jscurious.com/the-notification-api-in-javascript/)
*   [用 JavaScript 中的 HTMLAudioElement API 播放音频](https://jscurious.com/play-audio-with-htmlaudioelement-api-in-javascript/)
*   [JavaScript 中的地图，当它是比对象更好的选择时](https://jscurious.com/map-in-javascript-and-how-it-is-better-than-object/)
*   [JavaScript 设置对象存储唯一值](https://jscurious.com/javascript-set-object-to-store-unique-values/)
*   [JavaScript 中的生成器函数](https://jscurious.com/generator-functions-in-javascript/)
*   JavaScript 中承诺的简要指南

*感谢您抽出时间* ☺️
欲了解更多网络开发博客，请访问[jscurious.com](http://jscurious.com/)