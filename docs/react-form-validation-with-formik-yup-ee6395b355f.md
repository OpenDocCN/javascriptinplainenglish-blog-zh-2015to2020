# 使用 Formik 对表单验证做出反应+是

> 原文：<https://javascript.plainenglish.io/react-form-validation-with-formik-yup-ee6395b355f?source=collection_archive---------0----------------------->

## 使用 Formik 和 yey 以一种简单的方式构建一个包含其验证的表单

![](img/35ecb00f2fddb4b2bce588ca6231c910.png)

Photo by [Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

创建表单验证是一项很累但很重要的任务。以正常的“反应方式”构建表单及其验证可能会令人沮丧，因为您需要创建从设置状态到表单提交的过程。 [Formik](https://jaredpalmer.com/formik/) 可以帮助你以更简单的方式创建表单。还有[是](https://github.com/jquense/yup)，它提供了一系列的功能，所以你可以让验证过程更容易。

## 本文将涉及哪些内容？

1.  使用 Formik 设置表单字段和初始值
2.  处理用户输入
3.  使用“是”创建验证
4.  处理提交表格

在本文中，我将向您展示如何构建一个简单的注册页面并处理表单验证。它包括全名、电子邮件、密码和密码确认字段。

# 1.使用 Formik 设置字段和初始值

这是我做的一个注册表格的例子。

可以看到，没有声明任何状态来存储字段的值。我们不会为此使用`useState`挂钩。相反，我们将使用 Formik 来存储字段值。

在此之前，让我们安装 Formik。

```
npm install formik --save
// or
yarn add formik
```

Formik 提供了一个定制的挂钩，返回所需的所有状态和助手，它被称为`useFormik`。它接受一个定义表单配置的对象。

我们通过`initialValues`为我们的形式到`useFormik`挂钩。在`initialValues`声明的物业名称应与`<input />`的名称一致。现在可以从`formik`变量访问返回的状态和助手。有了这些，我们就可以。获取字段值并将其放在我们的输入组件上:`value={formik.values.full_name}`。无需反应`useState`。

# 2.处理用户输入

接下来是捕获用户输入并将其存储在我们的 Formik 状态中。Formik 为我们提供了一系列的助手功能，其中之一就是`handleChange`。此函数接受`event`对象(从`onChange`返回)，并使用字段名将值存储到适当的 Formik 状态。

```
<input
  type="text"
  name="full_name"
  value={formik.values.full_name}
  onChange={formik.handleChange} 
/>
```

无需将该字段名称传递给`handleChange`，因为其将获得带有`event.target.name`的字段名称。

使用 Formik 处理用户输入是一项简单的工作。使用`handleChange`函数，我们可以将它放入所有输入的`onChange`中。

# 3.使用 Yup 创建验证

设置表单和处理用户输入很容易，验证值就不容易了。我们必须创建值检查器来验证我们的所有字段。多亏了 Yup，我们可以使验证过程更容易！

什么是呀？引用自 [Yup 文档页面](https://github.com/jquense/yup):*Yup 是一个用于值解析和验证的 JavaScript 模式构建器*。我们可以使用 Yup 创建一个验证模式，并将其传递给`useFormik`钩子。Formik 为 Yup 提供了一个名为`validationSchema`的选项。

首先，让我们安装。

```
npm install yup --save
// or
yarn add yup
```

然后，我们可以在页面上导入 Yup，并为表单创建验证模式。

上面我做了两件事，即用 Yup 创建验证模式并显示错误消息。

## 用 Yup 创建验证模式

正如您在上面看到的，这个模式非常简单。我们只是链接验证函数来创建一组验证模式。例如，我们希望我们的`full_name`值不为空，是一个字符串，最少 2 个字符，最多 15 个字符。然后使用 Yup，我们可以使用`required`、`string`、`min`和`max`功能。错误消息也可以传递给每个函数。

我们通常遇到的更复杂的验证是电子邮件验证和密码确认。幸运的是，Yup 还为我们提供了验证电子邮件和值相等的功能。Yup 有更多的功能。关于 Yup 的详细信息可以在[它的文档页面](https://github.com/jquense/yup)中找到。

## 显示错误消息

`useFormik`钩子返回可用于显示错误信息的`errors`和`touched`状态。

*   `errors`:表单验证错误的对象。
*   `touched`:布尔对象，表示该字段是否被访问过。

Formik 的验证功能会在用户输入的每一次击键时运行。这意味着`errors`对象将包含任何给定时刻的所有验证错误。因此，我们可以显示这样的错误消息。

```
{formik.**errors**.full_name && <p>{formik.**errors**.full_name}</p>}
```

但是这段代码会在用户在字段中输入内容后立即向用户显示错误消息。通常，我们希望在他们在该字段中输入完之后显示一条错误消息(而不是在他们输入的第一个字符之后)。这就是为什么我们需要`touched`状态。

```
{formik.errors.full_name && formik.**touched**.full_name && (
  <p>{formik.errors.full_name}</p>
)}
```

通过检查`touched`状态，现在我们可以在他们处理完该字段后显示错误消息。

# 4.处理表单提交

为了处理表单提交，我们必须将一个`onSubmit`选项传递给`useFormik`钩子。我们可以在`onSubmit`选项功能中访问提交的值。您可以对这些值做任何您想做的事情，比如将它用作 API 的有效负载。

```
const formik = useFormik({
  initialValues: {
    ...
  },
  validationSchema: Yup.object({
    ...
  }),
  **onSubmit**: values => {
    alert(JSON.stringify(values, null, 2));  
  }
});
```

Formik 提供的另一个助手功能是`handleSubmit`。如果验证没有错误，它将执行我们传递给`useFormik`的`onSubmit`函数。我们可以把`handleSubmit`功能放在`<form>`上。

```
<form onSubmit={formik.handleSubmit}>
```

这里是注册表单的完整代码，以及它与 Formik 和 Yup 的验证。

# 结论

有了 Formik + Yup，我们可以使表单验证更容易。Formik 为我们提供了一堆状态和助手来创建表单。然后，Yup 为我们提供了一整套创建验证模式的方法。

这是我们上面讨论的一个工作示例。

谢谢！