# 理解 JavaScript 中的不变性

> 原文：<https://javascript.plainenglish.io/understanding-immutability-in-javascript-b10b0345086e?source=collection_archive---------11----------------------->

## 现代框架和函数式编程的核心

![](img/f3d243b735edd816b74503d5b8e3e5f7.png)

不变性是函数式编程的核心原则，现在被现代框架和面向对象程序所使用。

突变隐藏了变化，并产生意想不到的副作用，这可能会导致错误。另一方面，不可变对象一旦创建就不能更改。不过，这种变化在新版本中有所体现，这有助于我们简化软件开发，避免错误，使您的应用程序架构和金属模型保持简单。

此外，使用不变性，尽管所有更新都返回新值，但内部结构是共享的，以减少内存使用，从而显著提高整体性能。

让我们看一些例子:

在 JavaScript 中，原始值(数字、字符串、未定义值、空值、布尔值和符号值)本身不能变异。包含基元类型的变量总是指向原始值。如果你把它传递给一个不同的变量，另一个变量会得到这个值的新副本。

```
const value= 'Hello world!';
value[0] = 'P';        
console.log(value);    
//Hello worl", not Pello world!
```

另一方面，对象或数组是通过引用传递的。如果您将一个对象传递给另一个变量，它们将引用同一个对象，如果您随后从一个变量修改该对象，它们都将显示更改。

考虑这个例子:

```
const car1 = {
  brand: 'Ford',
  model: 'Mustang W-Code'
}**const car2 = car1;**car2.model= 'Thunderbird';console.log(car1 === car2); 
//trueconsole.log(car1);
//{brand: 'Ford', model: 'Thunderbird'}
```

当我们修改“car2”对象的属性时，我们会自动修改旧的“car1”对象，因为它们引用同一个对象。在大多数情况下，我们使用关键字“const”，但这并不能阻止我们改变我们的对象。一般来说，这是不受欢迎的行为，不建议这样做。

为了避免这种情况，与其将对象传递给新变量，不如创建一个新对象。这里我使用 ES6 spread 运算符来实现这一点:

```
const car1 = {
  brand: 'Ford',
  model: 'Mustang W-Code'
}**const car2 = {...car1};**car2.model= 'Thunderbird';console.log(car1 === car2); 
//falseconsole.log(car1);
//{ brand: 'Ford', model: 'Mustang W-Code'}...const car3 = {
  brand: 'Ford',
  model: 'Mustang W-Code'
}const car4 = {**...car3,** brand:'BMW'};
console.log(car3 === car4);
//falseconsole.log(car4);
//{brand: "**BMW**", model: "Mustang W-Code"}
```

您可以使用 ES6 对象实现相同的结果。assing 运算符:

```
const car2 = **Object.assing**({}, car1);
```

在这两种情况下，您都在创建一个新对象，并避免改变原始对象。

使用数组，您可以用同样的方式避免变异:

```
const aTeam = ['Murdock', 'B.A Baracus', 'Hannibal', 'Templeton'];
```

改变原始 aTeam 对象:

```
const bTeam = aTeam;
bTeam.push("Me");console.log(aTeam === bTeam;)
//trueconsole.log(aTeam);
//["Murdock", "B.A Baracus", "Hannibal", "Templeton", "Me"]console.log(bTeam)
//["Murdock", "B.A Baracus", "Hannibal", "Templeton", "Me"]
```

不出所料，只复制了项目的引用(而不是值)。我们可以快速解决这个问题，而不用使用 spread 操作符改变原始数组:

```
const aTeam = ['Murdock', 'B.A Baracus', 'Hannibal', 'Templeton'];
const bTeam = [**...aTeam**];bTeam.push("Me");console.log(aTeam === bTeam);
//falseconsole.log(aTeam);
//["Murdock", "B.A Baracus", "Hannibal", "Templeton"]console.log(bTeam)
//["Murdock", "B.A Baracus", "Hannibal", "Templeton", "Me"]
```

请注意，在数组上，“过滤”、“映射”或切片操作不会改变原始对象并创建新对象。

过滤器:

```
const bTeam = ['Murdock', 'B.A Baracus', 'Hannibal', 'Templeton', "Me"];const aTeam = **bTeam.filter**( e => e !== 'Me');console.log(aTeam === bTeam);
//falseconsole.log(aTeam);
//["Murdock", "B.A Baracus", "Hannibal", "Templeton"]
```

展开运算符+切片:

```
const bTeam = ['Murdock', 'B.A Baracus', 'Hannibal', 'Templeton', "Me"];const aTeam = [...**bteam.slice**(0,4)];console.log(aTeam === bTeam);
//falseconsole.log(aTeam);
//["Murdock", "B.A Baracus", "Hannibal", "Templeton"]
```

对于集合，另一种方法是使用 [Immutable.js](https://immutable-js.github.io/immutable-js/) 库，它提供了许多持久不变的数据结构，包括 List、Stack、Map、OrderedMap、Set、OrderedSet 和 Record。

## 结论

我希望这篇小文章已经让您了解了不变性是如何帮助您改进代码的，并且所提供的例子对您是有用的。

感谢您花时间阅读这篇文章！

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****