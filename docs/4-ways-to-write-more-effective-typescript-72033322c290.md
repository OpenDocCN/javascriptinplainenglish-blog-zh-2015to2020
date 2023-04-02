# 编写更有效的打字稿的 4 种方法

> 原文：<https://javascript.plainenglish.io/4-ways-to-write-more-effective-typescript-72033322c290?source=collection_archive---------1----------------------->

## 编写有效的 TypeScript 包括理解其目的、重新思考您的方法、避免常见的不良做法以及使用 TypeScript 的功能。

![](img/006f2caa976d97a477b8da45dff14710.png)

Finding his inner TypeScript — The TypeScript Mindset

# 打字稿思维模式

当你有效地使用它时，TypeScript 是一种强大的语言。

这意味着，尽管很难，脱离 JavaScript 思维模式，采用类型脚本思维模式。这意味着**在你写代码的时候采用不同的个性，**改变你思考过程的方式和你通过语法表达自己的方式。

因此，产生的 TypeScript 代码将提供对开发人员思维过程的更深入的了解。

> TypeScript 允许您更清楚准确地定义意图。

将 JavaScript 和 TypeScript 视为两种不同的个性有助于认识到，要成为一名有效的 TypeScript 程序员，您必须适应哪些个性特征。

## 如果 JavaScript 是一个人，它应该是“随和的”，“敢于尝试任何事情”的类型

我们都知道那种类型。那种你可以从中取乐的类型，但是当事情发展到严重的时候，你不应该太依赖他们。他们有把生活中重要的事情搞砸的倾向；最重要的时候。**他们通常需要大量的帮助和指导，才能被认为是可靠的。**

用 JavaScript 术语来说，这种额外的帮助和指导意味着额外的测试、类型检查和冗长的注释。TypeScript 不太可能需要的额外保证。

## TypeScript 讨厌这些类型

TypeScript 不信任任何人。它不会盲目地相信任何事情，希望以后不会引起问题。TypeScript 希望事先了解一切。它想看到你的思维过程和意图。没有懒惰。没有遗漏。

TypeScript 希望你告诉它从你的代码中可以得到什么，这样它就可以帮助你。

## 打字稿开发人员的积极心态

> TypeScript 需要思维方式的转变，变得更加主动、严格和深思熟虑。

TypeScript 为我们提供了支持这种积极心态的特性。我们获得了显式注释代码的能力，通过静态类型化为工具提供了所需的信息，从而改善了开发体验。它允许我们达到用 JavaScript 很难达到的静态分析水平。它能让我们在虫子出现之前就抓住它们。

JavaScript 太宽容了，但是 TypeScript 试图改变这一点。

# 打字稿的注意事项

这些该做的和不该做的是特定于 TypeScript 的**(不是 TypeScript 和 JavaScript)。在这些该做和不该做的事情中有一个共同的主题，那就是**不受欢迎的类型脚本行为是出于对他们的代码暂时像 JavaScript 一样运行的渴望**。**

虽然如果您有编写 JavaScript 的长期历史，那么 TypeScript 可能会暂时影响生产率，但是一旦您掌握了这种语言，除了质量和可读性之外，生产率将会超过 JavaScript 所能达到的水平。

> 如果你坚持使用 TypeScript，你将会爱上它为你的代码所做的一切，并且你会对代码充满信心。

因此，不使用类型似乎可以节省时间。然而，那是短期的。**坚持 JavaScript 行为的长期负面后果超过了任何最初的好处。**

后果通过未被发现的错误、糟糕的可读性和更大的(更少可重用的)代码库表现出来。

不可避免的是，你将不得不在很短的时间内正确地学习 TypeScript(所以你最好尽快完成)。

因为归根结底，创建 TypeScript 是为了解决 JavaScript 的缺点。

> 我在上一篇文章中提供了关于 TypeScript 如何“保存”JavaScript 的更详细的解释。

[](https://medium.com/swlh/typescript-rescued-the-javascript-language-bfc944c0f96b) [## TypeScript 拯救了 JavaScript 语言

### 它通过静态类型、改进的开发人员工具和 OOP 特性将可伸缩性带到了一个有缺陷和令人沮丧的…

medium.com](https://medium.com/swlh/typescript-rescued-the-javascript-language-bfc944c0f96b) 

> 从长远来看，TypeScript 可以为您节省大量时间和金钱。

**所以这里有一些写更有效的打字稿的技巧…**

# 1)使用严格模式

使用它。你以后会感谢自己的。就像在`tsconfig.json`中的编译器选项中提供`strict: true`一样简单。

> 严格模式默认为`false`。

严格模式通过更严格的类型来改进验证。它为我们代码中的类型提供了更强的保证。将`strict`转到`true`会启用属于严格系列的所有功能。然后由您根据需要禁用各个选项。严格模式提供了广泛的类型检查行为。

## 严格系列—严格模式选项

当`strict: true`时，一些重要选项作为严格系列的一部分被启用。

这些是…

*   `**strictNullChecks**` —当这是`**false**`时，`null`和`undefined`被 TypeScript 编译器忽略；这意味着您可以将任何值赋给`null`或`undefined`而不受 TypeScript 的干扰。这不能保证变量是`null`或`undefined`，通常会导致无法捕捉的错误。

> 将`*strictNullChecks*`设置为`*true*`指示编译器开始关心`*null*`和`*undefined*`，这意味着从那时起**你必须明确定义变量何时可以是** `***null***` **或** `***undefined***`类型。这可防止无意中操作`*null*`值。

*   `**noImplicitAny**` —这意味着当`true`，**时，你必须声明变量、函数、参数等的所有类型。这防止我们退回到类似 JavaScript 的函数:定义一个没有类型的函数。**

> 因此，`*const func = (param) => 'param is ' + param*`将引发错误，而`*const func = (param****: string****) => 'param is ' + param*`将是有效的 TypeScript。

## 为什么使用严格模式？

**在生命周期的早期修复问题会更便宜**。首先，根据代码的质量/状态，您可能会被看到的错误数量淹没。

但是重要的是要记住，TypeScript 比 JavaScript 更容易抱怨。它的目标是构建高质量的应用程序，具有更强的类型和更少的错误。

严格模式是符合 TypeScript 打字系统的最容易的途径。它为我们提供了智能的洞察力，帮助我们更好地理解代码中的关系；我们的项目分成的许多小组件、系统和服务之间的关系是相互作用的。

## 可能不使用严格模式的情况→从 JavaScript 迁移

最初你可能会尝试像`false`一样使用严格模式。

在用 JavaScript 编写的现有项目中，迁移到 TypeScript 是一种关闭严格模式会使转换更加平滑的情况。

对于新项目，**我看不出你为什么不使用严格模式**的任何逻辑理由。从项目一开始就使用它是构建您有信心的有效的 TypeScript 应用程序的一种方式。

> 你越早遵守严格的打字规则，你就能越早从你新学到的习惯中获益。

# 2)不要抑制类型

在 TypeScript 中取消类型会让您编写更冗长的 JavaScript 版本。你再也没有权利称之为 TypeScript 了。

## 用“任何”来掩盖气味

抑制类型最常见的方式是通过**转换为类型** `**any**`。

有时很容易想让编译器安静下来。尤其是如果你有一个截止日期，并且你 99.9%确定你知道你在做什么。

**然而，最好是针对原因，而不是症状。**所以也许要想想 TypeScript 为什么对你大喊大叫。也许是因为你没有正确地注释你的代码…

抑制类型给人的印象是发生了两件事情中的一件:开发人员更关心的是完成**而不是正确**或者他们没有用类型注释他们的代码，直到他们被编译器调用(这里使用了`any`类型转换)。****

可以变成**类型抑制→错误→类型抑制→错误的永无止境的循环。**

## 强制转换为“any”类型的危险

通过将一个变量强制转换为 any like `(obj as any).getName()`，你告诉 transpiler，当这个变量出现时，任何事情都会发生。您可以调用任何方法，并将其分配给任何类型的任何对象。

你是在告诉全世界‘这个变量可能是任何东西’。这是否给了你(以及其他看你代码的人)代码不会被破解的信心？

此外，在代码中的几个地方使用 type `any`意味着您将在大块代码中抑制所有与类型相关的错误。这使得 TypeScript 编译器更难工作，测试更难，可读性更差，并导致那些每个人都讨厌的耗时的 bug。

## 如果您不知道类型或无法创建类型，请使用“未知”

如果不知道类型，请使用`unknown`。

> 在没有类型断言的情况下，任何东西都可以赋给`*unknown*`，但是`*unknown*`除了自身之外不能赋给任何东西。

这意味着在类型为`unknown`的变量上调用方法会引发错误**，这是一件好事**。因为你不想调用一个你不能保证存在的方法。它会扰乱你的应用程序流程，影响用户体验。

## 正确定义您的类型以避免这种情况

如果在编写 TypeScript 代码时没有正确定义类型，您将会陷入这样的境地:

1.  您会遇到错误。
2.  意识到您应该做什么(即从工作流的开始返回并用正确的类型编写 TypeScript 代码)。
3.  选择反对，因为由于你离开它的时间长度，它现在是一个非常令人生畏的任务。
4.  多写一些糟糕的代码。
5.  **重复**。

那是一种你不想处于的情况。我们都经历过。

# 3)不要忽视警卫类型

使用鸭式类型，很难确定一个对象是否属于某种类型。对于 TypeScript 中的接口，您经常需要检查对象实现了哪个接口(数据的形状)。

然而，**如果不比较底层的区别属性，就不能可靠地检查接口**的类型相等性。

## 但是，内置的“类型”防护呢？

`typeof`守卫只能检查`string`、`number`、`bigint`、`function`、`boolean`、`symbol`、`object`、`undefined`。

**因此，检查自定义类型的相等性超出了** `**typeof**` **操作符的范围。**

> typeof 运算符只提供了一种浅层的类型检查。

使用`typeof`的类型检查过于简单，不适用于定制类型(类和接口)。

## …而‘实例’警卫呢？

只有在对**类**进行类型检查时`instanceof`保护才有价值。它检查一个对象是否是一个类的实例。它不为接口做任何事情。

因此，很容易产生假阴性；即使基础形状相同，也返回 false。因此，使用接口的`instanceof`是不可能的。

## 使用自定义类型脚本类型保护

编写 TypeScript 类型保护提供了一种在与对象交互之前检查接口类型的可重用方法。类型保护本质上是一个函数，它返回一个`boolean`。

你需要传递**对象来检查**作为你的守卫函数的参数。

```
function isPerson(user: Person | Dog) user is Person {return user.discriminator === 'person';}
```

# 4)不要忽视泛型的力量

泛型是编写可在整个项目中重用的可伸缩代码的关键。泛型是一个关键的类型脚本特性，它将**增加代码的可伸缩性**。

编写泛型代码涉及使用泛型参数，如`filter<T>(param: T[]): T[]` **，其中** `**T**` **是一个泛型类型**，它被传递到函数定义中，如`filter<User[]>(users)`和`filter<Post[]>(post)`，这允许编译器满足函数体内该类型的所有用法。

> 您可以将泛型视为动态类型的占位符。

为了确保一个函数作为通用函数工作，该函数必须具有单一的职责，并且该函数需要处理不同的输入。

## 泛型为您做了什么:

*   提倡干原则。
*   减少您编写的代码量。
*   给你多态函数的类型保证。

由于对编程行为的影响，泛型最终会降低开发成本。第一种方式是通过节省时间(和金钱),因为你通常会重新发明轮子。

其次，它**用更少的函数、类和组件促进了一个干净的代码库**。对于团队来说，这是一个更容易开发、理解和使用周围代码的环境。

最后，**它阻止我们使用**`**any**`**类型，这在编写可重用的 JavaScript 时是不可避免的。由于编译时更强的类型检查，这将导致更健壮的代码库，更少由于类型相关的错误而导致的错误。**

# **所以基本上，让 TypeScript 是 TypeScript。**

**退回到 JavaScript 习惯和模式会阻止 TypeScript 完成其设计目的。如果您通过采用 TypeScript 生活方式来满足 TypeScript 编译器，您将在以后获得回报。**

> **感谢阅读！有任何问题，请在评论中告诉我。**

## **为什么不在操场上尝试一下 TypeScript 功能呢？**

 **[## playground——探索 TypeScript 和 JavaScript 的在线编辑器

### Playground 让你以一种安全和可共享的方式在线编写类型脚本或 JavaScript。

www.typescriptlang.org](https://www.typescriptlang.org/play/)** 

# ****简明英语团队的笔记****

**你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！****

****我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们****

****一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！******