# JavaScript 干净代码—水平格式

> 原文：<https://javascript.plainenglish.io/javascript-clean-code-horizontal-formatting-7d5eab18ed87?source=collection_archive---------5----------------------->

![](img/9fd7106e9070a2e91b7487936cc403ad.png)

Photo by [Frederik Schweiger](https://unsplash.com/@flschweiger?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

以易读的方式格式化代码是保持代码整洁的重要部分。没有正确格式化的代码需要更多的脑力来解释和理解。

在本文中，我们将研究如何一致地格式化 JavaScript 代码，以便通过查看水平格式可以容易地阅读它们。

# 水平格式

随着屏幕比过去更大，我们可以有比以前更长的水平线。

80 个字符是过去的标准，但是现在 100 到 120 个也可以。

关键是大多数人不应该水平滚动来阅读我们的代码。

# 横向开放度和密度

在一行代码中的一些实体之间应该有一些空格。变量和操作符之间是放置空格的好地方。此外，文字和运算符之间的间距也很好。

我们不需要在方法名和左括号之间有空格。它没有运算符和变量或文字之间的区别大。

对于箭头函数，我们应该在右括号、粗箭头和左括号之间留一个空格。

例如，具有清晰水平格式的类可能如下所示:

```
class Calculator {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  } add() {
    return this.a + this.b;
  } subtract() {
    return this.a - this.b;
  } multiply() {
    return this.a * this.b;
  } divide() {
    return this.a / this.b;
  }
}
```

算术运算符之间有一个空格，方法名和每个方法的左括号之间没有空格。

每行也少于 120 个字符。

箭头函数可能如下所示:

```
const add = (a, b) => a + b;
```

我们还可以看到，参数列表在逗号后面也有空格。

# 水平线向

我们不必对齐变量声明，使它们彼此水平对齐。

例如，我们不必执行以下操作:

```
let foo = 1;
let x   = 2;
```

我们可以把它保持为:

```
let foo = 1;
let x = 2;
```

我们可以让代码格式化程序自动改变这种间距。

# 刻痕

代码文件就像一个大纲。我们从外部观察高层信息，随着深入，我们会看到嵌套的代码。

为了使层次结构可见，我们缩进了块，这样层次结构就对我们可见了。

我们可以通过增加两个空格来缩进。然而，我们通常不必自动这样做，因为代码格式化程序会替我们做。我们只需要记得运行它。

缩进适用于像条件和循环这样的块。

例如:

```
const loop = ()=>{if(true){for(let x of [1,2,3]){console.log(x)}}};
```

比以下内容更难阅读:

```
const loop = () => {
  if (true) {
    for (let x of [1, 2, 3]) {
      console.log(x)
    }
  }
};
```

我们可以很容易地分辨出第二个例子中的`if`块和`for`，而第一个例子几乎完全不可读。正如我们所见，间距和缩进是非常重要的。

![](img/0c805b115a1082322a9e82ea8c73b6d1.png)

Photo by [mkjr_](https://unsplash.com/@mkjr_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 断裂压痕

对于短函数，尤其是单行箭头函数，如果它们总共少于 120 个字符，我们可以把它们放在一行。

然而，对于其他任何事情，我们应该坚持通常的水平格式规则。

# 团队规则

如果我们在一个团队中工作，那么保持一套格式化代码的规则是很重要的。幸运的是，我们只需要在大多数时候运行团队选择的代码格式化程序。至少对于水平格式来说是这样。

像变量和函数分组这样的垂直格式规则必须在代码审查中查看，因为它们不能自动修复。

对于水平格式，我们可以使用 ESLint、JSLine 或更漂亮的工具来格式化我们的代码。

我们只是在需要格式化代码的时候自动运行它们。

像 Visual Studio Code 和 Sublime 这样的现代文本编辑器也有插件来格式化代码。

有各种各样的预设规则，比如这些短绒附带的默认规则，也有替代规则，比如 Airbnb 规则。

团队可以就选择哪一个达成一致，然后将其添加到他们的代码中，然后水平格式将自动完成。

# 结论

水平格式化代码有一些规则。我们应该对块进行适当的缩进，以便开发人员可以遵循代码。

变量或文字和操作符之间应该加空格，这样我们可以更容易地看到操作。

每行应该不超过 120 个字符，这样我们就不必来回滚动来阅读一行代码。

所有这些事情都可以由 ESLint、JSLint、appellister 这样的程序自动完成。它们可以与默认规则一起使用，也可以与其他规则一起配置，如适用于 ESLint 的 Airbnb 林挺规则。

大多数现代代码编辑器和 ide，如 Visual Studio Code 和 WebStorm，也有内置的代码格式化程序或作为扩展提供的代码格式化程序。

它们有助于保持一致的简洁风格，而无需开发人员为水平代码格式做额外的工作。

【JavaScript 用简单的英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)