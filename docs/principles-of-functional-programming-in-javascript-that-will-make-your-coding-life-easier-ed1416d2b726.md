# JavaScript 中的函数式编程原则将使您的编码生活更加简单

> 原文：<https://javascript.plainenglish.io/principles-of-functional-programming-in-javascript-that-will-make-your-coding-life-easier-ed1416d2b726?source=collection_archive---------5----------------------->

## 编写更安全的代码和限制副作用的方法。

![](img/797e3d50b856c0e8fbbae82eb945353a.png)

Photo by [Shahadat Rahman](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

还有谁爱写没有副作用的函数？

我想我们作为程序员都是这样。

今天，在这个故事中，我将带你了解函数式编程的基本原则，这些原则将使你的编码生活变得更容易。

让我们开始吧。

# 1.纯函数

说到纯函数，你只需要记住两件事:

*   如果给定相同的参数，它们将返回相同的结果。
*   它们不会引起任何副作用。

## 如果给定相同的参数，则返回相同的结果

让我们看看下面这个简单的例子:

```
const DISCOUNT = 0.5;const calculatePrice = price => price * DISCOUNT;let actualPrice = calculatePrice(15); // 7.5
```

函数 **calculatePrice** 纯吗？不，不是的。

为什么？因为全局变量 **DISCOUNT** 没有作为参数传递给函数。如果您改变**折扣的**值，返回的结果也会改变，尽管参数**价格**保持不变。

为了使它成为纯函数，你必须像这样给函数增加一个参数:

```
const DISCOUNT = 0.5;const calculatePrice = (price, discount) => price * discount;let actualPrice = calculatePrice(15, DISCOUNT); // 7.5
```

如你所见，它满足了纯函数的第一法则。

如果给定相同的参数，并不总是能够写出返回相同结果的函数。你不需要担心，因为这不是一件坏事。你怎么能指望一个每次执行都返回随机数的函数是纯函数呢？

```
const saySomething = threshold => {
  if (Math.random() > threshold) {
    return ‘I am something’;
  } else {
    return ‘I am something else’;
  }
}saySomething(0.5);
```

## 不要引起副作用

什么是副作用？

当一个函数被调用时，它不仅返回一个值，还会改变一些东西，比如一个全局变量或者一个引用参数。副作用使得函数不可预测。

```
let totalPrice = 15;const calculateFinalPrice = discount => {
  if (totalPrice > 10) {
    totalPrice = totalPrice * discount;
  }

  return totalPrice;
}console.log(calculateFinalPrice(0.2)); // 3
console.log(totalPrice); // 3
```

在上面的示例中，变量 **totalPrice** 满足条件，它被更改为新值。

从长远来看，改变函数范围之外的东西通常会导致难以控制你的源代码，所以你需要使它纯净。

在这个例子中，我们不直接将新值赋给全局变量，而是仅用于计算并返回最终结果。

```
let totalPrice = 15;const calculateFinalPrice = discount => {
  let finalPrice = totalPrice;

  if (finalPrice > 10) {
    finalPrice = finalPrice * discount;
  }

  return finalPrice;
}console.log(calculateFinalPrice(0.2)); // 3
console.log(totalPrice); // 15
```

对于给定的输入，纯函数总是返回相同的结果。您应该努力构建稳定的应用程序，并使您的源代码更易于测试。

# 2.不变

回头看看上面的例子，你可以看到我没有直接修改变量 **totalPrice** ，我得到了一个新变量，并对它做了一些修改。

这就是不变性的概念，一个物体应该是不随时间变化的，或者我们应该强迫它不变。这样，您的代码更安全，更可控。一般来说，您可以通过使事情不可变来避免意外的行为。

虽然 JavaScript 中的基本类型是不可变的，但对象和数组的行为相反，这些类型的数据结构可以改变。

以原始类型为例:

```
let totalPrice = 13;
let finalPrice = totalPrice;
finalPrice = finalPrice * 0.3;console.log(totalPrice); // 13
console.log(finalPrice); // 3.9
```

当你给一个基本变量赋值时，实际值被赋值。这就是为什么在上面的例子中，虽然我们首先将 **totalPrice** 分配给了 **finalPrice** ，但是 **finalPrice** 的变化并不影响 **totalPrice** 的值。

对象和数组的作用方式不同:

```
let languages = [‘JavaScript’, ‘C++’, ‘Kotlin’, ‘Golang’];
let clonedLanguages = languages;
clonedLanguages[0] = ‘Java’;console.log(clonedLanguages); // [“Java”, “C++”, “Kotlin”, “Golang”]
console.log(languages); // [“Java”, “C++”, “Kotlin”, “Golang”]
```

当您将一个对象或数组分配给另一个对象或数组时，您分配的是对包含实际值的内存的引用。这就是为什么在上面的例子中，改变**克隆语言**的值会导致**语言**的相同变化，因为它们都指向内存中的同一个地址。

起初，当接近不变性时，你会觉得有点烦人。是的，您必须通过在流程中创建新实例来编写更多代码。然而，从长远来看，你会感谢这种恼人的行为，因为它有助于避免意想不到的行为。如果你在基于反应的项目中使用 redux，你就会明白我的意思。

# 3.对透明性有关的

与纯函数类似，参照透明度被定义为一个表达式，它可以被它的值替换，而不改变程序的结果。您也可以将其理解为一个函数，它总是为相同的给定参数返回相同的值，并且不会引起任何副作用。

例如:

```
const sumOf = (a, b) => a + b;sumOf(2, 3); // 5
```

如您在示例中所见，它是参照透明的，因为在任何给定的 **a** 和 **b** 的情况下， **sumOf(a，b)** 的结果保持不变。

什么时候它不是参照透明的？看看上面例子的一个简单变化。

```
const sumOf = (a, b) => {
  console.log(a, b);
  return a + b;
}const take1 = (a, b) => {
  let result = sumOf(a, b);
  return result + result;
}const take2 = (a, b) => {
  return sumOf(a, b) + sumOf(a, b);
}take1(1, 2);
take2(1, 2);
```

您可能会认为 **take1(1，2)** 和 **take2(1，2)** 的结果是一样的，是 6。然而，事实并非如此。 **take1** 只打印一次日志，而 **take2** 打印两次日志。以下是实际结果:

```
take1(1, 2); // 1 2 result = 6
take2(1, 2); // 1 2 1 2 result = 6
```

为什么要关心引用透明度？因为它使您的代码可测试和可预测，所以您可以在编写每一行代码时避免任何不确定性。

# 4.一流的功能

当谈到第一类函数时，我们指的是被视为对象的函数。这意味着您可以将函数作为参数传递给其他函数，甚至作为其他函数的结果返回一个函数。JavaScript 就是这样对待函数的。

因为我们使用函数作为值，所以我们可以将它们存储在变量、对象和数组中。

例如:

为变量分配函数:

```
const sum = (a, b) => a + b;
```

将函数作为参数传递:

```
const averageOfTwo = (sum, a, b) => sum(a, b) / 2;
```

结果返回一个函数:

```
const operation = (a, b) => {
  return (a, b) => a + b;
}const op = operation(3, 5);
console.log(op(5, 3)); // 8
```

# 5.高阶函数

如果满足以下条件，则函数为高阶函数:

*   它采用其他函数作为参数。
*   或者它返回一个函数作为结果。

高阶函数可以缩短代码并使其更简单，从而减少错误并增加可读性。

JavaScript 中一些常见的高阶函数，您很可能使用过，但直到现在才知道这个概念:

## 。地图

```
const names = [‘Amy’, ‘James’, ‘Tom’];
const uppercasedNames = names.map(name => name.toUpperCase());console.log(uppercasedNames); // [“AMY”, “JAMES”, “TOM”]
```

## 。为每一个

```
const names = [‘Amy’, ‘James’, ‘Tom’];names.forEach(name => console.log(name));
```

## 。过滤器

```
const numbers = [3, 4, 1, 6, 7, 10];
const evenNumbers = numbers.filter(number => number % 2 === 0);console.log(evenNumbers); // [4, 6, 10]
```

# 结论

函数式编程是一个很好的标准，您应该遵循它来提高代码的质量和可维护性。

上述概念是函数式编程的基础，它将帮助你让其他程序员喜欢你的编码方式。

希望这个故事对你有用。

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a) [## 每个 Web 开发人员都应该知道的 11 个 JavaScript 概念，让他们的技能更上一层楼

### 不了解这些概念，就无法掌握 JavaScript。

medium.com](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a)