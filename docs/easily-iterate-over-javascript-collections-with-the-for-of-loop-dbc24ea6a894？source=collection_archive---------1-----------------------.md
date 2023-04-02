# 使用 For-Of 循环轻松迭代 JavaScript 集合

> 原文：<https://javascript.plainenglish.io/easily-iterate-over-javascript-collections-with-the-for-of-loop-dbc24ea6a894?source=collection_archive---------1----------------------->

![](img/8cbd74b86807f54c93864855d14c98a0.png)

Photo by [Tine Ivanič](https://unsplash.com/@tine999?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

从 ES2015 开始，我们有了一种新的循环来循环遍历可迭代对象。新的`for...of`循环是一种新的循环，它允许我们循环遍历任何可迭代的对象，而不使用常规的`for`循环、`while`循环，或者在数组的情况下使用`forEach`函数。它可以直接用于迭代任何可迭代对象，包括内置对象如字符串、数组、类似数组的对象如`arguments`、`NodeList`、`TypedArray`、`Map`、`Set`以及任何用户定义的可迭代对象。用户定义的可迭代对象包括像生成器和迭代器这样的实体。

如果我们想使用`for...of`循环来迭代一个 iterable 对象，我们可以用下面的语法来写:

```
for (variable of iterable){
  // run code
}
```

上面代码中的`variable` 是代表被迭代的 iterable 对象的每个条目的变量。可以用`const`、`let`或`var`声明。`iterable`是属性被迭代的对象。

例如，我们可以使用它来迭代数组，如下面的代码所示:

```
const arr = [1,2,3];

for (const num of arr) {
  console.log(num);
}
```

上面的代码中，`console.log`语句将记录 1、2 和 3。如果我们想在`for...of`循环中分配用于迭代的变量，我们可以用`let`代替`const`。例如，我们可以写:

```
const arr = [1,2,3];

for (let num of arr) {
  num *= 2 ; 
  console.log(num);
}
```

上面的代码中，`console.log`语句将记录 2、4 和 6，因为我们使用了`let`关键字来声明`num`，所以我们可以通过将每个条目乘以 2 来修改`num`。我们不能用`const`重新赋值，所以我们必须使用`let`或`var`来声明我们想要在每次迭代中修改的变量。

我们也可以迭代字符串。如果我们这样做，在每次迭代中，我们都会得到字符串中的每个字符。例如，如果我们有下面的代码:

```
const str = 'string';

for (const char of str) {
  console.log(char);
}
```

然后我们得到每一行中记录的`'string'`的单个字符。

同样，我们可以迭代 TypedArrays，它包含由一系列十六进制格式的数字表示的二进制数据。例如，我们可以编写以下代码:

```
const arr = new Uint8Array([0x00, 0x2f]);

for (const num of arr) {
  console.log(num);
}
```

在上面的例子中，`console.log`将记录 0 和 47。请注意，记录的值是十进制格式，但输入的值是十六进制格式。

如果我们在地图上迭代，那么我们得到地图的每个条目。例如，我们可以编写以下代码来迭代地图:

```
const map = new Map([['a', 2], ['b', 4], ['c', 6]]);

for (const entry of map) {
  console.log(entry);
}
```

如果我们记录条目，我们得到`['a', 2]`、`['b', 4]`和`['c', 6]`。映射由键值对作为它们的条目组成。当我们在一个 Map 上迭代时，每个条目的第一个元素是键，第二个元素是值。要将每个条目的键和值放入它自己的变量中，我们可以使用析构运算符，如下面的代码所示:

```
const map = new Map([['a', 2], ['b', 4], ['c', 6]]);

for (const [key, value] of map) {
  console.log(key, value);
}
```

然后当我们记录条目时，我们得到`'a' 2`、`'b' 4`和`'c' 6`。

我们也可以对集合使用`for...of`循环。例如，我们可以通过执行以下操作来循环一个集合:

```
const set = new Set([1, 1, 2, 3, 3, 4, 5, 5, 6]);

for (const value of set) {
  console.log(value);
}
```

我们设置记录 1、2、3、4、5 和 6，因为集合构造函数通过保留集合中第一个出现的值并丢弃后面出现的相同值来自动消除重复条目。

`for...of`循环还用于迭代`arguments`对象，这是一个全局对象，当函数被调用时，它具有传递给函数的参数。例如，如果我们编写以下代码:

```
(function() {
  for (const argument of arguments) {
    console.log(argument);
  }
})(1, 2, 3, 4, 5, 6);
```

我们看到记录了 1、2、3、4、5 和 6，因为这是我们调用函数时传入的。注意，这只适用于常规函数，因为`this`的上下文必须改为被调用的函数，而不是`window`。箭头函数不会改变`this`的内容，所以当我们在箭头函数中运行相同的循环时，我们不会得到正确的参数。

此外，我们可以迭代一系列 DOM 节点对象，称为`NodeList`。例如，一个浏览器实现了`NodeList.prototype[Symbol.iterator]`，那么我们可以像在下面的代码中一样使用`for...of`循环:

```
const divs = document.querySelectorAll('div');

for (const div of divs) {
  console.log(div);
}
```

在上面的代码中，我们记录了文档中所有的`div`元素。

对于`for...of`循环，我们可以通过使用`break`、`throw`或`return`语句来结束循环。在这种情况下，迭代器将关闭，但执行将在循环之外继续。例如，如果我们写:

```
function* foo(){ 
  yield 'a'; 
  yield 'b'; 
  yield 'c'; 
}; 

for (const o of foo()) { 
  console.log(o); 
  break;
}console.log('finished');
```

在上面的代码中，我们只记录了' a ',因为我们在`for...of`循环的末尾有一个 break 语句，所以在第一次迭代后，迭代器将关闭，循环结束。

![](img/28a22facd094707db34d4b261560dad4.png)

Photo by [Charlotte Coneybeer](https://unsplash.com/@she_sees?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以在生成器上循环，生成器是返回一个生成器函数的特殊函数。generator 函数返回 iterable 对象的下一个值。它用于让我们通过在一个`for...of`循环中使用 generator 函数来遍历一个对象集合。

我们也可以对一个生成无限值的生成器进行循环。我们可以在生成器内部建立一个无限循环来不断返回新值。因为`yield`语句直到请求下一个值时才运行，所以我们可以保持无限循环运行，而不会使浏览器崩溃。例如，我们可以写:

```
function* genNum() {
  let index = 0;
  while (true) {
    yield index += 2;
  }
}const gen = genNum();
for (const num of gen) {
  console.log(num);
  if (num >= 1000) {
    break;
  }
}
```

如果我们运行上面的代码，我们会看到记录了从 2 到 1000 的数字。然后`num`大于 1000，这样`break`语句就是 ran。我们不能在生成器关闭后重用它，所以如果我们编写如下代码:

```
function* genNum() {
  let index = 0;
  while (true) {
    yield index += 2;
  }
}const gen = genNum();
for (const num of gen) {
  console.log(num);
  if (num >= 1000) {
    break;
  }
}for (const num of gen) {
  console.log(num);
  if (num >= 2000) {
    break;
  }
}
```

第二个循环不会运行，因为生成器生成的迭代器已经被第一个循环用`break`语句关闭了。

我们可以迭代其他定义了用符号`Symbol.iterator`表示的方法的可迭代对象。例如，如果我们定义了以下 iterable 对象:

```
const numsIterable = {
  [Symbol.iterator]() {
    return {
      index: 0,
      next() {
        if (this.index < 10) {
          return {
            value: this.index++,
            done: false
          };
        }
        return {
          value: undefined,
          done: true
        };
      }
    };
  }
};
```

然后我们可以运行下面的循环来显示日志生成的结果:

```
for (const value of numsIterable) {
  console.log(value);
}
```

当我们运行它时，当`console.log`在上面的循环中运行时，应该会看到记录的 0 到 9。

重要的是我们不要混淆`for...of`循环和`for...in`循环。`for...in`循环用于遍历对象的顶级键，包括原型链上的任何对象，而`for...of`循环可以遍历任何可迭代对象，如数组、集合、映射、`arguments`对象、`NodeList`对象和任何用户定义的可迭代对象。

例如，如果我们有这样的东西:

```
Object.prototype.objProp = function() {};
Array.prototype.arrProp = function() {};const arr = [1, 2, 3];
arr.foo = 'abc';for (const x in arr) {
  console.log(x);
}
```

然后我们记录 0，1，2，' foo '，' arrProp '和' objProp '，它们是为`arr`对象定义的对象和方法的键。它包含了原型链上的所有属性和方法。它从对象和数组继承了所有添加到对象和数组原型的属性和方法，所以我们在`for...in`循环中获得了链继承中的所有东西。只有可枚举的属性以任意顺序记录在`arr`对象中。它记录我们在`Object` 和`Array` 中定义的索引和属性，如`objProp` 和`arrProp`。

为了只遍历不是从对象原型继承的属性，我们可以使用`hasOwnPropetty`来检查属性是否定义在自己的对象上:

```
Object.prototype.objProp = function() {};
Array.prototype.arrProp = function() {};const arr = [1, 2, 3];
arr.foo = 'abc';for (const x in arr) {
  if (arr.hasOwnProperty(x)){
    console.log(x);
  }
}
```

`objProp`和`arrProp`被省略，因为它们分别继承自`Object`和`Array`对象。

`for...of`循环是一种新的循环，它允许我们在不使用常规的`for`循环、`while`循环或者在使用数组的情况下不使用`forEach`函数的情况下遍历任何可迭代的对象。它可以直接用于迭代任何可迭代对象，包括内置对象，如字符串、数组、类似数组的对象，如`arguments`和`NodeList`、`TypedArray`、`Map`、`Set`以及任何用户定义的可迭代对象。用户定义的可迭代对象包括像生成器和迭代器这样的实体。这是一个方便的循环，因为它允许我们遍历任何可迭代的对象，而不仅仅是数组。现在我们有了一个处理 iterable 对象的循环语句。