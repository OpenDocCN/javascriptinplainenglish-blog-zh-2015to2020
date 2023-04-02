# 为什么要小心 JavaScript 类型强制？

> 原文：<https://javascript.plainenglish.io/why-should-we-be-careful-of-javascript-type-coercion-ba9c471f16cd?source=collection_archive---------8----------------------->

![](img/a4f0408d8453efb830bacc3069728c94.png)

Photo by [Stephanie LeBlanc](https://unsplash.com/@sleblanc01?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

由于 JavaScript 是一种动态类型的编程语言，对象和变量的数据类型可以动态改变。这是我们在编写越来越多的 JavaScript 程序时经常会遇到的问题。关于类型强制，需要注意一些事情，类型强制是指在程序执行过程中动态地转换数据类型。

# 类型强制

正如我们提到的，类型强制是动态地改变数据类型。当数据与预期类型不匹配时，就会发生这种情况。例如，如果我们想要操作数字和数字串，我们可以写:

```
2*'5'
```

我们能拿回 10 美元。

这似乎是一个非常方便的特性，但是它也设置了很多我们可能会陷入的陷阱。例如，如果我们有:

```
1 +'1'
```

我们得到:

```
"11"
```

这不是我们想要的。

JavaScript 具有类型强制，也是因为这种语言原本没有异常，所以在执行无效操作时会返回一些值。这些值的例子包括`Infinity`或`NaN`，当我们将一个数除以 0 或试图将一些没有数字内容的东西分别转换为一个数时，这些值就会返回。

`NaN`代表非数字。

例如，我们得到:

```
+'abc'
```

如果`NaN`因为它试图将字符串`'abc'`转换成数字不成功，所以它没有抛出异常，而是返回`NaN`。

JavaScript 更现代的部分确实会抛出异常。例如，如果我们试图运行:

```
undefined.foo
```

然后我们得到“未捕获的类型错误:无法读取未定义的属性“foo”

另一个例子是在算术运算中混合数字和 BigInt 操作数:

```
6 / 1n
```

然后我们得到“未捕获的类型错误:不能混合 BigInt 和其他类型，使用显式转换。”

# JavaScript 类型强制是如何工作的？

类型强制在 JavaScript 解释器中完成。几乎所有的浏览器都内置了这样的功能。我们有`Boolean`用于将值转换为布尔值，`Number`用于将值转换为数字等等。

# 避免类型强制陷阱

为了避免落入类型强制导致的陷阱，我们应该检查对象的类型，并在对它们进行操作之前将其转换为相同的类型。

## 数字

例如，我们使用`Number`函数将任何东西转换成数字。例如，我们可以如下使用它:

```
Number(1) // 1
Number('a') // NaN
Number('1') // 1
Number(false) // 0
```

`Number`函数将任何类型的对象作为参数，并尝试将其转换为数字。如果不能，那么它将返回`NaN`。

我们还可以在变量或值前面使用`+`操作符，尝试将其转换为数字。例如，我们可以写:

```
+'a'
```

然后我们得到`NaN`。如果我们写:

```
+'1'
```

那么我们得到 1。

![](img/a9826dc0a20edbd7ed499b5fc7b04aa0.png)

Photo by [Francesco De Tommaso](https://unsplash.com/@detpho?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 线

要将对象转换成字符串，我们可以使用`String`函数。它还接受一个对象，并尝试将其转换为字符串。

如果我们传入一个对象，我们返回:

```
"[object Object]"
```

例如，写:

```
String({})
```

会帮我们找到的。

原始值将得到与原始值内容相同的字符串。例如，如果我们写:

```
String(123)
```

我们得到`“123”`。

除了那些我们专门移除原型的对象，所有的对象都有一个`toString`方法。

例如，如果我们写:

```
({}).toString()
```

我们把`“[object Object]”`拿回来。

如果我们写:

```
2..toString()
```

然后我们回到`“2”`。注意，我们有两个点，因为第一个点将数字指定为数字对象，然后第二个点让我们调用数字对象上的方法。

其他涉及字符串的不可思议的转换包括:

```
"number" + 1 + 3        // 'number13'
1 + 3 + "number"        // '4number'
"foo" + + "bar"         // 'fooNaN'
{}+[]+{}                // '[object Object][object Object]'
!+[]+[]+![]             // 'truefalse'
[] + null + 2           // 'null2'
```

## `Symbol.toPrimitive`

对象也有将对象转换成相应原始值的`Symbol.toPrimitve`方法。当使用`+`一元操作符或者将一个对象转换为原始字符串时，就会调用这个函数。例如，我们可以编写自己的`Symbol.toPrimitive`方法来将各种值转换为原始值:

```
let obj = {
    [Symbol.toPrimitive](hint) {
        if (hint == 'number') {
            return 10;
        }
        if (hint == 'string') {
            return 'hello';
        }
        if (hint == 'true') {
            return true;
        }
        if (hint == 'false') {
            return false;
        }
        return true;
    }
};
console.log(+obj);     
console.log(`${obj}`); 
console.log(!!obj);
console.log(!obj);
```

然后我们得到:

```
10
hello
true
false
```

来自代码底部的`console.log`语句。

## 避免松散的平等

宽松相等比较由`==`运算符完成。它通过在比较前转换为相同的类型来比较两个操作数的内容是否相等。举个例子，

```
1 == '1'
```

将评估为`true`。

一个更令人困惑的例子是:

```
1 == true
```

因为`true`是真的，在比较它们之前，它将首先被转换成一个数字。所以`true`在比较之前会先转换为 1，使得表达式为真。

为了避免这种混乱的情况，我们使用了`===`比较操作符。

然后

```
1 === '1'
```

和

```
1 === true
```

都将是`false`，这更有意义，因为它们的类型不同。`===`操作符不会对操作数进行类型强制。比较类型和内容。

我们上面提到的比较问题适用于原始值。对象通过它们的引用进行比较，所以如果操作数有不同的引用，那么无论我们使用哪个操作符，它的计算结果都是`false`。

使用这些函数，我们将变量和值转换为我们显式编写的类型。这使得代码更加清晰，我们不必担心 JavaScript 解释器试图将东西转换成我们不想要的类型。同样，我们应该使用`===`操作符而不是`==`操作符来比较原始值。