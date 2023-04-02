# 15 种简单的编码技术，用更短的 JavaScript 代码完成任务

> 原文：<https://javascript.plainenglish.io/15-simple-coding-techniques-to-get-your-tasks-done-with-shorter-code-in-javascript-59d46801db0?source=collection_archive---------1----------------------->

## 不要浪费时间写长代码，而你可以把它写得更短，更清晰，更易读。

![](img/048f958a6f922618c7b7e71c18f5c8c1.png)

Photo by [Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

每个人一天有二十四小时。

我们不能创造比这个数字更多的时间，但我们可以有效地利用每一秒钟。

做到这一点的一个方法是用下面的 15 种技巧来缩短你的代码。

# 1.声明变量速记

如果没有意义，不要在每一行声明每个变量。一行就够了，节省你的空间。

而不是:

```
let name;
let price = 12;
let title;
let discount = 0.3;
```

执行以下操作:

```
let name, price = 12, title, discount = 0.3;
```

# 2.条件速记

我认为这是显而易见的，但只要它服务于主题，我就把它放进去。

而不是:

```
if (isUsernameValid === true) {}if (isExist === false) {}
```

执行以下操作:

```
if (isUsernameValid) {}if (!isExist) {}
```

# 3.使用十进制底指数

1000000 如果少了一两个零，会产生一些 bug。尾部的零越多，就越要注意。幸运的是，你不必那样做。不用写 1000000，直接缩写成 1e6 就可以了。 **6** 代表尾部零的个数。

而不是:

```
let length = 10000;
for (let i = 0; i < length; i++) {}
```

执行以下操作:

```
let length = 1e4;
for (let i = 0; i < length; i++) {}
```

# 4.默认参数

有时你必须定义一个有多个参数的函数。每次调用函数时，都需要传递所有的参数值吗？如果你初始化默认值，就不会了。

而不是:

```
let generateBookObject = (name, price, discount, genre) => {
  let book = { name, price, discount, genre };
  return book;
};let book = generateBookObject(‘JavaScript’, 12, 0.3, ‘Technology’); // { name: ‘JavaScript’, price: 12, discount: 0.3, genre: ‘Technology’ }
```

执行以下操作:

```
let generateBookObject = (name = ‘JavaScript’, price = 12, discount = 0.5, genre = ‘Technology’) => {
  let book = { name, price, discount, genre };
  return book;
};// In case discount and genre are the same as the default values, you don’t need to pass them to the function
let book = generateBookObject(‘JavaScript’, 12); // { name: ‘JavaScript’, price: 12, discount: 0.5, genre: ‘Technology’ }
```

# 5.三元运算符

如果条件语句中只有一个，使用**三元组**将其缩减为一行。

而不是:

```
let name = ‘Amy’;
let message;if (name === ‘Amy’) {
  message = ‘Welcome back Amy’;
} else {
  message = ‘Who are you?’;
}
```

执行以下操作:

```
let name = ‘Amy’;
let message = name === ‘Amy’ ? ‘Welcome back Amy’ : ‘Who are you?’;
```

# 6.迭代列表的短选项

并不是每种情况下你都可以用一个循环方法来代替一个简短的方法，但是如果确实如此，那就去做吧。

而不是:

```
let names = [‘Amy’, ‘James, ‘David’, ‘John’];for (let i = 0; i < names.length; i++) {}
```

执行以下操作:

```
let names = [‘Amy’, ‘James, ‘David’, ‘John’];for (let name of names) {}
```

如果指数很重要:

```
for (let [index, name] of names.entries()) {}
```

# 7.最小评估

在使用变量进行任何计算之前，你必须确保变量是正确的(正确的意思是它不是空的或者未定义的)。

对于这种情况， **if … else** 是一个不错的选择，但是使用**最小求值**应该更好。

而不是:

```
let book = {
  name: ‘Learn JavaScript’,
  price: 15
};let finalPrice;if (book.discount !== undefined || book.discount !== null) {
  finalPrice = book.price — book.price * book.discount;
}
```

执行以下操作:

```
let book = {
  name: ‘Learn JavaScript’,
  price: 15
};let finalPrice = book.price — book.price * (book.discount || 0);
```

# 8.声明对象速记

这是在您想要使用现有变量定义对象属性的情况下。

```
let name = ‘Amy’;
let age = 28;
let person = { name, age }; // { name: ‘Amy’, age: 28 }
```

请注意，属性的名称与现有变量的名称相同。

# 9.使用箭头功能

这是显著减少几行代码的好方法。

而不是:

```
function add(a, b) {
  return a + b;
}collection.map(function(item) {
  console.log(item);
});
```

执行以下操作:

```
let sum = (a, b) => a + b;collection.map(item => console.log(item));
```

# 10.字符串到数字转换速记

而不是:

```
let string = ‘2020’;
let number = parseInt(string);
```

执行以下操作:

```
let string = ‘2020’;
let number = +string;
```

# 11.使用析构

当你想提取对象属性值时，你不需要一个一个地给变量赋值。使用析构。

而不是:

```
let company = {
  name: ‘Apple’,
  industry: ‘Technology’,
  CEO: ‘Tim Cook’,
  stockPrice: 364,
  products: [‘iPhone’, ‘Macbook’, ‘Apple Watch’]
};let name = company.name;
let stockPrice = company.stockPrice;
let products = company.products;
```

执行以下操作:

```
let { name, stockPrice, products } = company;// In case you want to use different variable name. For example, **companyName** instead of **name** let { name:companyName, stockPrice, products } = company;
```

# 12.模板字符串

而不是:

```
let name = ‘Amy’;
let time = ‘yesterday’;
let welcomeMessage = ‘Welcome back ‘ + name + ‘. Your last time is ‘ + time;
```

执行以下操作:

```
let name = ‘Amy’;
let time = ‘yesterday’;
let welcomeMessage = `Welcome back ${name}. Your last time is ${time}`;
```

注意: **welcomeMessage** 使用的是**反斜杠**(`)，不是**撇号**(')也不是**引号**(")。

# 13.Math.pow()速记

而不是:

```
let result = Math.pow(3, 5); // 243
```

执行以下操作:

```
let result = 3**5; // 243
```

# 14.对多行字符串使用反斜线

而不是:

```
let greeting = ‘Hi, I am Amy\n’
             + ‘I am a programmer.\n’
             + ‘Nice to meet you.’;
```

执行以下操作:

```
let greeting = `Hi, I am Amy
 I am a programmer.
 Nice to meet you.`
```

# 15.使用跨页添加内容

代替:

```
let arr1 = [3, 5, 1];
let arr2 = [2, 4, 8];
let result = arr1.concat(arr2);
```

执行以下操作:

```
let arr1 = [3, 5, 1];
let result = [...arr1, 2, 4, 8]// Or even at any index you want
let result = [2, ...arr1, 4, 8]// You can also use spread with objects
let person = {
  name: ‘Tony’,
  age: 43
};let superHero = {
  heroName: ‘Ironman’,
  ...person
};
```

更短、更干净、更易读，是以上技术应该带给你的感觉。

当然，还有很多我不知道的速记技巧。如果你能建议一些，我将不胜感激。

[](https://medium.com/javascript-in-plain-english/who-else-wants-to-write-clean-javascript-code-ff4f7896e6bd) [## 还有谁想写干净的 JavaScript 代码？

### 让你的同事爱上你的代码的 7 个技巧。

medium.com](https://medium.com/javascript-in-plain-english/who-else-wants-to-write-clean-javascript-code-ff4f7896e6bd) 

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到他们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**