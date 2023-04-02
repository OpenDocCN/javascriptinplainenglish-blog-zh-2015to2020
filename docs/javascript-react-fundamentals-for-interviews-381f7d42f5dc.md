# 面试的 JavaScript 和 React 基础

> 原文：<https://javascript.plainenglish.io/javascript-react-fundamentals-for-interviews-381f7d42f5dc?source=collection_archive---------1----------------------->

## 如何解决有关 call()、apply()等问题

![](img/4eb1f11442681c152c80415d00163726.png)

# JavaScript 中的变量声明

*区别*`*var*`*`*let*`*&*`*const*`*

*   *使用`const`分配的变量是只读的，这意味着一旦使用`const`初始化，它们就不能被重新分配*
*   *在新的 JS 版本中引入另外两个关键字`const` & `let`的主要原因是允许程序员为他们定义的变量决定范围选项*

***>所有这些关键字的范围字***

*`var`:声明变量的函数*

*`let` & `const`:声明变量的块*

```
*Block scopes are what you get when you use if statements, for statements or write code inside curly brackets*
```

***>举例:设 vs var in for 循环***

```
*0 'inside for loop'
1 'inside for loop'
2 'inside for loop'
3 'inside for loop'
4 'inside for loop'
5 'outside for loop'*
```

*如果您用`let`替换`var`，您将得到错误`ReferenceError: i is not defined`，因为`let`是一个块范围，而`i`只能在花括号内访问*

***>经验法则***

*   *不要使用`var`，因为`let`和`const`更具体*
*   *默认为`const`，因为它不能被重新赋值或重新声明*
*   *当您想在将来重新分配变量时，使用`let`*
*   *总是喜欢使用`let`而不是`var`和`const`而不是`let`*

# *对象析构*

***>对象析构***

*   *当你必须从你的状态或者你的组件中的属性中访问大量的属性时，你可以在 JavaScript 中使用**析构**赋值，而不是一个一个地把它们赋值给一个变量*

**使用**

**而不是**

***>对象析构别名***

*您可以使用别名重命名它们*

```
*Jhon // which is **student.name***
```

***>物体析构反应状态***

*&*

***>剩余破坏***

*   *它通常用于分割一个对象的一部分，但保留另一个对象的剩余属性*

# *扩展运算符*

*Spread 操作符实际上将数组的内容扩展到它的元素中，这样就可以进行连接等操作。更容易的*

**相当于**

***>为什么 Spread 运算符在 React 中如此得心应手***

*   *您可以很容易地在两个数组中间添加一些元素*

```
*[...a, 'something', ...b];*
```

*   *您可以使用此运算符克隆阵列*

```
*clone = [...a];*
```

*   *您可以使用 Spread 运算符轻松地组合两个对象，并向该对象添加额外的属性*

# *数组函数*

*箭头函数是用 JavaScript 编写函数的一种更简洁的方式*

***>箭头函数有一个隐式返回***

**与*相同*

***经验法则***

*   *如果函数体中只有一个语句，那么你可以省略`return`关键字和花括号*
*   *在开头省略`function`关键字*
*   *如果只有一个参数，也可以省略参数括号*

# *` this '关键字&绑定*

***隐式绑定***

*   *当使用点符号来调用一个对象或一个类的对象中的函数时，我们说`this`被隐式绑定了*
*   *点号 左边的东西就是`this`*

***>显式绑定***

*独立函数可以在调用时显式绑定到对象*

***>调用()***

*   *使用`call()`函数将函数`printName()`显式绑定到`Developer`类的`me`对象*
*   *如果调用`printName()`时没有绑定任何对象，它会将名字和姓氏打印为未定义，因为`this`是未定义的*

***>应用()***

*   *可以使用 **call()** 将参数传递给函数*
*   *但是，如果您不想单独传递每个参数，而是将所有参数作为一个数组传递，您可以使用 **apply()** 函数*

***> bind()***

*当在一个函数上被调用时，`.bind()`设置一个`this`上下文并返回一个绑定了`this`上下文的新函数*

***>新装订***

*`this`可以像在类中一样在函数中显式定义*

# *希望你在面试和求职中好运*

*![](img/d3093507596fff16eb71bdf2f3226ca8.png)*

*Photo by [Free To Use Sounds](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

*【JavaScript 用简单的英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)*