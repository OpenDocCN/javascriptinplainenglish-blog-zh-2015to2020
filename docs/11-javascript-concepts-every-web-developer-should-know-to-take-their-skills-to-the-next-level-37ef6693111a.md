# 每个 Web 开发人员都应该知道的 11 个 JavaScript 概念，让他们的技能更上一层楼

> 原文：<https://javascript.plainenglish.io/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a?source=collection_archive---------0----------------------->

## 不了解这些概念，就无法掌握 JavaScript。

![](img/65245eb8aecd7188f87adeafbc0a8823.png)

Photo by [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

每个 JavaScript 开发人员都应该理解这种复杂语言的基本概念。

下面的概念并不是全部，但是这些是你向前迈出一步需要知道的基本概念。

让我们开始吧。

# 1.生活

它代表立即调用的函数表达式。它是在创建后立即被调用的函数。

你如何定义生活？请看下面的例子:

```
(() => console.log(‘Hello world’))();
```

随着代码的执行，控制台将立即记录 **Hello world** 。

使用 IIFE 的原因是为了保护变量的可访问性。生命中定义的变量不能从外部访问。这是编写可维护代码并防止您的源代码变得一团糟的方法。

# 2.MVC 结构

不仅在 JavaScript 中，这种结构在几乎所有编程语言中都有使用。与 MVC 的名字相去甚远，将代码组织成不同的层(如数据、视图和逻辑)并分别对待是一个流行的概念。

作为一个大项目，你需要一个结构来扩大规模，而不用碰壁。从长远来看，MVC 是最好的选择之一。在将来的某个时候，当添加新功能或调查错误时，您会感谢自己过去花时间实现 MVC。

# 3.关闭

我们在讨论内部函数时使用这个概念，这个内部函数总是可以访问它的外部函数的变量和参数，甚至在外部函数返回之后。

闭包允许你访问函数内部的数据，而不需要直接修改它们。这样，您可以保护您的代码，同时给予其他人扩展它的能力。尤其是当你公开一个图书馆的时候。

```
const sayHelloTo = name => {
  let message = ‘Hello ‘ + name;
  return () => console.log(message);
}const sayHelloToAmy = sayHelloTo(‘Amy’);
sayHelloToAmy(); // Hello Amy
```

# 4.异步/等待

Async/await 允许您使用异步处理。在处理调用 API 时，您通常会陷入异步任务。在视图上显示之前，需要完全提取数据。

让我对使用 async/await 感到满意的是，我可以摆脱回调地狱。如果你和我一样，你不喜欢嵌套代码。它使你的代码变得难看，更难维护。

有关 async/await 的用法，请参见下面的示例:

```
const displayData = async () => {
  const data = await fetch(‘https://api.github.com/repositories');
  const jsonData = await data.json();
  console.log(jsonData);
};displayData();
```

# 5.范围

JavaScript 中有两种类型的作用域。局部范围和全局范围。你可以想象一个可变作用域是一头被绳子拴在柱子上的牛。它只能在有限的区域内移动，这取决于绳子的长度。

用这个比喻，局部变量就像一头被短绳子拴着的牛，全局变量就像一头没有绳子的牛。

例如:

```
// Global scope
const globalCow = ‘global cow’;const showCow = () => {
  const localCow = ‘local cow’;
  return globalCow;
};const clonedCow = globalCow;const mixedCow = globalCow + localCow; // error: Uncaught ReferenceError: localCow is not defined
```

如你所见，变量 **globalCow** 可以在任何地方使用，甚至是在函数 **showCow** 的本地上下文中。但是你不能在函数 **showCow** 之外使用变量 **localCow** ，因为它是局部定义的。

# 6.值类型与引用类型

当你给变量赋值时，不仅仅是赋值这么简单。您需要了解它是实际值还是引用，否则，您可能会无意中更改这些值。

当您分配诸如字符串、数字或布尔之类的基本类型时，事情就简单了。它们是实际值。

如果给对象、数组或函数赋值的话，会稍微复杂一点。这一次，变量不会保存实际值，而是保存对内存中实际值的引用。

示例:

```
let num1 = 1;
let num2 = num1;// Changing num2’s value does not change num1’s value
num2 = 4;
console.log(num1); // 1
console.log(num2); // 4 let arr1 = [‘Amy’, ‘John’];
let arr2 = arr1;// Changing elements’ value in arr2 leads to changing elements’ value in arr1
arr2[0] = ‘Jane’;
console.log(arr1); // [“Jane”, “John”]
console.log(arr2); // [“Jane”, “John”]
```

# 7.回收

在 JavaScript 中，回调函数是在调用另一个函数后执行的函数。您可以将回调函数作为参数传递给其他函数。

那么我们为什么要用回调呢？通常，我们编写的代码是从上到下顺序运行的。然而，在某些情况下，有些任务需要在执行其他任务之前完成。这时回调就派上了用场。

```
const fetchUsers = callback => {
  setTimeout(() => {
    let response = ‘[{name: “Amy”}, {name: “John”}]’;
    callback(response);
  }, 500);
};const showUsers = users => console.log(users);
fetchUsers(showUsers);
```

在上面的例子中，我们调用了 **fetchUsers** 函数，并将 **showUsers** 回调函数作为参数传递。当所有数据全部加载后，**显示用户**将在屏幕上显示。

# 8.原型

每当我们用 JavaScript 创建一个函数或对象时，都会在其中添加一个原型属性。默认情况下，原型是与函数和对象相关联的对象，我们可以在其中附加其他属性，这些属性可以由其他对象继承。

示例:

```
function Person() {
  this.name = ‘Amy’;
  this.age = 28;
}Person.prototype.job = ‘Programmer’;
Person.prototype.showName = function() {
  console.log(‘My name is ‘ + this.name);
}let person = new Person();
person.showName(); // My name is Amy
```

# 9.班级

在 ES6 之前，JavaScript 中没有类。你只能用函数的方式来处理类的概念。

```
function Book(title) {
  this.title = title;
}Book.prototype.showTitle = function() {
  console.log(this.title);
};let book = new Book(‘JavaScript’);
book.showTitle(); // JavaScript
```

在 ES6 中，您可以像创建任何基于类的语言背景一样创建一个实际的类:

```
class Book {
  constructor(title) {
    this.title = title;
  } showBook() {
    console.log(this.title);
  }
}let book = new Book(‘ES6’);
book.showBook();
```

这很方便，因为它将创建类的几种方法统一到一个单独的方法中。

# 10.破坏

这是一种从对象中提取属性的干净方式。

基本用法:

```
const person = {
  name: ‘Amy’,
  age: 28
};let { name, age } = person;
console.log(name); // Amy
console.log(age); // 28
```

您可以保持变量与上述属性名称相同，或者定义新的变量:

```
let { name: newName, age: newAge } = person;console.log(newName); // Amy
console.log(newAge); // 28
```

# 11.传播算子

这个可以让你访问一个可迭代对象的内部。简单地说，这是一种快速而简洁的方法，可以向数组中添加项，合并对象，或者从数组中取出单个项，然后将它们传递给函数。

```
// Combining Arrays
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr3 = […arr1, …arr2];
console.log(arr3); // [1, 2, 3, 4, 5, 6]// Combining Objects
let obj1 = {
  name: ‘Amy’,
  age: 28
};let obj2 = {
  job: ‘programmer’
};let obj3 = { …obj1, …obj2 };console.log(obj3); // {name: “Amy”, age: 28, job: “programmer”}// Spreading out an array and pass it to a function
const sum = (…arr) => {
  const length = arr.length;
  let sum = 0; for (let i = 0; i < length; i++) {
    sum += arr[i];
  } return sum;
};let arr = [3, 5, 3, 2, 1];
console.log(sum(…arr)); // 14
console.log(sum(3, 5, 4, 1)); // 13
```

# 结论

你完全理解上面所有的概念吗？如果没有，现在是时候检查所有这些，并准备将你的技能提升到一个新的水平。

我错过什么了吗？请留下评论让我知道。

## 进一步阅读

[](https://medium.com/javascript-in-plain-english/13-super-useful-chrome-devtools-tips-to-speed-up-your-developing-workflow-e9ecb60fda5c) [## 13 个超级有用的 Chrome DevTools 技巧来加速你的开发工作流程

### 一套很棒的 web 开发工具。

medium.com](https://medium.com/javascript-in-plain-english/13-super-useful-chrome-devtools-tips-to-speed-up-your-developing-workflow-e9ecb60fda5c) 

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**