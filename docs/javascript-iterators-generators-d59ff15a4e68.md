# JavaScript:迭代器和生成器

> 原文：<https://javascript.plainenglish.io/javascript-iterators-generators-d59ff15a4e68?source=collection_archive---------5----------------------->

## 遍历“元素集”中的每个元素是一种非常普通的操作。JavaScript 为我们提供了很多方法。

## 但是，在本文中，我们不会爱上那些方法。相反，我们将探索这一切背后的机制，即概念:iterable、iterator、generator。

![](img/1e633551f96d5f3b097a4c5ced6b57f3.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 可迭代的

如果一个对象安装了可迭代协议，那么这个对象就叫做*可迭代*(稍后再谈)。这些可迭代对象经常可以使用元素浏览方法，例如`for..of`。

在 JavaScript 中，`Array`是*可迭代的*。

```
for (let v of [1, 2, 3, 4]) console.log(v)
// 1
// 2
// 3
// 4
```

此外，许多其他内置对象是可迭代的，例如`String`:

```
for (let v of 'hello') console.log(v)
// h
// e
// l
// l
// o
```

如果一个对象代表一组“元素”，那么我们完全可以用 for..循环遍历其元素。浏览这些元素的操作称为“迭代”(类似于许多其他语言)。

但是你有没有想过这样的事情是如何运作的？不需要等太久，我们很快就会知道。

# 可迭代协议

Iterable 协议是一个允许 JavaScript 对象自定义自己的浏览操作的协议，这些操作将在元素审批中使用(例如，`for..of`)。

JavaScript 的许多内置数据类型已经安装了可迭代协议(例如，`Array`，`Map`)，这使得我们可以轻松地遍历它的元素。

只有实现了 iterable 协议的对象才是 iterable 的。安装将通过`@@iterator`方法完成。也就是说，对象必须具有`@@iterator`属性。可通过`[Symbol.iterator]`设置该属性，如下所示:

```
let range = {
	from: 1,
	to: 5,
	[Symbol.iterator]: () => {
		return {
			current: this.from,
			last: this.to,
			next: () => {
				if (this.current <= this.last) {
					return { done: false, value: this.current++ };
				} else {
				return { done: true };
			}
		}
	};
}
```

`@@iterator`属性(通过`[Symbol.iterator]`设置)是一个没有参数的函数，在我们可以浏览元素之前，它的结果必须是一个迭代器(稍后会学到)。一个对象的。实际上，这个函数可以返回任何东西，但是如果它不是迭代器，那么我们一浏览元素就会得到一个错误，我认为没有人会在为整个对象设置属性`@@iterator`时做这种“愚蠢”的事情。

每当一个对象需要遍历它的元素时，例如使用`for..of`。这些方法首先调用该对象的`@@iterator`(不带参数)，然后使用该方法的结果返回迭代。在上面的例子中，我们可以通过自己定义方法`@@iterator`来完全构建我们自己的可迭代对象。通过自定义方法，我们可以完全自定义元素迭代的方法。

和上面的例子一样，我们将浏览元素 1–5(实际上，这些“元素”并不完全在对象中，但是由于定制了 browse 方法，我们已经完全遍历了这些元素)。

```
for(let num of range) console.log(num)
// 1
// 2
// 3
// 4
// 5
```

浏览方法，如..只处理返回的迭代器。注意，这个迭代器可能是一个完全不同的对象，而不是一个批准的对象，这取决于每个特定对象的设置。

# 迭代程序

在 JavaScript 中，迭代器是一个对象，它定义了一个“字符串”元素，并将返回每个元素，直到遇到某个端点。更精确的定义是，迭代器是迭代器协议实现的对象。

迭代器协议完全独立于 iterable 协议，一个对象不一定要安装在两种协议上。这也意味着可迭代对象不一定是迭代器，反之亦然。

**迭代器协议** 迭代器协议是一种协议，它定义了标准方法来“生成”一个结果序列(可以是有限的，也可以是无限的)，并依次返回结果供我们在迭代中使用。

这是将与从`@@iterator`属性返回的迭代器的`for..of`一起使用的协议。

如果一个对象定义了迭代器协议，它就是迭代器。这个协议很简单，它要求对象必须用下一个方法(没有参数)安装，结果是返回的对象具有以下两个属性:

*   `done (boolean`:表示迭代器是否结束。还有其他值可以浏览的时候就是`false`了。如果该值为`false`，则需要下一个属性。
*   `value`:下一次浏览的返回值。该属性的值可以是任何 JavaScript 对象。

`next`方法必须总是返回一个具有上述两个属性的对象(如果`done`是`true`，则可能不需要`value`)。如果返回的不是上面描述的对象，我们在执行元素浏览时会得到一个`TypeError`错误。

事实上，很难知道一个对象是否安装了迭代器协议。但是我们可以很容易地构建一个安装了这个协议的对象。

如上所述，迭代器对象不一定是可迭代的，反之亦然，但是如果我们有意构建一个对象，我们可以同时安装可迭代协议和迭代器协议。那么，我们的对象将既是一个可迭代对象，又是一个迭代器。

**自己构建迭代器** JavaScript 的内置迭代器之一，Array 就是一个最好的例子。这个迭代器简单地返回其中每个元素的值，我们将相应地浏览它们。

我们可以想象迭代器类似于数组。但事实上不是那样的。迭代器完全可以自由运行(甚至没有停顿)，取决于如何安装自己的迭代器协议。

下面是一个自建迭代器的例子。它构造了一个给定范围内的值序列。

我们甚至可以把迭代器协议和 iterable 协议结合起来，这样我们都可以构造一个迭代器，我们可以用`for..of`迭代它的元素。

```
var myIterator = {
    next: function() {
        // ...
    },
    [Symbol.iterator]: function() { return this }
};
```

# 发电机

构建自己的迭代器是一项非常酷且有效的工作。但是这要求我们在设置 iterable 协议和 iterator 协议时非常小心。因为我们必须手动管理当前状态以及计算下一个值。

但是 JavaScript 有另一个工具，生成器函数，它将帮助我们在这方面更加轻松。生成器能够使用一个函数帮助我们定义一系列要浏览的值。

从语法上来说，生成器函数是由关键字`function*`定义的，这与常规函数有点不同。发电机性能也大不相同。当被调用时，它实际上并不立即执行。相反，它返回一个对象，我们可以称之为生成器。

**生成器是迭代器**

在`next`方法上将自动定义一个生成器，因此它将自动成为一个迭代器。即使这是全自动的，我们也不需要担心管理生成器的状态和值。

每次方法为`next`时，实际执行此时的生成器函数，直到遇到`yield`才会停止。而`yield`的值也是`next`将要返回的值。

回到上面自建迭代器的例子，我们可以很容易地用生成器重写它，如下所示:

```
function* makeRangeIterator(start = 0, end = Infinity, step = 1) {
    let iterationCount = 0;
    for (let i = start; i < end; i += step) {
        iterationCount++;
        yield i;
    }
}
```

用预定义的`next`方法，我们可以调用如下:

```
range = makeRangeIterator(1, 10, 2);
range.next()
// {value: 1, done: false}
range.next()
// {value: 1, done: false}
```

**生成器是可迭代的** 不仅是一个迭代器，*生成器*也是一个可迭代的对象。也就是会自动定义为既有 iterable 协议又有 iterator 协议，方便我们。由于协议的实现，我们可以使用元素批准，类似于其他常规的 *iterables* :

```
for (let num in makeRangeIterator(1, 10, 2)) console.log(num)
// 1
// 3
// 5
// 7
// 9
```

你可以任意多次调用一个生成器函数。而且每次调用都会返回不同的生成器，可以单独迭代，不影响其他调用。

但是，与自建迭代器不同，我们不能定制生成器遍历。每个生成器只能浏览一次，每个值只能浏览一次。当然，我们可以多次调用生成器函数，但每次调用只能浏览一次。

由于我们的优势，我们可以考虑使用生成器，而不是需要手动构建的可迭代迭代器。因为很明显，使用发电机更方便也更安全。

**生成器生成另一个生成器** 这是一个生成器的特殊方法，我们可以将这个生成器“嵌入”到另一个生成器中。然后，我们需要一个特殊的语法:`yield*`:

```
function* generateSequence(start, end) {
    for (let i = start; i <= end; i++) yield i;
}

function* generatePasswordCodes() {

    // 0..9
    yield* generateSequence(48, 57);

    // A..Z
    yield* generateSequence(65, 90);

    // a..z
    yield* generateSequence(97, 122);

}

let str = '';

for(let code of generatePasswordCodes()) {
    str += String.fromCharCode(code);
}
console.log(str):
// 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```

上面的`yield*`语法允许我们调用另一个生成器，并在执行下一个计算之前等待该生成器完成。如果不使用这种方法，我们必须构建一个更复杂的生成器，如下所示:

```
function* generateSequence(start, end) {
    for (let i = start; i <= end; i++) yield i;
}

function* generateAlphaNum() {

    // yield* generateSequence(48, 57);
    for (let i = 48; i <= 57; i++) yield i;

    // yield* generateSequence(65, 90);
    for (let i = 65; i <= 90; i++) yield i;

    // yield* generateSequence(97, 122);
    for (let i = 97; i <= 122; i++) yield i;

}

let str = '';

for(let code of generateAlphaNum()) {
    str += String.fromCharCode(code);
}
console.log(str);
// 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```

另一个生成器的 yield 给了我们一个更简单的方法，把一个生成器嵌入到一个生成器中。

**为产量赋值**

从一开始，我们仍然以非常自然的方式使用生成器，进而`yield`生成器的每个值。许多人也在做同样的事情。但是生成器非常灵活，允许我们做更多的事情。

那个 yield 也能从外部认识到价值，而不是在返回价值的土丘。考虑下面一个简单的例子，我们使用这个技巧从头开始重置字符串:

```
function* foo() {
    let index = 0;
    while (true) {
        const result = yield index++;
        if (result) {
            index = 0;
        }
    }
}

const bar = foo();

console.log(bar.next())
// {value: 0, done: false}
console.log(bar.next())
// {value: 1, done: false}
console.log(bar.next(true))
// {value: 0, done: false}
console.log(bar.next())
// {value: 1, done: false}
```

Generator 还有一个非常大的应用程序，它是带有`[async/await](https://manhhomienbienthuy.github.io/2018/Jul/17/javascript-asyncawait.html)`关键字的异步代码的基础。

# 总结

迭代器和生成器是 JavaScript 中非常重要的概念。希望这篇文章能提供有用的信息，帮助你更好地掌握 JavaScript 代码。👏