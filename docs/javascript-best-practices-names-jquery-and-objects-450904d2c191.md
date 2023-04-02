# JavaScript 最佳实践—名称、jQuery 和对象

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-names-jquery-and-objects-450904d2c191?source=collection_archive---------5----------------------->

![](img/baa48dad36e46db6a0449762d8abec7e.png)

Photo by [Daniel Jerez](https://unsplash.com/@danieljerez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将研究命名事物以及处理对象、名称和 jQuery 代码的最佳方式。

# 使用 PascalCase 导出构造函数、类、单例或库

我们坚持使用 PascalCase，因为我们没有导出构造函数、类、单例对象和库。

例如，我们写道:

```
const StudentUser = {
  //...
};

export default StudentUser;
```

这也适用于构造函数和构造函数:

```
class StudentUser {
  //...
}function StudentUser {
  //...
}
```

# 首字母缩写词应该全部大写或小写

首字母缩略词都用同一个字母会使它们更易读。

例如，我们可以写:

```
const HTTPResponses = [
  // ...
];
```

或者:

```
const httpResponses = [
  // ...
];
```

# 什么时候我们应该让常量名大写？

当常数不变时，我们可以用常数命名特例。

让它们大写意味着我们可以相信不会改变。

如果常量名称被导出，我们也应该让它们大写。

然而，这应该只适用于最高级别。

例如，我们可以写:

```
export const API_KEY = 'foo';
```

或者:

```
export const FOO = {
  key: 'value'
};
```

# 访问者

我们不需要属性的访问函数。

此外，我们不应该使用 JavaScript getters 或 setters，因为它们有意想不到的副作用。

它们也使我们的代码更难理解。

相反，我们创建函数来获取和设置我们的值。

例如，不写:

```
class Person {
  get age() {
    // ...
  } set age(value) {
    // ...
  }
}
```

我们写道:

```
class Person {
  getAge() {
    // ...
  } setAge(value) {
    // ...
  }
}
```

正常的方法更容易理解。

# 如果一个属性或方法返回一个布尔值，那么我们给它加上前缀`is`或`has`

例如，我们写道:

```
person.hasAge()
```

而不是:

```
person.age()
```

现在我们知道这个函数是做什么的了。

# 创建获取和设置函数

我们可以创建`get`和`set`函数来获取和设置值。

例如，我们可以写:

```
class Person {
  constructor() {
    //...
  } set(key, val) {
    this[key] = val;
  } get(key) {
    return this[key];
  }
}
```

这样，我们可以通过它们的键和值动态地获取和设置值。

# 触发事件

如果我们触发事件，我们应该传入一个对象而不是一个原始值。

例如，我们写道:

```
const event = new CustomEvent("eventName", {
  "detail": "event"
});
document.dispatchEvent(event);document.addEventListener("eventName", (e) => {
  console.log(e.detail);
});
```

现在我们有一个对象作为`detail`的值而不是一个原始值。

如果我们总是用一个事件发送一个对象，那么我们就不必检查发送的对象是一个原始值还是一个对象。

# jQuery

如果我们使用`jQuery`，我们应该添加一个`$`前缀，这样我们就知道它是由 jQuery 返回的。

例如，我们可以写:

```
const $main = $('.main');
```

![](img/e7b285f68026b17ca18c86937b8b3c24.png)

Photo by [Jakub Kriz](https://unsplash.com/@jakubkriz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 缓存 jQuery 查找

如果我们用 jQuery 查找一些东西，我们应该缓存它们，这样我们就不必重复查找相同的东西。

例如，我们可以写:

```
const main = $('.main');
main.hide();
// ...main.css({
  'background-color': 'green',
});
```

# 对作用域 jQuery 对象查询使用`find`

我们不应该使用`find`进行限定范围的查询。

相反，我们只对一个查询使用一个 CSS 选择器:

```
$('.main ul').hide();
```

这样，我们只需要查找一次，而不是两次。

# 箭头功能

对于回调，我们应该尽可能多地使用箭头函数。

例如，我们写道:

```
[1, 2, 3].map((x) => x + 1);
```

比用传统函数要短很多。

`this`的值在里面也不变。

# 跳过括号

如果函数只有一条语句，我们可以跳过括号，使用隐式返回。

上面的例子告诉我们，我们可以跳过括号，返回一些东西。

我们写道:

```
(x) => x + 1
```

它返回`x`加 1。

# 结论

为了在使用 jQuery 时加快速度，我们应该缓存查询。

此外，我们不应该使用`find`进行限定范围的查询。相反，使用一个 CSS 选择器。

我们应该坚持标准的 JavaScript 命名约定。

我们应该对访问器使用`get`和`set`关键字。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**