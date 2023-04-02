# 您应该知道的 5 个 JavaScript 新特性

> 原文：<https://javascript.plainenglish.io/5-new-javascript-features-you-should-know-adb410c2bf0a?source=collection_archive---------2----------------------->

## JavaScript 中你应该知道的新特性

![](img/e47d50debcc85ebfc201e7706a262ab7.png)

Photo by [JASUR JIYANBAEV](https://unsplash.com/@djianbaev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

JavaScript 是每个 web 开发人员都必须知道的语言。它是 web 开发的女王。随着 ECMAScript 新版本的推出，它在过去几年中有了很大的改进，尤其是 ES6，它使得开发人员的开发过程变得更加容易。这就是为什么作为 JavaScript 开发人员，您需要更新这些特性。

在本文中，我们将向您展示您应该知道的 5 个 JavaScript 新特性。让我们开始吧。

# 1.JavaScript 字符串填充

JavaScript 中增加的一个新特性是字符串填充。所以 ECMAScript 2017 为此增加了两个字符串方法:`**padStart**` 和`**padEnd**` 。这允许您在字符串的开头和结尾添加填充。

看看下面的例子:

```
let str = "5";
str = str.**padStart(4,0)**;
// result is **000**5
```

现在，让我们尝试使用`**padEnd**` **在字符串末尾添加填充。**

```
let str = "1";
str = str**.padEnd(3,0)**;
// result is 1**00**
```

确保这些字符串填充方法有两个参数，第一个参数应该是一个比要添加的元素数大 1 的数字。例如，如果您想在一个字符串的末尾添加两个字符，您必须在方法`padEnd`的第一个参数中添加 3。

# 2.属性描述符

JavaScript 增加了一个新特性`**Object.getOwnPropertyDescriptors()**` **。**此功能允许您返回包含描述符的对象的每个属性。描述符基本上是添加到属性中的元信息，它定义了如何使用该属性。

这里有一个例子:

```
var x = { number1: 1, number2: 2};
console.log(**Object.getOwnPropertyDescriptors(x)**);/* Prints: { number1: {value: 1, writable: true, enumerable: true, configurable: true},
number2: {value: 2, writable: true, enumerable: true, configurable: true} } */
```

如您所见，该方法为您提供了一种以非常简单的方式获取对象的所有描述符信息的方法。

# 3.JavaScript 异步函数

异步函数在 JavaScript 中异步执行。这意味着我们的代码可以同时执行多个任务，而不必等待任务转移到另一个任务。

异步函数是用关键字`**async**` 声明的，并且关键字`**await**`允许出现在这些函数中。

让我们看看下面的例子:

```
// async functions can be called befoe declaration. **myDisplay()**;**async** function myDisplay() {
  let myPromise = new Promise(function(myResolve, myReject) {
    setTimeout(function() { myResolve("I love You !!"); }, 3000);
  });
  console.log(**await** myPromise);
}// This will come before the promise even when it is called after.
console.log("This will come before even when it is called after");
// Calling the function.//Output:
// Prints: "This will come before even when it is called after" 
// Returns a fulfilled promise "I love You !!"
```

正如你所看到的，承诺将出现在字符串之后，即使我们在函数中在字符串之前调用它。那是因为承诺需要 3000 毫秒才能执行。异步代码不会等待它，所以字符串会在承诺之前出现。即使函数在声明之前被调用，它仍然工作，因为它是一个异步函数。

# 4.对象值

JavaScript 还引入了`**Object.values**` ，它返回对象值的一维数组。

看看下面的例子:

```
const person = {
  firstName : "Mehdi",
  lastName : "Aoussiad",
  age : 19,
  eyeColor : "black"
};
console.log(**Object.values**(person));
// Prints ["Mehdi","Aoussiad",19,"black"]
```

# 5.对象条目

`**Object.entries**`类似于`**Object.values**`，但是返回对象值和属性(键)的多维数组。

这里有一个例子:

```
const person = {
  firstName : "Mehdi",
  lastName : "Aoussiad",
  age : 19,
  eyeColor : "black"
};
console.log(**Object.entries**(person));
// Prints: [["firstName", "Mehdi"],["lastName", "Aoussiad"],["age", 19],["eyeColor", "black"]]
```

# 结论

正如你所看到的，这些只是一些你需要知道的简单特性，因为它很有帮助。工具变化很快，我们需要不断学习，因为它从未停止，尤其是在科技行业。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-javascript-projects-you-should-build-as-a-front-end-developer-57318b710344) [## 作为前端开发人员应该构建的 5 个 JavaScript 项目

### 为您的投资组合提供前端 web 开发项目

medium.com](https://medium.com/javascript-in-plain-english/5-javascript-projects-you-should-build-as-a-front-end-developer-57318b710344)