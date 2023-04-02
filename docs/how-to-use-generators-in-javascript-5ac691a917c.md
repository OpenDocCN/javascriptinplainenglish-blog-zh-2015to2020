# 如何在 JavaScript 中使用生成器

> 原文：<https://javascript.plainenglish.io/how-to-use-generators-in-javascript-5ac691a917c?source=collection_archive---------7----------------------->

## 了解如何在 JavaScript 中使用生成器关键字

![](img/1e633551f96d5f3b097a4c5ced6b57f3.png)

Photo by Caspar Camille Rubin on Unsplash

今天，我们将了解 JavaScript 中生成器的功能。对于经常使用 JavaScript 进行开发的开发人员来说，这个概念是未知的。

# 发电机

生成器是一种不同于普通函数的函数。它有独特的功能被激发和重新进入，它的变量在重新进入中被保留。

有了 JavaScript 中的 ES6，我们知道了箭头函数、运算符等基本功能..，让我们深入了解生成器的概念，看看它是如何工作的。如果我们可以执行一个常规的函数，解释器会重复执行代码，直到函数结束。这被称为从运行到完成的模型。

让我们看一个例子，

```
function regularFunction() {
    console.log("I'm a regular function")
    console.log("Surprise surprice")
    console.log("This is the end")
}

regularFunction()

-----------------
Output
-----------------
I'm a regular function
Surprise surprice
This is the end
```

正如您在上面的代码中所看到的，这是一个常规的函数，它会重复执行，直到到达末尾或返回一个值。如果我们想在某个时候停止这个功能并继续。我们如何做到这一点？这就是发电机的特性。

# 第一发生器功能

```
function* generatorFunction() {
    yield "This is the first return"
    console.log("First log!")
    yield "This is the second return"
    console.log("Second log!")
    return "Done!"
}
```

在执行上述函数之前，您必须了解什么是函数？它的语法以及如何将它们声明为生成器。除此之外，你还遇到了屈服这个词。所以你也会知道产量的作用。收益不算什么，但它将暂停执行，并将所有函数保存在其状态中，然后从我们暂停的地方继续。

让我们看一个例子，

```
generatorFunction()

-----------------
Output
-----------------
generatorFunction {<suspended>} {
    __proto__: Generator
    [[GeneratorLocation]]: VM272:1
    [[GeneratorStatus]]: "suspended"
    [[GeneratorFunction]]: ƒ* generatorFunction()
    [[GeneratorReceiver]]: Window
    [[Scopes]]: Scopes[3]
}
```

从上面的例子中，如果我们调用一个生成器函数，它不会被自动触发。相反，它返回一个迭代器对象。该对象定义当调用方法 **next()** 时，生成器函数体被执行，直到它返回第一个表达式。让我们看看它是如何工作的。

```
const myGenerator = generatorFunction()
myGenerator.next()

-----------------
Output
-----------------
{value: "This is the first return", done: false}
```

正如我们之前看到的，生成器将重复执行，直到第一个 yield 语句返回一个对象。生成的对象包含一个**值**属性和一个 **done** 属性。

```
{ value: ..., done: ... }
```

**价值** -它是一种财产，等于我们付出多少的价值。

**Done** -这是一个具有布尔值的属性，只有当生成器函数返回一个值时，该属性值才设置为真。

让我们再次调用 **next()** 函数。

```
myGenerator.next()

-----------------
Output
-----------------
First log!
{value: "This is the second return", done: false}
```

此时，我们可以在**控制台看到，日志**生成器主体被执行并返回**第一个日志**！然后第二个物体屈服了。让我们继续同样的过程。

```
myGenerator.next()

-----------------
Output
-----------------
Second log!
{value: "Done!", done: true}
```

现在第二条 **console.log** 语句被执行，我们得到一个新的返回对象，但是这次 **done** 属性被执行并设置为 true。

# 产生迭代器

在我们深入探讨这个问题之前，还有一个函数叫做 **yield*** 。它定义了它将允许我们通过创建一个新的函数来迭代一个数组。让我们试试，

```
function* yieldArray(arr) {
    yield arr
}

const myArrayGenerator1 = yieldArray([1, 2, 3])
myArrayGenerator1.next()

-----------------
Output
-----------------
{value: Array(3), done: false}
```

但是我们不能得到我们所期望的，我们希望得到数组中的每个元素。所以让我们尝试一些更有趣的。

```
function* yieldArray(arr) {
    for (element of arr) {
        yield element
    }
}

const myArrayGenerator2 = yieldArray([1, 2, 3])
myArrayGenerator2.next()
myArrayGenerator2.next()
myArrayGenerator2.next()

-----------------
Output
-----------------
{value: 1, done: false}
{value: 2, done: false}
{value: 3, done: false}
```

现在我们得到了它，但我们仍然可以做得更好。让我们再试一次。

```
function* yieldArray(arr) {
    yield* arr
}

const myArrayGenerator3 = yieldArray([1, 2, 3])
myArrayGenerator3.next()
myArrayGenerator3.next()
myArrayGenerator3.next()

-----------------
Output
-----------------
{value: 1, done: false}
{value: 2, done: false}
{value: 3, done: false}
```

太棒了。通过使用 **yield*** 表达式，我们可以迭代操作数并产生数组中的每个值并返回给它。我们可以将这个策略应用到其他生成器、数组、字符串等。

# 发电机的重要性

重要的是，它们的计算速度非常慢，这是因为在我们调用 **next()** 方法后，该值将被返回，只有在我们调用它时才会被计算。这为解决多个场景提供了更好的性能。

# 生成无限序列

我们可以使用生成器生成无限数量的序列。让我们用质数来做这个。在这种情况下，我使条件 **i > =10** 从循环中退出。否则会永远迭代下去。

```
function* infiniteSequence() {
    let num = 0
    while (true) {
        yield num
        num += 1
    }
}

for(i of infiniteSequence()) {
    if (i >= 10) {
        break
    }
    console.log(i)
}

-----------------
Output
-----------------
0
1
2
3
4
5
6
7
8
9
```

# 实现迭代器

为了实现 iterable，首先您必须使用 **next()** 方法手动创建一个对象，并将其保存到状态中。

让我们用一个例子来说明，我们想要的输出是 **I** ，**是**，**可迭代的。**不使用发电机进行此项操作。

```
const iterableObj = {
  [Symbol.iterator]() {
    let step = 0;
    return {
      next() {
        step++;
        if (step === 1) {
          return { value: 'I', done: false};
        } else if (step === 2) {
          return { value: 'am', done: false};
        } else if (step === 3) {
          return { value: 'iterable.', done: false};
        }
        return { value: '', done: true };
      }
    }
  },
}
for (const val of iterableObj) {
  console.log(val);
}

-----------------
Output
-----------------
I
am
iterable.
```

使用发电机也可以做到这一点。

```
function* iterableObj() {
    yield 'I'
    yield 'am'
    yield 'iterable.'
}

for (const val of iterableObj()) {
  console.log(val);
}

-----------------
Output
-----------------
I
am
iterable.
```

# 限制

我们只能访问生成器对象一次。一旦用尽，你就不能再迭代了。如果你想这样做，你必须创建一个新的生成器对象。

这些对象不允许随机访问实例、数组。如果你想得到它，你必须调用所有的 **next()** 函数手动进入你得到想要的输出。

# 结论

我希望你喜欢，现在你将知道 JavaScript 中的生成器的功能。您可以在您的项目中实现这个概念。

感谢阅读！