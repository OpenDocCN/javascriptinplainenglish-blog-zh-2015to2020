# 24 个快速 JavaScript 面试问题

> 原文：<https://javascript.plainenglish.io/24-quick-fire-javascript-interview-questions-a71f78d03f08?source=collection_archive---------0----------------------->

## JavaScript 快速面试问题

![](img/7303ec261104cec677237d608b37a3af.png)

Photo by [Drew Hays](https://unsplash.com/@drew_hays?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **解释“==”和“===”的区别**

“==”用于比较两个值，而不考虑变量的数据类型。

" === "用于比较两个值，但将进行严格检查，因此将检查值和数据类型是否匹配。

*示例:*

```
"50" == 50 // true
"50" === 50 // false
50 === 50 // true
```

## 如何检查一个值是否是一个数字？

`isNaN()`函数确定该值是否不是数字。

如果你使用`isNaN()`，你将不得不做**额外的检查**，正如你从下面的例子中看到的。

*示例:*

```
isNaN(48) //false
isNaN(-1.23) //false
isNaN(5-2) //false
isNaN('123') //false
isNaN('Hello Im a real string') //true
isNaN('2005/12/12') //true
isNaN('') //false
isNaN(undefined) //true
```

## 如何将一个字符串转换成一个整数？

`parseInt()`将字符串转换为整数

*举例；*

```
parseInt("30", 10) // 30
parseInt("55px", 10) // 50
parseInt(2.55, 10) // 2
```

`parseFloat()`将字符串转换成点数(带小数)

*例如:*

```
parseFloat("30") // 30
parseFloat("55px") // 50
parseFloat(2.55) // 2.55
```

你也可以接受

`Number()`将字符串转换为数字。这可以是整数，也可以是点数。这些通常比使用`parseInt`或`parseFloat`更不安全

## **说出 JavaScript 中不同的循环**

`for` -多次循环通过一个代码块

`for/in` -遍历对象的属性

`for/of` -循环遍历可迭代对象的值

`while` -当指定的条件为真时，循环通过代码块

`do/while` -当指定的条件为真时，也在代码块中循环

## **`**var**`**`**let**`**`**const**`**有什么区别？********

****`var`可以重新申报和更新。****

****`let`可更新但不可申报****

****`const`不能更新或重新申报。****

*****例如:*****

```
**var myNumber = 10;
var myNumber = 20;// No errorlet myNumber = 10;
let myNumber = 20;// In the console I get an error:
Uncaught SyntaxError: Identifier 'myNumber' has already been declared**
```

## ******`**NULL**`**和** `**undefined**` **有什么区别？********

****`NULL`是一个赋值。它可以作为无值的表示赋给变量。****

****`undefined`表示一个变量已经被声明，但还没有赋值****

*****举例:*****

```
**let hi; // undefinedlet hi = NULL; //NULL**
```

## ******`**typeof**`**运算符是做什么用的？********

****`typeof`运算符返回一个字符串，指示未赋值操作数的类型。****

*****例如:*****

```
**typeof(122) // "number"
typeof(122.55) // "number"
typeof("I'm a string") // "string"
typeof({ name: "James Smith"}) // "object"
typeof(false) // "boolean"**
```

## ****如何检查一个对象是否是一个数组？****

****`isArray()`函数判断一个对象是否为数组。****

## ******什么是回调？******

****回调函数也称为高阶函数，是作为参数传递给另一个函数的函数。****

## ******解释什么是默认功能参数******

****如果没有传递值或`undefined`，它们允许用默认值初始化命名参数。****

*****例如:*****

```
**function addTogether(x, y = 1) {
     return x + y;
}addTogether(10, 10) // 20
addTogether(10) // 11**
```

## ******什么是 ES6 模块？******

****他们组织了一套相关的 JavaScript 代码。一个模块可以包含变量和函数。模块只不过是写在文件中的一段 JavaScript 代码。****

## ******命名两种不同的 ES6 出口******

******默认**当一个模块只需要导出单个值时，使用导出。****

******命名**出口以其名称来区分。一个模块中可以有多个命名的导出。****

## ******什么是承诺？******

****它们用于处理异步操作。它们可以轻松处理多个异步操作，并提供比回调和事件更好的错误处理。****

## ******承诺有哪些不同的状态？******

****承诺有三种状态:****

1.  ******完成**:承诺相关动作成功****
2.  ******拒绝**:承诺相关动作失败****
3.  ******待定**:承诺仍处于待定状态，即尚未履行或拒绝****

## ******什么是 async/await？******

****`async`在函数之前意味着一件简单的事情:函数总是返回一个承诺。其他值自动包装在已解析的承诺中。****

****让 JavaScript 等待，直到承诺完成并返回结果。****

## ******本地存储&会话存储有什么区别？******

****`local`一直存在，直到被明确删除。所做的更改将被保存，并可用于当前和将来对该网站的所有访问。****

****`session`变更仅适用于每个选项卡。所做的更改将被保存并可用于该选项卡中的当前页面，直到它被关闭。一旦关闭，存储的数据将被删除。****

## ******命名不同的 DOM 选择器******

*   ****getElementsByTagName()****
*   ****getElementsByClassName()****
*   ****getElementById()****
*   ****查询选择器()****
*   ****querySelectorAll()****

## ******什么是闭包？******

****当一个定义在引用的作用域之外的变量从某个内部作用域被访问时，我们需要闭包。****

## ****如何将一个项目添加到一个数组中？****

****您可以使用`push()`函数将一个项目添加到一个数组中。它还可以同时添加许多项目。****

*****举例:*****

```
**let items = ["dan", "john"];items.push("liam");  // ["dan", "john", "liam"]
items.push("lee", "james"); // ["dan", "john", "liam", "lee", "james"]**
```

## ******如何从数组中移除一个特定的项目？******

****`splice`函数使用索引从数组中删除一个项。****

****`filter`使用回调函数。****

## ******为什么 JavaScript 被称为松散类型或动态语言？******

****JavaScript 被称为松散类型或动态语言，因为 JavaScript 变量不直接与任何值类型相关联，任何变量都可以被赋值和重新赋值。****

*****示例:*****

```
**let someLet = 10; // someLet is a number
someLet = "Hello world"; // someLet is now a string
someLet = true; // someLet is now a bool**
```

## ******什么是“这个”？******

****`this`关键字指的是当前正在编写代码的对象。****

## ******如何用 JavaScript 进行 API 调用？******

****寻找下面的一个:****

1.  ****XMLHttpRequest****
2.  ****取得****
3.  ****Axios****
4.  ****jQuery****

## ******说出你将在 JavaScript 中使用的 API 动词类型******

****寻找下面的:****

1.  ****邮政****
2.  ****得到****
3.  ****修补****
4.  ****删除****
5.  ****放****

# ****结论****

****在面试中，我被问及上述问题的混合体。我认为对于任何 JavaScript 面试来说，混合提问是有好处的。将编码挑战与一些快速提问结合起来是一种很好的方式，可以看出候选人的潜力。****

****希望你喜欢！****

****[](https://medium.com/javascript-in-plain-english/top-19-frequently-asked-typescript-interview-questions-dac4ff30c017) [## 19 个常见的打字稿面试问题

### 关于 TypeScript 的常见面试问题

medium.com](https://medium.com/javascript-in-plain-english/top-19-frequently-asked-typescript-interview-questions-dac4ff30c017) 

## **说白了就是**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！******