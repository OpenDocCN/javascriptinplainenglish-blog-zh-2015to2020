# 如何用 React 处理表单和输入

> 原文：<https://javascript.plainenglish.io/a-better-way-to-handle-forms-and-input-with-react-e01500ac73c?source=collection_archive---------2----------------------->

## React 表单和状态管理的权威指南

## 在 React 状态下优雅地管理表单和输入值(第一部分)

![](img/62ccd4650deec4a7b8d3bac2c7314dfe.png)

Photo by [Brandon Zack](https://unsplash.com/@brandonzack?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这篇文章是深入探讨如何使用表单和 React 处理输入数据的系列文章的第一部分。

如果您使用过 React，那么您已经知道使用表单不是一件容易的事情。它比原生 HTML 表单复杂得多。在我看来，这些是主要的难点:

*   迭代元素。
*   管理输入字段数据以获取输入值。
*   即使是非常简单的表单，也有大量的样板代码。
*   表单验证和在正确的位置显示错误消息。

在这篇文章中，我们将介绍如何编写一个`Form`组件，并将其用作一个特设(高阶组件)来维护每个输入数据并在特设级别添加验证。这一部分应该:

*   **渲染它的所有子节点。**
*   **维护跟踪字段数据的状态。**
*   **通过上下文 API 提供字段/表单数据。**
*   **提供向其状态添加新字段的功能。**
*   **提供更新和验证字段值的方法。**

现在让我们开始编写组件代码。我们将逐个分析每个部分，并为每个部分添加代码:

*   **渲染所有表单子级:**

这是一个相当容易的部分。我们需要做的就是创建一个`function`组件。而`children`是这样渲染的:

basic form component

我使用“Form”作为类组件。我发现通过`useState`钩子更新和维护字段的值有点困难。您可能需要编写另一个钩子来获得前一个状态。这超出了本文的范围。

*   **维护表单状态保存输入字段数据:**

在这一部分中，我们将为表单组件中的字段数据添加一个占位符状态。我们将使用`state`来存储现场数据，如下所示:

use the form component state to save fields data and errors

我现在添加了两个状态部分:

*   是一个对象，将包含所有字段数据，如字段值、id、类等。
*   `errors`字段将有字段 id 和相关错误的键值对。如果没有错误，该值将是一个空字符串。

我们将在这篇文章的下一部分看到如何更新/添加一个字段。

*   **通过上下文 API 提供字段/表单数据:**

现在我们已经创建了状态占位符来保存字段数据。让我们创建一种在字段组件级别提供这些数据的方法。我更喜欢上下文 API:

use React context API to share the fields data

在这里，我从表单上下文分享`fields`、`setFields`、`errors`。`setFields`目前有虚拟代码。它将用于设置字段值。表单子级可以通过使用上下文来访问这些项目，如下所示:

accessing form data in a custom Input field.

这是一个虚拟的“输入”组件，用于访问表单上下文项(字段、错误等)。).我们在 onChange 事件中使用了`setField`函数。这个输入组件只是为了显示如何使用表单上下文中的字段数据。我将写另一篇关于如何创建一个合适的输入字段组件的文章。

*   **提供向其状态添加新字段的功能:**

我们使用“Input”组件中的“setField”函数来设置字段值。但是我们仍然没有任何方法向表单上下文添加新字段。我们需要一个函数将新字段添加到上下文中的`fields`属性。调用此函数时，它会向上下文中添加一个新字段，其中包含所有默认值:

Form component with `addField` and `setField` functions.

我已经更新了组件中的一些东西。`setFields`函数设置字段值。您可以使用此函数在事件触发器上或以编程方式设置/更新字段值。`addField`函数将字段添加到表单状态。当组件呈现时，此函数只应调用一次。让我们在一个虚拟输入组件中看看这一点:

Dummy input accessing form data.

注意在`useEffect`钩子中调用了`addField`函数。它只会被调用一次。`setFields`由`onChange`事件触发。可以在任何地方使用适当的值调用该函数来设置字段值。

*   **提供验证字段值的方法:**

字段验证有两个部分:a)表单应该提供一些预定义的验证规则，b)输入字段必须告诉表单应该根据哪些规则(预定义)来验证字段，如果规则不存在，输入应该提供一个定制的验证功能。现在让我们看看如何实现这一点:

*   预定义验证:

预定义验证是用正则表达式匹配输入字符串的基本功能。如果字符串不匹配正则表达式规则，它将返回一个错误消息。

Basic predefined validation rules.

出于演示目的，我只添加了两个验证规则。您可以根据业务需要添加更多规则。在添加了一些自定义规则之后，我们将会看到如何使用这些规则。

*   自定义验证:

正如您可能已经猜到的，要通过预定义的验证规则来验证一个字符串，您需要调用`rule.test` (regex test)函数。为了使事情简单和统一，定制的规则也将是一个对象，并将有两个属性:`rule`和`formatter`。这两个规则的唯一区别是预定义规则将在 prop `validate`中作为管道操作符(|)分隔的字符串传递。并且一个定制的规则将作为一个对象在 prop `customRule`中传递。

我在表单组件中添加了字段验证逻辑:

form component with field validation logic.

我通过上下文公开了`validateField`函数。输入字段根据需要调用`validateField`。让我们称之为输入中的值变化(仅为演示目的):

Input calls the `validateFeild` function on field value change.

现在我们有了一个全功能的表单和输入组件。让我们看看如何使用它来创建一个表单:

所以，这就是你如何根据自己的需要制作自己的表单和管理状态。这里有一个[代码沙盒链接](https://codesandbox.io/s/polished-river-ywyhx)供大家参考。由于这篇文章只是给你一个想法，这段代码并没有照顾到所有的边缘情况。如果你想要生产就绪的东西，请随时查看我的[“反应形式”回购](https://github.com/iiison/react-form)。这可以处理所有的极端情况，并具有一些额外的功能，您可能会发现这些功能在现实生活中很有用。

我将很快撰写另一篇文章，介绍如何添加自定义输入组件，以及如何将多个组件组合成一个输入来获得复杂的值。

这是我在这里的第一个帖子。请分享你的想法，让我知道你是否想添加或删除任何东西。快乐编码:)