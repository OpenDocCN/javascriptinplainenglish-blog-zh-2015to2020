# 现代 JavaScript 的技巧和窍门

> 原文：<https://javascript.plainenglish.io/tips-and-tricks-for-modern-javascript-117be53a511d?source=collection_archive---------2----------------------->

## 现代 JS 的基础使它成为最好的。

![](img/79c9cef3b175552370e426581bb25e99.png)

Photo by [ian dooley](https://unsplash.com/@sadswim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

成为一名 JavaScript 开发人员并不是一件容易的事情。这是一场战斗。永无止境的研究。然而我们选择了痛苦。有丰富的教程和大师来学习它。但是最好的学习方法是自我激励。你对学习的执着。

不谈大道理，让我们开始你为什么在这里。

# 如何阻止‘const’变异？

ES6 中的 const 关键字是一个只读值。这是无法改变的。但是你确定吗？打开控制台，执行以下操作:

```
const val = [1, 2, 3];
// val gives [1,2,3] const val = [2, 3, 4];
// gives errorval[0] = 4;
val[1] = 5;
val[2] = 6;// val give [4, 5, 6]
```

最初的值[1，2，3]变成了[4，5，6]或者说是突变的。这里有一个简单的解决办法，防止它变异。

```
const val = [1, 2, 3];
Object.freeze(val);val[0] = 4;
val[1] = 5;
val[2] = 6;// val gives [1, 2, 3]
```

这是一个简单的解决方案。

# 如何将函数转换成箭头函数？

这很简单，你可能已经知道了。对于没有参数和单个 return 语句的函数，我们可以执行以下操作:

```
var greetings = function() {
  return console.log("Hello Universe");
};const greetings = () => console.log("Hello Universe");
```

如果一个函数有参数，我们可以这样做:

```
var concat = function(array1, array2) {
     return arr1.concat(array2);
};const concat = (array1, array2) => array1.concat(array2);
```

高阶函数是以其他函数为参数的函数。要了解更多信息，请点击此处。以下是如何将它们更改为 ES6 高阶箭头功能:

```
const numbers = [2, 4, 8];const square = (array) => {
     const squared = array.map(num => num * num);
     return squared;
};const squared = square(numbers);
console.log(squared);
```

# 在 JavaScript 函数中写默认参数？

参数的默认值可以通过以下方式给出(下面的情况下 *val = 5):*

```
const multiplication = (function() {
    return function multiply(num, val = 5) {
    return num * val;
};
})();console.log(multiplication(5, 2));
console.log(multiplication(5));
```

# Rest 操作符如何帮助优化你的代码？

Rest 操作符允许您在函数中接受各种参数。

不带 Rest 运算符:

```
const multi = (function() {
    return function mul(x, y, z) {
     const args = [ x, y, z ];
     return args.reduce((a, b) => a * b);
    };
})();console.log(multi(6, 5, 4));// 120
```

使用 Rest 运算符:

```
const multi = (function() {
    return function mul(...theArgs) {
     return theArgs.reduce((a, b) => a * b);
    };
})();console.log(multi(6, 5, 4));// 120
```

# “扩展运算符”如何在不破坏数组的情况下帮助数组增值？

展开前运算符:

```
const numArray1 = [‘ONE’, ‘TWO’, ‘THREE’, ‘FOUR’];
let numArray2;
    (function() {
     numArray2 = numArray1; // change this line
     numArray1[0] = ‘ZERO’
})();console.log(numARRAY2); // ['ZERO', ‘ONE’, ‘TWO’, ‘THREE’, ‘FOUR’]
```

后扩展运算符

```
const numArray1 = [‘ONE’, ‘TWO’, ‘THREE’, ‘FOUR’];
let numArray2;
    (function() {
     numArray2 = [...numArray1]; // change this line
     numArray1[0] = ‘ZERO’
})();console.log(numARRAY2); // [‘ONE’, ‘TWO’, ‘THREE’, ‘FOUR’]
```

看看 console.log，试着找出不同之处，自己了解一下。

# 如何使用析构赋值给变量赋值？

析构是 ES6 中的一个新概念，它将为 JavaScript 开发人员节省大量时间。

## **从对象中析构赋值**

从对象中获取变量的老方法:

```
const cars = [ a: 'tesla', b: 'ford', c: 'lambo', d: 'lexus']const a = cars.a, // a = tesla
const b = cars.b, // b = ford
const c = cars.c, // c = lambo
const d = cars.d // d = lexus
```

获取变量的新方法:

```
const cars = [ a: 'tesla', b: 'ford', c: 'lambo', d: 'lexus']const { a: w, b: x, c: y, d: z } 
// w: tesla, x: ford, y: lambo, z: lexus
```

从嵌套对象中获取变量:

```
const Address = { 
Australia: { Street: 'Victoria Rd', Suburb: 'Alberta', State: 'NSW', Zip: 2222 }
}function getState(state) {
    const { Australia : { State: stateName }} = state;
    return stateName; 
}console.log(getState(Address)); //NSW
```

## 从数组中析构**赋值**

所以你想从一个数组中随机选择元素。这就是你如何选择。

```
const [a, b] = [1, 2, 3, 4, 5]
console.log(a,b) // return 1 2
```

如果你想选择第一个元素(1)和最后一个元素(5)，这就是析构将如何帮助你。

```
const [a, , , , b] = [1, 2, 3, 4, 5]
console.log(a,b) // return 1 5
```

## 用 Rest 运算符析构赋值来重新排列数组元素

从上面的例子，如果你想删除前两个元素。您可以使用 rest 操作符来做到这一点。让我们通过下面的例子来学习:

```
const numbers = [1, 2, 3, 4, 5]
function destroyStr(num) {
  const [, , ...args] = num 
  return args
}const newNumbers = destroyStr(numbers);
console.log(newNumbers); // [3, 4, 5]
console.log(numbers); // [1, 2, 3, 4, 5]
```

## 析构赋值以将对象作为函数参数传递

这是带有一堆值的 Tesla Model 3 对象。但是我们只需要价格和最大功率，所以我们创建了一个新函数`specINeed`并传递整个`teslaModel3`对象，然后返回我们需要的值。并用`console.log`打印出来。

```
const teslaModel3 = {
  price: '$68,000',
  seating: 5,
  range: '470 to 620km',
  maximumPower: '190 to 335 kW'
}const specINeed = (function(){
  return function specINeed(teslaModel3) {
   return `Price: ${teslaModel3.price} , Maximum Power: ${teslaModel3.maximumPower}`
  }
})();console.log(specINeed(teslaModel3));
// returns Price: $68,000 , Maximum Power: 190 to 335 kW
```

使用 ES6，我们可以做的是只传递我们需要的值，并像这样打印这些值:

```
const teslaModel3 = {
  price: '$68,000',
  seating: 5,
  range: '470 to 620km',
  maximumPower: '190 to 335 kW'
}const specINeed = (function(){
  return function specINeed({ price, maximumPower }) {
   return `Price: ${price} , Maximum Power: ${maximumPower}`
  }
})();console.log(specINeed(teslaModel3));
// returns Price: $68,000 , Maximum Power: 190 to 335 kW
```

这是一个很小的变化，但却非常重要。它通常与 API 调用一起使用。当我们用 Ajax 或 API 获取请求时，它会有比我们需要的更多的信息，所以它可以帮助我们只获取我们需要的数据。

# 进出口

如果在导入具有多个函数/类的导出文件时，以相同的方式导出和导出，则使用带花括号{}的导入。

```
import { function/class } from "filename" // an export 
export { function/class } // an export 
```

## 导入默认值，导出默认值

如果设置了导出默认值，则导入默认值时导入没有{}。

```
import function/class from "filename" // an export default
export default function/class  // an export default
```

点击[这里](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export#Using_the_default_export)了解更多进出口知识。

# 你从中学到了什么？

*   更好的 JavaScript 开发知识。
*   ES6 的基本概念
*   作为开发人员学习新事物的快乐
*   尝试新的 JS 框架，你会有家的感觉

# 喜欢你读的吗？

如果你想阅读更多像这篇文章这样的信息丰富的文章，我的电子邮件摘要正是你想要的。

[*今天点击加入我的邮件摘要。*](https://upscri.be/zm7qsy)