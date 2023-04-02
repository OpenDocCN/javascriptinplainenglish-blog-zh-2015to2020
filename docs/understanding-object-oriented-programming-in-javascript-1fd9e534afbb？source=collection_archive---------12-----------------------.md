# 理解 JavaScript 中的面向对象编程

> 原文：<https://javascript.plainenglish.io/understanding-object-oriented-programming-in-javascript-1fd9e534afbb?source=collection_archive---------12----------------------->

![](img/787a068fb91414473f54b96f3a2fc6dd.png)

Photo by [Nubelson Fernandes](https://unsplash.com/@nubelsondev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是一名新的 Web 开发人员，你可能已经看到过，每当你在谷歌上搜索关于 Web 开发的内容时，就会看到“面向对象编程”这个词。面向对象编程(简称为 **OOP** )是你在开发新旅程中可以学到的最重要的东西之一。

在这篇文章中，你将了解 OOP 的目的，为什么你应该关心它，以及如何开始编写面向对象的代码。

顺便说一下，只是为了让你知道有大量的视频和博客文章是关于 OOP 中真正先进的技术和思想的，所以我将把重点放在基本的思想上，只是为了让你开始。

如果你坚持到帖子的最后，我会提供一些资源，帮助你在了解基本概念后真正投入 OOP。

# 那么什么是面向对象编程呢？

根据 [developer.mozilla.or](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS) g 的说法，OOP 的基本思想是我们使用对象来建模我们希望在程序中表现的真实世界的事物。这基本上意味着我们可以用模拟现实世界的方式编写代码。

例如，假设我们想在代码中建模一个人。

```
class Person{
  constructor(name, profession){
    this._name = name;
    this._profession = profession;
  }}const newPerson = new Person("John Smith", "software consultant");
```

上面我们已经创建了代表一个人的代码。在我们的代码中，单词 **class** 是 JavaScript 中的一个[保留字](https://www.w3schools.com/js/js_reserved.asp)，它允许您创建一个“蓝图”来表示一个真实世界的对象。因为这是一个蓝图，我们可以使用 JavaScript 中的 **new** 保留字来实际创建一个基于这个类的蓝图的对象。

这里还需要注意的是，这被称为面向对象的编程，所以你最终以这种编程风格产生的所有代码都将导致对象的创建。

您创建的这些对象将基于您的类中定义的蓝图。

如果我们在控制台中记录前面代码的结果

```
const newPerson = new Person("John Smith", "software consultant");console.log(newPerson) //=> the result of this statement will look like what's in the comment below//Person {_name: "John Smith", _profession: "software consultant"}
```

如你所见，控制台会显示我们的`newperson`是`Person`类的**实例**，他有一个名字和职业，就像我们在类中声明的一样。实例这个词，只是指使用特定的类创建的东西。

所以当我说`newPerson`是`Person`类的一个实例时，我的意思是`newPerson`只是我们使用`Person`类提供给我们的蓝图制作的一个新对象。

# 构造函数和新保留字的用途

在我们上面的类中，你可能已经注意到了一个叫做`constructor()`的函数和一个叫做`new`的保留字的使用。这个函数和保留字是我们需要帮助我们在代码中开始使用 OOP 的两个最重要的东西。

`constructor()`函数是一个允许你创建一个内部有预定义值的类的函数。构造函数就像一个基本的 javascript 函数，所以你可以用参数来定义它。

作为对你的一个快速提醒，参数是你在定义一个新函数时使用的东西。**参数作为实际数据的占位符，这些数据将在函数使用时传递给函数**。

每当我们使用`new`保留字来创建我们定义的任何种类的类的新实例时，就会用到`constructor()`函数。

让我们仔细看看我们的`constructor()`函数，这样我可以更好地说明这一点。

```
//this is the constructor function used above in our Person class
constructor(name, profession){
    this._name = name;
    this._profession = profession;
  }
```

这个构造函数有两个参数，name 和 profession。这两个参数在我们的代码中用作某人实际姓名和职业的占位符。

我们的构造函数所做的就是获取传递到语句中的实际值，该语句用于创建该类的新实例。

```
//the strings "John Smith" and "software consultant" are the actual //values that we are passing into our new class instanceconst newPerson = new Person("John Smith", "software consultant");
```

# 为什么在构造函数中使用这个？

`this`可能是进入 OOP 时要考虑的最复杂的部分。详细解释使用`this`保留字需要什么将需要它自己的帖子，但是我将快速解释它在我们的`Person`类中的用法。

`this`用于告诉构造函数引用使用该类创建的对象。所以构造函数中的`this._name`引用的是`Person`类的当前实例。因此，当我们的`newPerson`变量存储我们的类的新实例的值时，我们可以从技术上调用`newPerson._name`来获得我们作为参数提供给`constructor()`函数的名称。

当以这种方式使用保留字`this`时，我们可以像存储变量一样存储某个东西的值。这实际上是我们如何在 JavaScript 中创建**实例变量**。实例变量是一个类的特定实例所特有的变量。我们可以像引用对象属性一样引用实例变量！

现在让我们回过头来充实我们的 Person 类，赋予它更多的功能。

```
class Person{
  constructor(name, profession){
    this._name = name;
    this._profession = profession;
  } dailyRoutine = () => {
    return `${this._name} wakes up, brushes his teeth and gets ready to have a great day`;
  }}
const newPerson = new Person("John Smith", "software consultant");
newPerson.dailyRoutine()//this will return: "John Smith wakes up, brushes his teeth and gets ready to have a great day"
```

在上面的例子中，我们创建了一个名为`dailyRoutine()`的方法，并让它使用我们的一个实例变量来输出一个值。

这似乎是一个非常基本的例子，但是你已经见证了 OOP 的强大。OOP 允许我们在类定义中封装多个变量和方法，我们创建的每个实例都可以访问所有这些功能。

所以我们可以使用 OOP 来清理我们的代码，并根据函数所在的类将它们组合在一起。

如果我们必须使用功能方法来做同样的事情，我们必须做以下事情。

```
const name = "John Smith";
const profession = "Software Consultant"function dailyRoutine(name){
  return `${name} wakes up, brushes his teeth and gets ready to have a great day`;
}dailyRoutine(name)//this will return: "John Smith wakes up, brushes his teeth and gets ready to have a great day"
```

我们设法使用函数式编程而不是面向对象编程来获得相同的返回值，但是使用这种方法有一个问题。

采用这种方法的唯一问题是，随着我们的程序变得越来越大，我们必须编写更多的代码，随着时间的推移，我们的代码库会变得更加混乱。即使我们使用 OOP 也会发生同样的事情，但是当我们创建的新的**方法**都被捆绑在同一个地方时，通常更容易跟踪它们。

如果我们的代码增长，并且我们使用函数式方法，我们将不得不不断地跟踪什么函数做什么，它们的参数应该代表什么功能。

好了，现在你也可以写面向对象的代码了。所以我要求你在下一个普通的 JavaScript 项目中使用面向对象的代码。随着你学习更多的编程，我想你会非常喜欢 OOP 的。

**顺便说一下，这是我承诺的额外资源:**

[https://www.youtube.com/watch?v=4l3bTDlT6ZI&list = pl 4 cuxegkcc 9 I 5 yvdkjgt 60 vnvwffpblb 7](https://www.youtube.com/watch?v=4l3bTDlT6ZI&list=PL4cUxeGkcC9i5yvDkJgt60vNVWffpblB7)

[](https://scotch.io/tutorials/object-oriented-programming-in-javascript) [## JavaScript 中的面向对象编程

### 面向对象编程是一种流行的编程风格，它从一开始就在 JavaScript 中扎下了根…

scotch.io](https://scotch.io/tutorials/object-oriented-programming-in-javascript)