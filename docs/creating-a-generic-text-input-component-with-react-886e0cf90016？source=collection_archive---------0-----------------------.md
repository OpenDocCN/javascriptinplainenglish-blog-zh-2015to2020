# 用 React 创建通用文本输入组件。

> 原文：<https://javascript.plainenglish.io/creating-a-generic-text-input-component-with-react-886e0cf90016?source=collection_archive---------0----------------------->

## 使用 React 进行表单、输入和状态管理的权威指南。

## 在 React 状态下优雅地管理表单和输入值(第二部分)

![](img/8f4e632d4ff414f65c91f1dba0822b0c.png)

Photo by [Jordan Harrison](https://unsplash.com/@aligns?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这篇文章是我上一篇文章的第二部分:[如何用 React](https://medium.com/javascript-in-plain-english/a-better-way-to-handle-forms-and-input-with-react-e01500ac73c) 处理表单和输入。这篇文章是关于如何使文本输入与我们在上一篇文章中制作的`Form`兼容的指南。如果你错过了这篇文章，这里有一些亮点:

*   创建一个`class`组件:`Form`，保存所有输入字段的数据。
*   通过`context` API 共享现场数据。
*   在`Form`组件中添加方法进行更新(`setField`)并添加新字段(`addField`)。
*   添加一个方法来验证字段，并提供一些预定义的验证规则。

现在，我们将创建一个文本输入字段:`TextInput`。这一部分将:

*   **接受来自开发者的初始值和配置，并将自身注册到** `**Form**` **。**
*   **根据道具值渲染为文本输入或** `**textarea**` **。**
*   **调用自定义** `**onChange**` **函数后将其值保存到** `**Form**` **(可选)。**
*   **在字段值改变时验证自身(可选)。**

让我们从基础开始。让我们创建呈现文本输入字段的基本输入组件:

Basic Text Input

上面的组件是`TextInput`字段最简单的例子。它呈现一个输入字段，并按照 props 中传递的方式设置它的值。现在，让我们一个接一个地完成所有步骤，让这个领域更加实用:

*   **接受来自开发者的初始值和配置，并将自身注册到** `**Form**` **:**

在这一步中，我们将传递“TextInput”字段的默认配置。默认配置可以是字段 ID、默认值、占位符、字段样式、错误样式、验证规则等。然后，可能是一些像 onChange，onError 这样的函数回调。我们将在上下文中使用“表单”提供的函数“添加字段”。让我们将代码添加到组件中:

Make the TextInput dynamic by using props values.

虽然我们渲染`props`是为了让组件动态，但是如果我们仔细观察，你会发现我们并没有在 TextInput 中使用`Form`提供的字段数据。这意味着我们忽略了“单一事实来源”规则——所有数据都应该存储在一个地方，所有组件都应该引用该数据来呈现应用程序的动态部分。让我们解决这个问题，并从“表单”组件获取所有数据:

TextInput component using data from the Form.

我在组件中做了两个重要的改变:a)现在组件利用保存在`Form`的数据来呈现输入及其属性，b)组件接受一个属性:`events`，而不是单独传递每个事件。这样你就可以传递所有的本地输入事件，并且所有的事件都将通过传播`events`对象按原样传递给输入字段。`onChange`事件也有一点变化。我创建了一个不同的函数:`handleChange`，它调用`setFields`来更新处于`Form`状态的字段值，然后我们调用用户定义的`events.onChange`函数。

*   **根据 props 值将组件渲染为文本输入或文本区域:**

因为原生文本区和文本输入，两者的作用是一样的。我认为最好把它们都放在一个单独的组件中，然后根据用户的输入来渲染它们中的任何一个。让我们在组件中引入另一个道具— `type`来选择要渲染的内容:

Input component that renders text-input or textarea on the basis of props

在上面的组件中，我为组件添加了一个条件，以基于属性`type`呈现文本输入或文本区域。我还添加了一些特定组件的道具。比如，如果`type`的值是 textarea，我们就增加一个新道具:`row`。它将在 textarea 字段中指定默认的行数。当我们渲染文本区域时，我也将`value`道具名称改为`defaultValue`。你也可以为 textarea 创建一个单独的组件，但是我更喜欢这样。通过这种方式，我可以向您展示如何根据您的需求在您的自定义库中变通规则。

*   **调用自定义** `**onChange**` **函数后将其值保存到表单:**

虽然我们已经在文章的前面部分介绍了自定义 onChange 部分。这里，我们将首先将字段数据保存到表单状态，如果过程中没有错误，那么我们将调用自定义 onChange 函数。在这一部分中，我们还将了解如何添加和使用自定义事件:

Custom input onChange function with other custom events.

现在，让我们看看如何在表单组件中使用 TextInput 组件:

TextInput used in the form component.

观察上面的组件，所有的事件都在 prop: `events`中传递。请注意，事件名称应该与本地输入事件名称相同。我们没有通过上面例子中的`type`道具。该组件默认呈现为文本输入，但是，如果我们添加值为“textarea”的`type`属性。它将被呈现为 textarea。

*   **在字段值改变时验证自身:**

尽管我们已经在[之前的文章](https://medium.com/javascript-in-plain-english/a-better-way-to-handle-forms-and-input-with-react-e01500ac73c)中讨论了验证部分，现在我们将看看如何在输入中使用它。我们可以随时验证输入。但是，为了方便发布，让我们在字段值改变时验证它。我们需要做的就是从表单上下文中调用`validateField`函数来验证字段:

validate Text Input on change.

我们使用`useEffect`钩子来检测字段值的变化。如果数值改变，我们将触发`validateField`功能。我们现在还显示了字段错误。注意，错误的占位符呈现了一个包含字段错误的变量。

因此，这就完成了如何创建一个非常类似于本地文本输入字段的文本输入(或文本区域)字段。当然，你还是可以给这个组件添加很多东西的。如果你想要生产就绪的东西，请随时查看我的[“反应形式”回购](https://github.com/iiison/react-form)。这可以处理所有的极端情况，并具有一些额外的功能，您可能会发现这些功能在现实生活中很有用。你可以在 NPM 上找到它，名字是`[react-state-form](https://www.npmjs.com/package/react-state-form)`。你可以在[这个沙箱](https://codesandbox.io/s/polished-river-ywyhx)上看到这个例子的完整代码。

在下一篇文章中，我们将介绍如何使用相同的 TextInput 组件制作一个“自动建议”输入框。

## 关于作者:

Bharat 自 2011 年以来一直是前端开发人员。他对“前端开发经验”情有独钟。他喜欢学习和教授技术。他和最可爱的女人和两个宝贝双胞胎一起享受生活。

总的来说是个好人。在 [Twitter](https://twitter.com/iiisoni) 、 [Github](https://github.com/iiison) 、 [Linkedin](https://www.linkedin.com/in/iiison/) 上找到他。

## 进一步阅读

[](https://bit.cloud/blog/composable-link-component-that-works-in-any-react-meta-framework-l7i3qgmw) [## 可在任何 React 元框架中工作的可组合链接组件

### Bit 的链接组件是一个与运行环境无关的组件。您可以将此链接用于…

比特云](https://bit.cloud/blog/composable-link-component-that-works-in-any-react-meta-framework-l7i3qgmw) 

*更多内容看* [***说白了就是 io***](https://plainenglish.io/) *。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于* [***推特***](https://twitter.com/inPlainEngHQ) ， [***领英***](https://www.linkedin.com/company/inplainenglish/) *，*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*[***不和***](https://discord.gg/GtDtUAvyhW) *。对增长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) *。**