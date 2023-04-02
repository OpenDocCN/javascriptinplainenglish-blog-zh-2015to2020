# JavaScript 数字比它们应该有的更复杂

> 原文：<https://javascript.plainenglish.io/simple-things-gone-wrong-how-to-find-out-the-number-of-digits-in-a-javascript-number-c0a78c6acbec?source=collection_archive---------0----------------------->

![](img/93b844c14c76bd04e0bf854eeeaeb025.png)

“Numbers along the starting line on a running track” by [Kolleen Gladden](https://unsplash.com/@rockthechaos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为一名开发人员，你有时可能会得到一些任务，需要做一些不寻常的事情或者获取一些信息，或者你可能只是为了知道而想知道一些事情。这些任务有些可能很简单，有些可能不简单。但是可能发生的情况是，这个任务看起来很简单，但是如果你不够小心，就会有一些陷阱等着咬你。

在这篇短文中，我想和大家分享我的一个经历，有一个看似简单的解决方案，但实际上这个解决方案并不是在所有情况下都能给出正确的结果。

# 问题是

这个问题非常简单:**确定一个数字有多少位数**。这看起来并不复杂，对吗？许多 JavaScript 对象都有`.length` 属性来轻松确定对象的长度。唯一的问题是，`**Number**`对象没有`.length` 属性！但是也许我们可以把我们的数字转换成另一个具有该属性的对象，这样我们就可以用它来计算出初始数字的位数？！这似乎是一个很好的起点，所以让我们试试吧。

> **注意:**为了简单起见，我们只处理正整数。主要思想适用于负数和浮点数，但是实现需要更多的条件和检查。

# stringy by`***Number***`

当我们看数字时，它只是一串数字。因此，尝试将数字转换成字符串是有意义的，幸运的是，字符串有一个`.length` 属性，它给出了字符串中的字符数。这似乎是一个解决方案，这就是我们想要的！让我们试一试。

```
*const number = 12345; // obviously has five digits
const numberString = number.toString(); // this is now a string '12345'

// let's log the string length now
console.log(numberString.length);

// 5 -> as expected, there are five characters in this string, accounting for five digits in the number*
```

它真的起作用了，我们很高兴这个任务被解决了。让我们只是玩多一点的数字，以确保它的工作。

```
*const number1 = 1234567890;
const number1String = number1.toString();
console.log(number1String.length); // 10 -> works

const number2 = 123456789000000;
const number2String = number2.toString();
console.log(number2String.length); // 15 -> nice, still works

const number3 = 1234567890000000000;
const number3String = number3.toString();
console.log(number3String.length); // 19 -> nice, still works even though we have passed the Number.MAX_SAFE_INTEGER value*
```

目前为止看起来不错。让我们给它一个很大的数字，只是为了好玩。

```
*const number4 = 12345678900000000000000000000000000000000;
const number4String = number4.toString();
console.log(number4String.length); // 14 -> awesome still wo.... Wait! What? Fourteen!?*
```

不对不对！这本该是 41，结果却是 14。像往常一样，就在你认为已经完成、解决了问题的时候，你偶然发现了一个打破你的解决方案的案例。:(

调试时间到了。

# 问题的原因

由于某种原因，`number4String` 变量的`.length` 属性返回 14，而不是我们预期的 41。这意味着即使数字有 41 位，字符串也只有 14 个字符。为了查看我们的`number4` 变量的字符串表示发生了什么，我们可以简单地记录这个字符串表示，看看它看起来像什么。其实我们把之前的都记录下来，这样就可以对比了。

```
*const number1 = 1234567890;
const number1String = number1.toString();
console.log(number1String); // '1234567890' -> fine
console.log(number1String.length); // 10 -> works

const number2 = 123456789000000;
const number2String = number2.toString();
console.log(number2String); // '123456789000000' -> fine
console.log(number2String.length); // 15 -> works

const number3 = 1234567890000000000;
const number3String = number3.toString();
console.log(number3String); // '1234567890000000000' -> fine
console.log(number3String.length); // 19 -> works

const number4 = 12345678900000000000000000000000000000000;
const number4String = number4.toString();
console.log(number4String); // '1.23456789e+40' -> oh! ah! uh! of course!
console.log(number4String.length); // 14 -> wrong*
```

看，最后一个数字用科学符号表示，以减少使用的位数。`number4String` 变量有 14 个字符，所有的零都被塞进了指数`e+40`。

# 寻找解决方案

为了找到这个问题的解决方案，我们需要思考一下。我们不能使用`.length` 方法，但是我们仍然有一个数字可以用来执行计算。

## 强力计算

假设我们的数字是十进制的，我们可以用一个简单的数学运算来计算出数字的个数。

例如，我们有一个数字 100。很明显，这个数字中有 3 个数字，但是我们如何通过计算得到这些信息呢？

所以，我们需要通过数学计算从 100 中得到 3。我们可以试着除以 33.33，我们会得到非常接近 3。唯一的问题是这种方法不适用于 200 这个数字，我们需要它适用于从 100 到 999 的每一个 3 位数。

显然，加法、减法和乘法在这里并不适用于所有这些数字，所以我们可以得出结论，除法仍然是一条路要走。我们只需要找到最好的除数。

我们真正需要的是，当我们一次除数的时候，结果实际上比起始数少了一位。你说:“好吧，但是我们可以把 100 除以任何一个数，这样就可以少一个数字，这有什么用呢？”。这是一个很好的观察结果，它将我们带到下一个条件，我们实际上需要能够用相同的数除 999，并且得到比 999 少一位的结果数。所以现在，我们看到 1-9 范围内的数字都不行。

让我们试着用 10 除 100。

第一组:

第二组:

现在让我们试着用 10 除 999。

第一组:

第二组:

嗯，所以…运气不好！？不，不是真的。在第二个示例中，总位数没有减少，但是小数点前面的位数实际上减少了，并且减少的方式与第一个示例中的位数相同。现在让我们追踪小数点前面的数字:

第一组:

第二组:

我们看到除法的数目是 2，我们得到了一位数。但是我们知道原来的数字有 3 位数。所以，如果我们再分一次，这两种情况下我们得到 0.1 和 0.999。

到目前为止，我们的最终目标可能已经很明显了…通过反复除以数字 10，使值小于 1。

在这个例子中，我们做了 3 次除法，与两个数的位数相匹配。例如，如果我们用 384698 来尝试，您会得到这样的结果:

*   分部 1 => 38469.8 > 1 = >一位数字
*   分部 2 => 3846.98 > 1 = >两位数
*   分部 3 => 384.698 > 1 = >三位数
*   分部 4 => 38.4698 > 1 = >四位数
*   分部 5 => 3.84698 > 1 = >五位数
*   除法 6 => 0.384698 < 1 =>六位数

我们得到这个数有 6 个数字，这是正确的。

所有这一切只是一些简单的数学，现在我们需要把它转化成一个程序来完成这些计算。首先，让我们明确定义我们需要做什么:

**当数字大于或等于 1 时，除以 10，每除以一次，位数增加 1。**

这条语句告诉我们应该如何编程。通过使用`while` 循环:

```
*let number = 468168;
let length = 0;
while (number >= 1) {
    number = number / 10;
    length++;
}
console.log(length); // 6*
```

这在数学中总是可行的，虽然在 JavaScript 中并不总是可行，因为它的浮点很奇怪，但是在大多数情况下都是可行的，这是我们在计算中能做到的最好的。

## 使用指数

还有另一种方法可以得到位数，不需要我们计算任何东西，我们可以使用 JavaScript 的内置方法来得到我们想要的。让我们看看如何做到这一点。

好的，所以问题是数字是用[科学符号](https://en.wikipedia.org/wiki/Scientific_notation)写的，这限制了其字符串表示的字符数，我们不能使用`.length`字符串方法来获得位数。但是我们可以使用科学符号中的信息来得到数字的个数。为了做到这一点，让我们用科学记数法写一个数字:

```
*const number = 3.1131e+7 // this is 31 131 000*
```

我们看到这个数的位数应该是 8。让我们再做一个:

```
*const number = 7.35e+17 // this is 735 000 000 000 000 000*
```

这个有 18 位数。我们现在可以看到一个模式，显示指数值的数字实际上告诉我们小数点后有多少位。在第二个例子中，我们看到数字中的两个数字，其余的都是零，总共是 17 个数字。要获得完整的位数，我们只需在指数值上加 1，就可以得到正确的 18 位数:

```
*const number = 4.68e+25;
const string = number.toString();
const exponent = string.split('e')[1].substring(1);
const numberOfDigits = parseInt(exponent) + 1;
console.log(numberOfDigits); // 26*
```

我们在这里做了什么？在第二行，我们将数字转换为字符串，以便能够使用字符串方法。在第三行我们做了几件事。首先，我们将字符`e` ( `string.split(‘e’)`)上的`string` 分开，这样我们就得到了两个字符串的数组`[‘4.68’, ‘+25’]`。这个数组的第二个成员是指数值，所以我们用`string.split(‘e’)[1]`来访问它。现在我们有了一个带有指数值的字符串，我们只对数字感兴趣，对符号不感兴趣。因此，我们可以用`*.*substring` 的方法得到加号:`string.split(‘e’)[1].substring(1)`后的一切。现在我们有了字符串`‘25’`，我们需要在下一行给它加 1，但是我们需要注意不要把数字 1 和这个字符串连接起来。这就是为什么我们使用`parseInt()` 方法将字符串`‘25’`转换成数字并安全地进行加法运算。

现在，有一件事要记住。编号`4.68e+25` 也可以写成`46.8e+24` 或`468e+23` 或`4680e+22`等。位数仍然是指数值加上小数点前面的位数，所以总是 26。只要值的类型是`number` 就没问题。当我们将这个数字作为字符串类型而不是数字类型获取时，这里的问题就出现了，而且大多数情况下，我们从表单中的输入字段获取值，而表单通常会获取字符串。如果我们将我们的解决方案应用于这样写的字符串，我们会得到错误的结果。幸运的是，这很容易解决，我们所要做的就是将得到的字符串转换成数字，然后再转换回字符串，然后我们就可以对该字符串应用我们的解决方案:

```
*const value = '468e+23';
const number = Number(value); // this re-formats the number into '4.68e+25', so-called "normalized" scientific notation
const string = number.toString();
const exponent = string.split('e')[1].substring(1);
const numberOfDigits = parseInt(exponent) + 1;
console.log(numberOfDigits); // 26*
```

当然，如果我们得到正常表示的数，也就是说，不是科学记数法，那么这个方法将再次失败，因为它没有指数。因此，我们需要添加一个条件来检查数字的格式。让我们将它封装到一个函数中，并添加以下检查:

```
*function getDigits(value) {
    const number = Number(value);
    const string = number.toString();
    const hasExponent = string.indexOf('e') !== -1; // is the number written using the scientific notation

    if (hasExponent) {
        const exponent = string.split('e')[1].substring(1);
        return parseInt(exponent) + 1;
    }

    return number.toString().length;
}

// as strings
console.log(getDigits('468e+23')); // 26
console.log(getDigits('12345678')); // 8
// as numbers
console.log(getDigits(468e+23)); // 26
console.log(getDigits(12345678)); // 8*
```

这里，如果数字中有指数部分，我们分析数字以获得指数并返回指数值加 1。如果没有指数，我们返回`number.toString().length` ，直接给出位数。

# 示例应用程序

我制作了一个简单的应用程序，您可以在输入字段中输入一个数字，并查看本文中描述的方法的效果。请点击下面的链接:

[示例应用](https://gruximillian.github.io/number_length/app/)

# 结论

本文中描述的问题是我们在实践中很少遇到的，但在我看来，值得记住这一点。解决方案并不困难，特别是对于有经验的开发人员来说，但是对于没有经验的开发人员来说，这可能是一个问题，他们可能甚至没有意识到有问题，因为`number.toString().length` 对于长达 22 位数的数字很有效，之后事情对我来说就更“科学”了。🤓

我希望你学到了一些新的东西，或者至少在阅读这篇文章时获得了一些乐趣。感谢您的阅读！