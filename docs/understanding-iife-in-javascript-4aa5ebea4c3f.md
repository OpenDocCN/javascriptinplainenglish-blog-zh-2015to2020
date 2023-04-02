# 理解 JavaScript 中的生活

> 原文：<https://javascript.plainenglish.io/understanding-iife-in-javascript-4aa5ebea4c3f?source=collection_archive---------7----------------------->

## 通过实例了解 JavaScript 中的生活

![](img/d7f4f34394a09c1144101662d4ae17c0.png)

Photo by [Azharul Islam](https://unsplash.com/@azhar93?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

JavaScript 中的一个常见模式是一声明函数就执行它。这就是所谓的立即调用函数表达式或生命模式，它将导致函数立即被执行或调用。

在本文中，我们将通过实际例子来了解 JavaScript 中的生活。让我们开始吧。

![](img/7134d879ad57100d5bab728b73359bd6.png)

Image created with ❤️️ By [Mehdi Aoussiad](https://mehdiouss315.medium.com/).

# 什么是生活？

IIFE 代表**I**immediately**I**nvoked**f**function**e**expression，它是 JavaScript 中的一种模式，用于在声明函数后立即执行函数。

这是一个生活的例子:

```
**(**function () {
  console.log("Chirp, chirp!");
}**)()**; // this is an anonymous function expression that executes right away
// Outputs "Chirp, chirp!" immediately
```

请注意，该函数没有名称，也没有存储在变量中。函数表达式末尾的两个括号`**()**`使它立即被执行或调用。

# 什么时候使用生命？

有时，您无意中给变量和函数起了与全局变量和函数相同的名字，从而污染了全局范围。例如，当您的应用程序中有多个由多个开发人员在一段时间内编写的 JavaScript 文件时。所以在这种情况下，函数和变量很有可能同名。

考虑下面的例子，一个网页中包含两个不同的 JavaScript 文件。

文件`index.js`:

```
var userName = "Bill";

function display(name)
{
    alert("index.js: " + name);
}

display(userName);
```

文件`app.js`:

```
var userName = "Steve";

function display(name)
{
    alert("app.js: " + name);
}

display(userName);
```

如果你在你的网页上运行这两个 JavaScript 文件，你会发现只有文件`app.js`被执行，因为它包含在 HTML 中的文件`index.js`之后。因此，如果两个函数或变量同名，JavaScript 总是考虑最后一个定义。

生命通过拥有自己的范围来解决这个问题。它限制函数和变量成为全局变量。因此，在 IIFE 内部声明的函数和变量即使同名也不会污染全局范围。

为了解决上述两个 JavaScript 文件的问题，我们可以将这两个文件中的所有代码打包。

看看下面的例子:

```
**(**function () {
    var userName = "Steve";

    function display(name)
    {
        alert("index.js: " + name);
    }

    display(userName);
  }**)()**;
```

如你所见，这是利用生命的好时机。全局范围不会再受影响。

# 结论

生命不会创造不必要的全局变量和函数。它还有助于组织代码并使其可维护。你可以从 Mozilla 文档中了解更多。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/6-useful-javascript-array-methods-every-beginner-should-know-35f1db5e8491) [## 每个初学者都应该知道的 6 个有用的 JavaScript 数组方法

### 强大的 JavaScript 初学者数组方法和实际例子

medium.com](https://medium.com/javascript-in-plain-english/6-useful-javascript-array-methods-every-beginner-should-know-35f1db5e8491)