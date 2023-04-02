# 如何写出每个程序员都喜欢维护的有效代码

> 原文：<https://javascript.plainenglish.io/how-to-write-effective-javascript-code-that-every-programmer-loves-to-maintain-a490b42b9b9f?source=collection_archive---------8----------------------->

## 要不要写更易维护的代码？运用坚实的原则

![](img/052ee6296494cbb703f98b7e5321e76c.png)

Photo by [Alexander Sinn](https://unsplash.com/@swimstaralex?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为一名程序员，你可能听说过 SOLID。你曾经在你的项目中应用过这个原则吗？

在本文中，我将带您了解 SOLID。相信我，一旦你掌握了它，你的同事会很乐意维护你的代码。

# 单一责任原则

一心多用会降低你的工作效率。你不可能一次完成十个高质量的任务，更别说你有时间去完成它们。重点是挑一个做好。

同样的心态也应该适用于编程。每当你写一个函数，让它仅仅做一件事。函数名中没有和/或。如果你写了类似于 **validateAndLogin()** 的东西，你就做错了。

保持你的功能简明扼要。对你写的每一个函数使用你的常识，如果你觉得一个函数的一部分可以被提取到另一个函数中，不要犹豫去做。

一个什么都做的函数是丑陋的，难以阅读的，你肯定想下地狱而不是维护它。

让我们以 **validateAndLogin()** 为例。

```
function validateAndLogin(username, password) {
  // Validate username and password
  // Then login
}
```

为了满足 SRP，将其分成两个独立部分——验证和登录。

```
function validate(username, password) {
  // Validate username and password
}function login(username, password) {
  // login
}
```

# o:开闭原则

你写的每一个函数/模块都应该对扩展开放，对修改关闭。这意味着无论何时你想要做出改变，比如一个新的特性，你应该添加新的代码，而不是修改现有的代码。

例如，您希望根据用户类型显示不同的徽章。

```
function showBadge(userType) {
  if (userType === ‘normal’) {
    // Show normal badge
  } else if (userType === ‘admin’) {
    // Show admin badge
  } else (userType === ‘author’) {
    // Show author badge
  }
}
```

想出示 vip 胸卡怎么办？除了编辑 showBadge 函数为其添加一个 **if** 之外，没有其他方法。每当你需要展示一个新徽章的时候, **if/else** 就会一直持续下去。

以下是你如何让这种情况符合开闭原则:

```
function showNormalBadge() {
  // Show normal badge
}function showAdminBadge() {
  // Show admin badge
}function showAuthorBadge() {
  // Show author badge
}
```

需要出示新的徽章吗？只需添加新功能，无需修改现有功能。

```
function showVipBadge() {
  // Show vip badge
}
```

你看。您不需要编辑现有的功能。每次需要展示新徽章时，只需添加一个函数来扩展代码库即可。

# 李:里斯科夫替代原理

每个子类必须可以替换它们的父类。

正如 Bob 叔叔所说，“要用可互换的部件构建软件系统，这些部件必须遵守一个允许这些部件相互替换的契约。”

例如:

```
class Programmer {
  constructor(name: string) {
  } getSkills() {
  }
}class Junior extends Programmer {
  getSkills() {
    return ‘Junior skills’;
  }
}class Senior extends Programmer {
  getSkills() {
    return ‘Senior skills’;
  }
}function getProgrammersSkills(programmers: Array<Programmer>) {
  for (let i = 0; i < programmers.length; i++) {
    console.log(programmers[i].getSkills());
  }
}
```

在 *getProgrammersSkills()* 函数中可以看到， *getSkills()* 方法的实现是可以互换的。这样，你就遵守了利斯科夫替代原理。

# I:界面分离原理

我不知道为什么我的一些同事经常强迫客户端依赖它不使用的接口或方法。

JS 中没有本机接口，但这应该没关系。如果你不需要依赖一个函数或模块，那就不要依赖。

让我们看看下面的例子:

```
class Programmer {
  constructor(name: string) {}

  showJuniorSalary() {
  } showSeniorSkills() {
  }
}class Junior extends Programmer {
  showJuniorSalary() {
    console.log(‘junior salary’);
  } showSeniorSkills() {
    // Do nothing
  }
}class Senior extends Programmer {
  showJuniorSalary() {
    // Do nothing
  } showSeniorSkills() {
    console.log(‘senior skills’);
  }
}
```

在父类中同时有 *showJuniorSalary()* 和 *showSeniorSkills()* 是没有意义的。我们永远不会用*senior . showjuniorsalary()*和*junior . showseniorskills()*。

我知道这个例子很荒谬，但你应该明白了。

# 依赖倒置原则

这个原理也被称为控制反转。

这意味着抽象不能依赖于细节。细节必须依赖于抽象。而且高层模块一定不能依赖低层模块。

抽象不应该知道细节是如何实现的。否则，你就违反了规则。

例如，不要这样做:

```
function calculateProgrammerSalary(rate) {
  let senior = new SeniorProgrammer();
  return senior.workTime() * rate;
}
```

calculateProgrammerSalary()应该不知道高级是怎么实现的。让我们这样做:

```
function calculateProgrammerSalary(rate, programmer) {
  return programmer.workTime() * rate;
}
```

这样更好。计算器不依赖于程序员类的具体实现。现在，您可以将 senior 或 junior 作为参数传递。

写代码的时候，要时刻保持扎实的头脑。

不仅仅是关乎代码的质量，还要表现出你对它的在乎，对每一行代码都用心——一种你应该成为的程序员。

[](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) [## Visual Studio 代码的 9 大 JavaScript 扩展加速您的开发

### 谁想更快更容易地编码？

medium.com](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) [](https://medium.com/javascript-in-plain-english/7-simple-ways-to-conditionally-render-components-in-react-a3170d0cd9e0) [## 在 React 中有条件地呈现组件的 7 种简单方法

### 为用户类型 A 显示红色用户名，为用户类型 B 显示蓝色用户名，或者只为登录用户显示仪表板…

medium.com](https://medium.com/javascript-in-plain-english/7-simple-ways-to-conditionally-render-components-in-react-a3170d0cd9e0) 

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**