# 如何在 JavaScript 中拆分字符串

> 原文：<https://javascript.plainenglish.io/learn-how-to-use-split-method-to-split-string-in-javascript-7b3d3d2e60d6?source=collection_archive---------8----------------------->

理解 JavaScript 中的 Split 方法

![](img/a8bce0b13ec65b9ca78eb2703bbe9fef.png)

Image taken [here](https://images.pexels.com/photos/278887/pexels-photo-278887.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260).

`**split**` 方法根据提供的**模式**将字符串拆分为子字符串的**数组。**

## 例子

```
let numbersStr = "1,2,3,4,5";let numArray = numbersStr.split(",");console.log(numArray); // ["1","2","3","4","5"]
```

在上面的例子中，split 方法将循环遍历字符串中的所有字符，如果找到了提供的模式，那么 split 方法将收集在找到模式之前循环的字符，并将所有字符连接成一个字符串，并将该字符串推入一个数组。

如果我们没有传递任何模式，那么整个字符串将被推到一个数组中并返回

```
numbersStr.split(); // ["1,2,3,4,5"]; 
```

如果传递的模式是字符串的第一个字符，则将一个空字符串推入数组。

```
numbersStr.split("1"); // ["", "2,3,4,5"]
```

同样，如果传递的模式是字符串的最后一个字符，则将一个空字符串推送到结果数组中。

```
numbersStr.split("5")' // ["1,2,3,4", ""]
```

如果我们以空字符串的形式传递模式，那么字符串中的每个字符都将被拆分并放入数组中

```
numbersStr.split("");
//  ["1", ",", "2", ",", "3", ",", "4", ",", "5"]
```

我们还可以通过传递参数`limit`来传递要执行的拆分操作的数量。

```
numberStr.split(",", 2); // ["1", "2"]
```

如果我们将 limit 作为`0`传递，那么将返回一个空字符串

```
numberStr.split(",", 0); // []
```

# 真实世界的例子。

1.  在 JavaScript 中从 css 属性获取值。

```
let divHeight = document.getElementById("div1").style.height; //10pxlet height = divHeight.split("px");console.log(height); // "10"
```

2.反转字符串

```
let name = "test";let splittedArray = name.split(""); // ["t", "e", "s", "t"]splittedArray.reverse(); // ["t", "s", "e", "t"]let reversedString = splittedArray.join(""); // tset
```

感谢您的阅读。如果你觉得这有用，请考虑通过 [PayPal](https://paypal.me/jagathishSaravanan?locale.x=en_GB) 捐赠给我。