# 如何对 JavaScript 对象执行 CRUD 操作

> 原文：<https://javascript.plainenglish.io/how-to-perform-crud-operations-on-javascript-objects-5c832860ddf?source=collection_archive---------3----------------------->

## 在 3 分钟内你需要知道的关于 JavaScript 对象的事情。

![](img/e74f89d7d413b78d2073c3fd8efd907b.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 中的对象是由各种数据类型组成的相关数据或函数的集合，当包含在对象中时，这些数据类型也称为属性和方法。

对象是 JavaScript 生态系统中最重要的部分之一。JavaScript 对象的学习曲线可以很快变得很深，所以理解它的基础非常重要。

JavaScript 中有 7 种数据类型。其中 6 个是原始数据类型，因为它们只包含一种东西(字符串或数字或任何东西)。

# 创建对象

![](img/58857346850594cb84e563e57ed4ac7c.png)

在浏览器中打开控制台，并键入

```
const user = new Object(); // object constructor syntax
const user = {}; // object literal syntax
```

**恭喜**你创建了你的第一个对象。

对象用于同时存储多个值，并且是用带有一列`key`和`value`对的花括号{…}创建的。这种创建对象的方法被称为**对象文字**语法。

因为你在控制台中创建了`user`对象类型的用户。根据您的浏览器，您将看到以下内容。

```
[object Object]
Object { }
{ }
```

正如你所猜测的，它是一个空的物体。

示例:

```
let user = {           // an object
  name: "Herald",      // key is  "name" with value "Herald"
  age: 29,             // key is "age" with value 29 
  isAdult: true,       // key is "isAdult" with boolean of true
  interest: ['drawing', 'travelling'], // array of object
  passion: function() {
               console.log("My passion is " + this.interest[1])
            },          // function as an object
  "loves cat": true     // 2 words key with boolean value};
```

在这个例子中，`user`是`object`的名字，`keys`是`name, age, isAdult, interest, passion`和`values`是字符串、数字、数组和函数。这些值可以是任何类型。现在让我们看看如何从它们的键中检索数据值。

# 读取对象值

![](img/d9e705f64b1f6d9b32385b5eff5beeac.png)

尝试运行下面的代码，看看值是多少。

```
user.name         // Herald
user.age          // 29
user.isAdult      // true
user.interest[0]  // drawing
user.interest[1]  // travelling
user.passion()    // My passion is travelling
user["loves cat"] // true
```

`object.key`是如何操作任何对象的值，正如你可以看到的，我们第一个例子中 name 的值是使用`user.name`打印的

对象可以用**点符号或括号符号**进行评估。

“.”运算符或“[ ]”用于访问对象中的值。

# 更新值

很容易更新，只需传递如下新值:

```
user.name = "Harry"
user.age = 35
```

# 删除值

删除值是通过删除操作符完成的。它从对象中移除属性。

```
delete user.name
```

感谢您的阅读。