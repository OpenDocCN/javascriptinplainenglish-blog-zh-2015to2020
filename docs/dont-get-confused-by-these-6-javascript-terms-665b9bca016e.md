# 每个 JavaScript 开发人员都应该知道的 6 个区别

> 原文：<https://javascript.plainenglish.io/dont-get-confused-by-these-6-javascript-terms-665b9bca016e?source=collection_archive---------8----------------------->

## 通过了解这些功能和关键词之间的区别来确定工作面试

![](img/301f7b70dd88d412e2b8ef9bea14c3dc.png)

Photo by [Dex Ezekiel](https://unsplash.com/@dexezekiel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 有很多听起来和执行类似动作的函数和保留字，许多开发人员互换使用这些术语，却不知道它们之间的重要区别。

虽然这不完全是他们的错，因为 JavaScript 不断引入更好的方法来执行操作，但是为了向后兼容，他们倾向于在旧函数旁边添加新函数，而不是删除执行相同功能的已有函数。

在 2015 年之前，只有一种方法来声明变量，那就是使用关键字`var`。没有`let`或`const` 。

面试官很想知道候选人是否了解这些最新的进展，以测试候选人的知识有多新、多深，以及他/她对执行类似操作的 JavaScipt 术语的掌握程度。

下面是你在下一次技术面试中应该知道的 6 个区别。

## 1.let vs var

`let`和`var`都是在声明变量时使用的关键字。虽然`var` 和`let`都用于 JavaScript 中的变量声明，但还是有区别的。

JavaScript 从 2015 年 ES6 版本发布后才开始支持`let` ，而`var` 之前就存在了，并且是在`let`出现之前声明变量的唯一方式。

另一个区别是变量作用域，即程序的哪些部分可以访问和使用它。`var`是函数作用域，`let`使用块作用域。

为了更好地理解，我将分享一个相关的例子。

```
var a=10;
function myFun(){
for (var i = 0; i < 2; i++) {     
console.log(`Inside the loop: ${i}`); 
} 
console.log(`Outside the loop: ${i}`);
}
myFun();
console.log(i)
```

以下内容的输出将是:

```
Inside the loop: 0Inside the loop: 1Outside the loop: 2error: Uncaught ReferenceError: i is not define
```

正如你所看到的，我们甚至可以在 for 循环之外的同一个函数中使用变量`i`的值。这是因为它是使用 var 声明的，这意味着它将遵循函数作用域。由于变量`a`是在函数外部定义的，我们可以在全局范围内使用它。

现在我们来看看`let`的范围。

```
for (let i = 0; i < 2; i++) {
    console.log(`Inside the loop: ${i}`);
}console.log(`Outside the loop: ${i}`);
```

输出:

```
Inside the loop: 0
Inside the loop: 1
error: Uncaught ReferenceError: i is not defined
```

输出是不同的，因为`let`遵循块范围，这意味着它只能在声明它的 for 循环中看到和使用，而不能在此之外。

你可以在这里找到一个更详细的例子[。](https://www.geeksforgeeks.org/difference-between-var-and-let-in-javascript/)

## 2.静止与扩散

rest 和 spread 操作符在语法上看起来非常相似。但两者都用于执行本质上完全相反的操作。

一个好的开发人员应该能够停止差异，并知道何时使用 rest 和 spread 运算符。

当我们不知道可能收到的参数数量时，在接受参数时使用 Rest 运算符。

例如，考虑下面的例子:

```
function sumAll(...args) { // args is the name for the array
  let sum = 0; for (let arg of args) 
     sum += arg; return sum;
}console.log( sumAll(5) ); // 5
console.log( sumAll(5, 10) ); // 15
console.log( sumAll(5, 10, 15) ); //30
```

我们使用 rest 操作符接受未知数量的参数，如`…args`所示，其中“args”是数组名。

尽管展开运算符在语法上看起来相似，但它们的行为却不同。spread 运算符允许数组/字符串/对象扩展成一个参数列表。

因此，它服务于与 rest 操作符完全相反的目的。

```
const arr = ["2", "3", "4"];
const newArr = ["1", ...arr];
console.log(newArr)
```

我们在数组`arr`上使用了 spread 操作符，将数组的元素添加到名为`newArr`的新数组中。我们的代码片段将把`[“1”,”2”,”3”,”4”]`记录到控制台。

## 3.函数与方法

大多数程序员交替使用术语“函数”和“方法”,但是存在一个微妙但明显的区别。

在 JavaScript 中，一切都被认为是一个对象。字符串是对象，数组也是对象。函数是一个对象，而方法是属于另一个对象的函数。

简而言之，要调用一个方法，我们需要引用它所属的对象，有时也称为“接收者”对象，而对于函数，我们可以直接调用函数。

我知道这可能会令人困惑，但希望下面的代码片段能让你有一个清晰的理解。

```
var myObject= {
  myMethod: function() {
    console.log("Method");
  }
};
myObject.myMethod;

var myFunction = ()=>{ 
    console.log("function)
}
myFunction();
```

要调用`myMethod()`，我们必须首先引用`myObject`，在本例中，T5 是声明方法的对象。另一方面，我们可以在没有任何对象引用的情况下调用`myFunction()`。

## 4.切片与拼接

切片和拼接是对数组执行某些操作的方法。作为一名开发人员，当我开始我的编码之旅时，我总是认为他们执行相同的操作，只有最小的差异。

但是这两种方法非常不同，尽管它们听起来很相似。简而言之，切片方法可以比作字符串的“子串”方法。slice 方法检索数组的元素，并接受两个参数——要检索的元素的起始和结束索引。

```
const arr = ["Christmas", "Halloween", "New Year"];console.log(arr.slice(0,2)) // --> ["Christmas","Halloween"]
console.log(arr) // --> ["Christmas", "Halloween", "New Year"]
```

我让控制台再次记录数组“arr”的原因是为了强调 slice 方法对原始数组没有任何影响。

另一方面，拼接方法接受更多的参数并影响原始数组。

拼接方法执行阵列中元素的移除和替换。拼接方法至少采用 2 个参数，即开始索引和要从该开始索引中删除的元素数量。

但是，您可以将您想要插入的值作为参数传递。您传递的值将被添加到您作为第一个参数提供的开始索引之后。

```
const arr = ["Christmas", "Halloween", "New Year"];console.log(arr.splice(0,1))
console.log(arr);
arr.splice(0,1,"Chistmas");
console.log(arr)
```

上述代码片段的输出如下所示:

```
["Christmas"]["Halloween","New Year"]["Chistmas","New Year"]
```

如果您仔细观察，您可以看到拼接方法返回它已移除的元素，如第一个控制台日志所示，其中它返回数组的第一个元素。

另一个收获是，当我们第二次记录数组时，我们看到拼接方法确实从原始数组中移除了第一个元素。

因此，切片和拼接方法用于完全不同的目的。

## 5.三重等号(===)对双等号(==)

了解这两者之间的区别非常有用，不仅在面试中，在构建复杂的应用程序时也是如此。JavaScript 的优势之一是类型推断，它可以为我们确定类型。

例如，如果我们将数字 1 存储在一个变量中，它会自动确定它是一个数字，如果我们尝试将“1”存储在一个变量中，它可以推断类型是 String。

虽然这种初学者友好的方法受到欢迎，但有时我们需要匹配类型，以获得无错误和健壮的代码。

这就是知道三倍和两倍等号之间的区别的地方。三重等号等于匹配类型以及值，而双等号正好匹配值。如下面提供的代码片段所示:

```
let number = 1234;let stringNumber = '1234';console.log(number == stringNumber); *//true*console.log(number === stringNumber);  *//false*
```

这里，“1234”是一个字符串，而 1234 是一个数字。tripe 运算符符号根据类型以及所示进行区分，而 double 等号只匹配值，而不匹配类型。

事实上，当练习 JavaScript 的[现代规则](https://medium.com/javascript-in-plain-english/5-modern-practices-of-javascript-that-every-developer-should-know-1a61dc9a6ee0)时，大多数林挺软件会将双等号标记为不良练习的警告。

## 6.Map 与 forEach 方法

使用这两种方法可以帮您省去很多麻烦，同时也有利于可读性。虽然`map()`和`forEach()`都在阵列上工作，但它们都用于不同的目的。

这两种方法都将一个函数作为参数，并将该函数应用于数组的每个元素。然而，它们返回不同的值。forEach 方法总是返回未定义的，而 map 方法在执行该函数后返回一个新的数组。

由于这个原因，`map()`变成了可链接的，也就是说，你可以在它后面附加其他的方法，比如`sort()`和`reduce()`。这对于 forEach 方法是不可能的，因为它返回一个未定义的值。

```
var arr= [3,2,1];
var doubled= arr.map(currentValue => currentValue*2).sort();
console.log(doubled) // --> [2,4,6]var Val= arr.forEach(x=>x*x)
console.log(Val) // --> undefinedconsole.log(arr) //--> [3,2,1]
```

在上面的代码片段中，我们在数组“arr”上使用了`map()`，还链接了`sort()`方法，并将结果数组存储在“doubled”中。

然后，我们使用 `forEach()`对数组的每个元素求平方，并将返回值存储在变量‘Val’中，如控制台日志所示，该变量包含一个未定义的值。

最后，为了强调没有任何东西影响原始数组，我也记录了它。

然而，仅仅因为您可以使用`map()`来执行 forEach 方法的任务以及更多任务，并不会削弱 forEach 方法的价值。forEach 方法为 for 循环提供了一个简单的替代方法，如下所示:

```
var arr= [3,2,1];
for(let i=0;i<arr.length;i++)
    console.log(arr[i]**2);console.log('-------------')
arr.forEach(x=>console.log(x**2))
```

输出如下所示:

```
941-------------941
```

因此，如果您只想遍历元素而不需要存储新形成的数组，那么使用 forEach 方法是最好的方法，否则您可以使用 map 方法。

## 最后的想法

JavaScript 正在发展以满足市场创造的新需求。掌握这样一种流行的语言可以深刻地影响你的程序员生涯。

因此，保持与 JavaScript 领域的最新发展保持联系变得越来越重要，这有助于让你的职业生涯走上正轨。有些方法听起来和执行起来类似，比如 forEach 和 Map 方法，但是有细微的差别，一个熟练的开发人员应该能够利用这些差别。

此外，了解一些最广泛使用的方法和操作者的差异和用例，对于破解技术性工作面试并获得相对于其他人的优势至关重要。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**