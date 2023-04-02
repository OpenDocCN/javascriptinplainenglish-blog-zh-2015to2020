# 打字稿初学者指南

> 原文：<https://javascript.plainenglish.io/the-beginners-guide-to-typescript-fb2ad35bc735?source=collection_archive---------14----------------------->

![](img/90429a29052ce0df91cc92faebab7d3a.png)

新年快乐，欢迎来到 2020 年！我希望每个人都有一个有趣和安全的除夕。我最近一直忙于一个项目，所以我已经有一段时间没发帖子了，但我的项目还没有完成，我很高兴能和你们分享成果。我刚刚完成了我的第一本书，并在亚马逊上自助出版！

# 我的灵感

我为什么要写这本书？在过去的几年里，我对 TypeScript 变得非常热情，我想我应该和其他人分享我的热情！TypeScript 有一个蓬勃发展的生态系统，我认为它只会在 2020 年变得更大、更受欢迎。也就是说，我认为程序员了解一点这方面的知识会有好处。如今，TypeScript 可以在前端使用 Angular、React 或 Vue，在后端使用 NodeJS 来支持完整的应用程序。如果我们去 GitHub 的 Octoverse 网站，我们可以看到 TypeScript 已经破解了 10 大编程语言，并且它开始攀升！

[](https://octoverse.github.com/#top-languages) [## 八分体的状态

### 在过去的一年里，1000 万新的开发人员加入了 GitHub 社区，贡献了超过 4400 万个存储库…

octoverse.github.com](https://octoverse.github.com/#top-languages) 

# 为什么是打字稿

TypeScript 越来越受欢迎，但我首先要说的是，某些东西受欢迎并不意味着它总是正确的。然而，TypeScript 的流行促使许多开发人员，比如我，使用它并爱上它。

## 上手很快，到处跑

TypeScript 是 JavaScript 的超集，运行在 NodeJS 上。这意味着启动 JavaScript 项目与启动 TypeScript 项目是一样的。你只需要安装一些额外的依赖项就可以了！就像 JavaScript 一样，它可以在任何地方运行，比如浏览器、后端、平板电脑和几乎无处不在的手机。

## 惊人的类型系统

给 JavaScript 带来了一个强(ish)类型系统，使得这种语言更容易使用。现在，TypeScript 项目有了更多的结构，以及帮助我们避免错误的类型安全。我给你举个例子。

```
// JavaScript
const me = {
  firstName: 'Sam',
  lastName: 'Redmond'
  age: 100,
  fullName: () => `${this.firstName} ${this.LastName}`
}
```

在这个例子中，我们用一些属性和函数创建了一个人的对象文字。在函数中，我们可以访问`this`，这是对象文字的范围，因此我们可以使用 firstName 和 lastName。让我们看一个 TypeScript 示例。

```
// TypeScript
class Person {
  firstName: string;
  lastName: string;
  age: number; constructor(firstName: string, lastName: string, age: number) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  } fullName() {
    return `${this.firstName} ${this.lastName}`;
  }}const me = new Person('sam', 'redmond', 100);
```

您将注意到的第一件事是，我们通过为 person 对象创建一个类，充分利用了面向对象编程(OOP)的优势。我想说的是，我们可以用普通的 JavaScript 来实现，但是，我们也可以创建一个对象文字，就像 JavaScript 示例中那样。这两种方法在 JavaScript 中都是有效的，但是我们只能在 TypeScript 的类中访问`this`。这意味着 JavaScript 示例在 TypeScript 中编译时会抛出错误。

你可能会觉得这剥夺了你的选择，但事实并非如此。通过实施这一策略，它使您更多地考虑您的代码，并帮助它更具可扩展性。我再给你举个例子。

```
class Student extends Person {
  gpa: number;   constructor(firstName: string, lastName: string, age: number, gpa: number) {
    super(firstName, lastName, age);
    this.gpa = gpa;
  }}const me = new Student('Sam', 'Redmond', 100, 4.0);console.log(me.fullName()); // "Sam Redmond"
```

使用 TypeScript，我们可以利用继承的原理来扩展我们对 person 类所能做的事情。如您所见，我们可以通过扩展 Person 类并使用`super`函数传递属性来简化 Student 类。我们现在可以访问 Person 类的所有属性和函数，以及 Student 类中的任何函数。这是一个非常好的方法来简化你的代码，让所有的东西都更可读和可维护。

# 这本书

写这本书是一次很棒的经历，我真的希望你们会买它，像我一样爱上 TypeScript。感谢阅读我的文章，如果你想购买我的书，这里有链接！

[亚马逊图书](https://www.amazon.com/Beginners-Guide-TypeScript-Sam-Redmond-ebook/dp/B083C5S9LV/ref=sr_1_2?keywords=The+Beginner%27s+Guide+to+TypeScript&qid=1577981888&sr=8-2)

这是我写的第一本书，请留下一些反馈！

如果你喜欢这篇文章，我希望你[加入我的邮件列表](https://email.sam-redmond.com/)，在那里我会发送额外的提示&技巧！

如果你觉得这篇文章有帮助、有趣或娱乐性[请给我买杯咖啡](https://email.sam-redmond.com/products/throw-me-a-bit)来帮助我继续发布内容！