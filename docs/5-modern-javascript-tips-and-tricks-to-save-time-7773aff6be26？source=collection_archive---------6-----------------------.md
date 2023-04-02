# 节省时间的 5 个现代 JavaScript 技巧和窍门

> 原文：<https://javascript.plainenglish.io/5-modern-javascript-tips-and-tricks-to-save-time-7773aff6be26?source=collection_archive---------6----------------------->

## 使用这些 JavaScript 技巧减少工作量并编写干净的代码

![](img/e55198bc89f57e55c78547049cba0b5c.png)

Photo by [Jamie Street](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

说到编程，没有一个正确的做事方法。几乎所有的事情都可以通过多种方式来完成。

然而，有一些提示和技巧你应该记住，以加快这一进程。

下面是 5 个提示和技巧，希望对你有用。

## 1.合并对象和数组

要合并对象，标准的方法会导致很多麻烦。因此，我们将使用 ES6 更新中引入的 spread 运算符。

使用扩展操作符大大简化了合并过程。

```
const person = {
  firstName: "John",
  lastName: "Doe",

};const job = {
  role:'Developer',
  level:'Senior'

};let myPerson  = {
  ...person,
  ...job
};console.log(myPerson);
```

上面代码片段的输出:

```
{
  firstName: "John",
  lastName: "Doe",
  level: "Senior",
  role: "Developer"
}
```

但是如果在两个或更多的对象中存在相同的键呢？

在这种情况下，最后一个对象的键值被考虑并放入合并的对象中。

```
const person = {
  firstName: "John",
  lastName: "Doe",

};const person1 = {
 firstName: "Smith",
  lastName: "Doe",

};let myPerson  = {
  ...person,
  ...person1
};console.log(myPerson); // => {firstName: "Smith",lastName: "Doe"}
```

我们可以用同样的方法合并数组。

```
const person = ["John", "Felix"];
const job = ["Accountant"];const mergedArray = [...person, ...job];console.log(mergedArray); // => ["John", "Felix", "Accountant"]
```

如果您有兴趣了解更多关于不同数组方法的知识，可以查看下面给出的文章:

[](https://medium.com/javascript-in-plain-english/8-modern-array-methods-that-every-developer-should-know-416855e01757) [## 每个开发人员都应该知道的 8 种现代数组方法

### 非常有用的方法让你的生活更轻松

medium.com](https://medium.com/javascript-in-plain-english/8-modern-array-methods-that-every-developer-should-know-416855e01757) 

## 2.正确使用`console` 物体

使用 console 对象，您可以记录重要的调试消息。

然而，`log` 只是控制台对象提供的惊人方法之一。

使用`console.warn()`你可以抛出警告，而`console.clear()`方法允许你清除控制台日志。

您甚至可以使用 console 对象来测量代码运行的时间！

```
const person = ['John', 'Felix'];
const job = ['Accountant', 'Developer'];
const mergeThings = function () {
  const mergedArray = [...person, ...job];
};
mergeThings();
console.timeEnd('mytime'); // => mytime: 1ms
```

如您所见，用`console.time()`和`console.timeEnd()`方法包装您想要测量的时间的代码将使您能够记录时间。

值得一提的是，您需要在两个方法中传递相同的字符串作为参数。

您甚至可以使用 console 对象提供的方法对消息进行分组、创建表格以及设置日志样式。要找到同样的综合指南，你可以去[这里](https://developer.mozilla.org/en-US/docs/Web/API/console)。

## 3.解构

ES6 更新中引入的另一个特性是能够轻松地析构对象。

使用析构可以快速访问和使用对象中的值。

```
const person = {
  name: 'Kevin',
  job: {
    role: 'Developer',
    position: { level: 'Senior', experience: '10 yrs' },
  },
};
const {experience}=person.job.position
console.log(experience) // => "10 yrs"
```

从上面的代码片段中，您可以看到，我们可以通过用括号将键括起来，从特定的键-值对中提取值。

我们甚至可以使用相同的技术提取多个值，如下所示:

```
const user = {
  firstName: "Kevin",
  lastName: "Goodman",
};const { firstName, lastName } = user;console.log(firstName, lastName); // => "Kevin Goodman"
```

这还不是全部。

我们甚至可以在函数中使用相同的方法，使它们只接受相关的值作为参数。

```
function getHobby({ hobby }) {
  return hobby;
}const user = {
  firstName: "Kevin",
  lastName: "Smith",
  hobby: "Football",
};console.log(getHobby(user)); // => "Football"
```

在上面的代码中，我们可以看到析构的用法是只获取‘hobby’键值作为参数。

这导致了更干净和健壮的代码。

## 4.三元运算符

尽管 JavaScript 提供了许多方法(如 if-else 和 switch-case)来检查是否满足特定条件，但是使用三元运算符可以用更短的方式编写这些检查。

例如，我们想检查这一天是否是星期一，并相应地显示消息。

使用 if-else，我们的代码将如下所示:

```
let day = 'Monday';
if (day == 'Monday') {
  console.log('It is Monday');
} else console.log('It is not Monday'); 
```

然而，我们可以使用三元运算符来缩短代码并增强可读性。

```
let day = 'Monday';day=='Monday' ? console.log('It is Monday'):console.log('It is not Monday')
```

使用三元运算符`?`和`:`，我们可以替换 if-else 代码块。

尽管三元运算符对于小型比较非常有效，但对于长时间的复杂比较并不理想，因为它们会降低可读性。

## 5.快速转换

很多时候，我们希望将一个字符串转换成一个数字，反之亦然，然后执行一些操作。

下面是几种快速转换变量数据类型的方法。

**转换成字符串**

字符串可能是最容易转换的。

```
const num= 1 + "";

console.log(num); // => "1"
console.log(typeof num); // => "string"
```

**转换为整数**

为了转换成整数，我们使用`+`操作符告诉 JavaScript 将字符串转换成数字。

```
let num= "1";
num= +num;

console.log(num); // => 1
console.log(typeof num);// => "number"
```

**转换为布尔型**

JavaScript 为所有变量赋值 true 或 false。

`0`、`""`、`null`、`undefined`、`NaN`当然还有`false`被 JavaScript 认为是假的，而其他所有值都是真的。

考虑到这一点，我们可以简单地将变量转换为布尔值。

```
const mTrue  = !0; //true
const mFalse = !1; //false
const qFalse = !!0; // false
```

通过使用`!`操作符，我们可以在变量的真值和假值之间切换。

## `**Conclusion**`

随着 JavaScript 被用来创建越来越复杂的应用程序，掌握一些技巧和窍门来简化开发过程是明智的。

此外，人们必须了解并遵循 JavaScript 的一些现代实践。

我有一个关于 JavaScript 现代实践的博客，无论你是新手还是专业人士，都值得一看。

使用 JavaScript，您可以灵活地用多种方式完成相同的任务。

然而，使用这些实践和技巧可以大大减少你的工作量，并帮助你破解面试和建立更好的应用程序。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**