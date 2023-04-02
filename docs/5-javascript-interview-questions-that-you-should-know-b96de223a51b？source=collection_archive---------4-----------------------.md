# 你应该知道的 5 个 JavaScript 面试问题

> 原文：<https://javascript.plainenglish.io/5-javascript-interview-questions-that-you-should-know-b96de223a51b?source=collection_archive---------4----------------------->

## 5 个 JavaScript 面试问题及答案

![](img/e272a15d06692e648970a79c6f28ea80.png)

Photo by [Headway](https://unsplash.com/@headwayio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

面试是招聘过程中很重要的一部分。对于雇主来说，这是筛选出不适合某个职位的候选人的最可靠的方法之一。作为一名 JavaScript 开发人员，你必须在面试前做好准备，因为这将增加你被公司、创业公司或其他公司聘用的机会。这会让你有信心在技术面试中表现出色。

在这篇文章中，我决定给你一些你应该知道的 JavaScript 面试问题。让我们开始吧。

![](img/6b081ba8b6e0c84c2599df61b1a0dbce.png)

Image Created With ❤️ ️By author [Mehdi Aoussiad](https://mehdiouss315.medium.com/).

# 1.什么是吊装？

提升是一个术语，用来描述将*变量*和*函数*移动到它们的 *(* 全局或函数 *)* 作用域的顶部，在那里我们定义变量或函数。这是 JavaScript 将声明移动到顶部的默认行为。

通过提升，变量可以在没有任何错误的情况下被声明之前使用。看看下面的例子:

```
x = 5; // Assign 5 to x

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x;                     // Display x in the element

var x; // Declare x
```

注意，只有用 *var* 关键字声明的函数声明和变量被*提升，*不是函数表达式或箭头函数，`**let**`和`**const**`关键字。有兴趣可以看看我关于吊装的文章。

[](https://medium.com/dev-genius/4-things-you-should-know-about-javascript-hoisting-8b53803a1ed0) [## 关于 JavaScript 提升你应该知道的 4 件事

### 用实例理解 JavaScript 提升

medium.com](https://medium.com/dev-genius/4-things-you-should-know-about-javascript-hoisting-8b53803a1ed0) 

# 2.什么是闭包？

一个闭包是一个函数的组合，该函数被捆绑在一起(被封闭)，并引用其周围的状态(词汇环境)。换句话说，闭包允许您从内部函数访问外部函数的范围。在 JavaScript 中，闭包是在每次创建函数时创建的。

看看下面的例子:

```
//Outer function.
function makeFunc() {
  var name = 'Mozilla'; //name is a local variable created by init.//Inner function
function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();
```

闭包是一个可以访问父作用域的函数，即使父函数已经关闭。

这是另一个例子:

```
var add = (function () {
  var counter = 0;
  return function () {counter += 1; return counter}
})();

add();
add();
add();

// the counter is now 3
```

自调用函数只运行一次。它将计数器设置为零(0)并返回一个函数表达式。这样 add 就变成了一个函数。“精彩”的地方在于它可以访问父作用域中的计数器。

这被称为 JavaScript **闭包。**它使得函数有可能拥有“**私有**变量。计数器受匿名函数的作用域保护，只能使用 add 函数进行更改。

# 3.JavaScript 中的`this"`有什么价值？

JavaScript `**this**`关键字指的是它所属的对象。根据使用场合的不同，它有不同的值。比如，关键字`**this**`单独指全局对象，但和方法一起使用时，指的是所有者对象。看看下面的例子:

关键词`**this**`独自一人:

```
var x = this;
```

在这种情况下，它指的是全局对象(对象窗口)。

事件处理程序中的关键字`**this**`:

```
<button onclick="this.style.display='none'">
  Click to Remove Me!
</button>
```

在 HTML 事件处理程序中，`this`指的是接收事件的 HTML 元素。但是在严格模式下，`this`就是`**undefined**`。

# 4.什么是高阶函数？

任何以另一个函数作为自变量的函数都称为**高阶函数。** JavaScript 提供了一些有用的高阶函数来以一种简单的方式操作数据。

以下是一些你应该知道的重要功能:

**map** 方法帮助传递一个函数来转换数组中的每一项。

```
const numbers = [1, 2, 3];
const doubles = numbers.map(num => num * 2) //[2, 4, 6]
```

**过滤器**接收一个数据集合，您可以传递一个返回集合子集的条件函数。

```
const numbers = [1, 2, 3];
const isGreaterThanOne = numbers.filter(num => num > 1) //[2, 3]
```

**reduce** 方法采用一个带有两个参数(累加器和项目)的函数。我们还可以使用 reduce 方法返回所有数组项的总数，如下例所示。

```
const numbers = [1, 2, 3];
const mySum = numbers.reduce((accumulator, num) => accumulator + num) // returns: 6.
```

# 5.什么是类？

类是创建对象的模板。它们建立在原型的基础上，并且它们提供了一个简单的语法糖来为 JavaScript 中的面向对象编程编写构造函数。

要创建一个 JavaScript 类，你必须使用关键字`**class**` 并使用其中的构造函数方法。看看下面的例子:

```
class Car {
  constructor(name, year) {
    this.name = name;
    this.year = year;
  }
}
```

现在，您可以使用该类创建一个名为“myCar”的新对象，示例如下:

```
let myCar = new Car("Ferrari", 2020);
```

也可以从其他类继承方法和属性。要创建一个类继承，使用`**extends**`关键字。看看下面的例子，我们将创建一个名为“Model”的类，它将继承“Car”类的方法。

```
class Car {
  constructor(brand) {
    this.carname = brand;
  }
  present() {
    return 'I have a ' + this.carname;
  }
}

class Model extends Car {
  constructor(brand, mod) {
    super(brand);
    this.model = mod;
  }
  show() {
    return this.present() + ', it is a ' + this.model;
  }
}

let myCar = new Model("Ford", "Mustang");
```

`**super()**` 指父类的方法。通过调用它，我们可以访问 car 类(父类)的所有属性和方法。

# 结论

了解并练习 JavaScript 中的常见面试问题会让你在参加编码面试时充满自信。熟能生巧。

感谢您阅读本文，希望您觉得有用。如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**

## 更多阅读

[](https://medium.com/javascript-in-plain-english/5-javascript-functional-programming-concepts-that-you-should-know-c96f14b02a87) [## 你应该知道的 5 个 JavaScript 函数式编程概念

### 带有实际例子的函数式编程概念

medium.com](https://medium.com/javascript-in-plain-english/5-javascript-functional-programming-concepts-that-you-should-know-c96f14b02a87)