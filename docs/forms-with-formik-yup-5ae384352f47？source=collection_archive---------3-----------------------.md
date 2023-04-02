# Formik 表单&是的

> 原文：<https://javascript.plainenglish.io/forms-with-formik-yup-5ae384352f47?source=collection_archive---------3----------------------->

## 如何使用 Formik 在 ReactJS 中轻松设置、验证和提交客户数据

![](img/dda71ae9d8b262a5285a1f0211867c36.png)

Photo by [israel palacio](https://unsplash.com/@othentikisra?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 概观

开发移动应用和网站时，从用户那里获取数据很重要。当我们构建一个反应网站或一个反应本地的移动应用程序时，人们经常遇到的一个障碍是选择一个可靠的表单库来帮助捕获和管理信息——UI 组件可能很容易构建，但是捕获输入；提供经过验证的有意义的反馈并提交表单是需要考虑的很多事情。

[Formik](https://formik.org/) 是一个考虑了任何以前构建的库的所有缺点的库，并且引入了一种简单的方式来处理表单，没有任何“魔力”。有一个小的学习曲线，但一旦我们通过了这个，图书馆使形式状态管理变得轻而易举！

本文将重点介绍如何使用 Formik 创建一个简单的登录页面，并提供代码示例供参考。在本文的最后，应该提到学习曲线。就上下文而言，结构将是:

1.一般设置
2。验证输入并向用户提供有意义的反馈
3。了解如何提交表单

## 用户界面设置

React-Bootstrap 附带了许多基本组件来制作一个像样的表单——不是超级花哨，但是设置如下，下面的代码只是 UI 设置，还没有实现任何逻辑。
*注意:你可以使用任何用户界面库，有些有特殊的 Formik 插件——通过反应引导，我们可以直接使用 Formik 库。*

Simple login form set up using a few react-bootstrap components we can interact with the form, add values, but this does nothing at this point.

## 通用 Formik 设置(布线)

现成的，Formik 附带了一个直接的组件，我们把它包装在表单用户界面周围。想象正在发生的事情的最简单的方法是想象一个简单的父子方法，对孩子(字段，按钮)的更新被传递给父母(Formik Component ),来自父母的道具可以通过孩子访问，传递下来的道具给 UI 表单提供了“功能”,这些同样的道具允许孩子进行交流。

View the Setup Form Tab — Basic Formik component wired up with our card

最初，这个结构看起来令人生畏，似乎有很多事情要做，让我们把它分解一下:

1.  <formik>组件是我们的父组件，它允许我们传递描述事物的特殊道具，即 *initialValues、validationSchema 和 onSubmit(我们将在后面详细介绍这些)*</formik>
2.  当我们在<formik>组件中定义子组件时，我们定义了一个胖箭头函数，这允许我们将道具从 Formik(父组件)传递给子组件(字段、按钮)，道具如*手柄更改、手柄提交、手柄模糊、值、触摸、错误等。现在可供儿童使用*</formik>
3.  当孩子们与道具“互动”时，结果会传递给父母

在这一点上，我建议打开上面的代码沙箱，查看下面的代码

## 验证输入并提供有意义的反馈

在建立了 Formik 的基本框架后，逻辑上接下来的重点应该是建立输入验证，在我们的场景中，当客户键入无效的电子邮件或不够长的密码时，我们希望提供一些有意义的反馈。

组件带有一个验证模式属性——我们可以传入一种类型的对象，当特定字段更新时，会进行检查以确保值符合我们在模式中定义的规则。我们使用另一个名为[是的](https://github.com/jquense/yup)的库来定义我们的模式

**是的概述**

> Yup 是一个用于值解析和验证的 JavaScript 模式构建器。定义模式、转换匹配的值、验证现有值的形状，或者两者都进行。Yup 模式极具表现力，允许对复杂、相互依赖的验证或值转换进行建模。——[https://github.com/jquense/yup](https://github.com/jquense/yup)

本质上，让我们定义我们的验证模式，我们通过 validationSchema prop 将它传递给 Formik。然后，Formik 根据相应的 yup 验证检查字段值。

**Click the ‘Yup Validation’ tab in the UI pane and try type an invalid email address, Password hasn’t been wired up with validation, open the sandbox and try set up the validation as a small side activity.**

我们来分解一下上面的代码:

1.  我们使用*Yep*定义了一个通用的*validationSchema*——有许多不同的验证*Yep*开箱即用，我们挑选了一些基本的
    *注意:在 validation schema 中，我们需要* ***与命名*** *一致，如果我们的字段被称为“emailAddress ”,我们必须在下面的任何地方使用这个名称。*
2.  将已定义的模式应用到<formik>组件，这将告诉 Formik 我们想要运行的验证以及针对哪些字段。</formik>
3.  我们解构传递到子字段中的属性，为了使验证正常工作，我们需要来自 Formik 的 *handleChange(func)、handleBlur(func)、errors(obj)和 touched(obj)* 属性
4.  如果你使用的是反应引导表单，那么每个表单都很重要。Group 有一个 controlId(命名需要与*的* valIdationSchema 一致，这样 Formik 就知道这个字段需要验证什么)
    *注意:如果您没有使用 react-bootstrap，那么向您的输入传递一个 id，这与*的工作方式相同
5.  以我们的形式。“控制”或“输入组件”我们定义 *onChange* 和 *onBlur* ，分别传递 *handleChange* 和 *handleBlur* props。注意:在这一点上，我们将信息从子节点传递回父节点。

该流程的概述如下:用户对电子邮件字段进行更改， *onChange* 和 *onBlur* 将每个更改传递回< Formik >。
< Formik >消耗字段 *controlId* ，针对 out *yup* 模式运行验证，并更新其自身的*错误*和*用信息触动*道具。

React-Bootstrap 允许我们针对表单字段定义“is invalid”——我们可以对错误和接触的对象使用条件逻辑来确定某些内容是否无效:

```
//if emailAddress has been touched by the users and there are errors, if the errors are fixed or there are no errors, then the errors prop won't have our field.isInvalid = { touched.emailAddress && errors.emailAddress }
```

然后我们可以定义形式。control . Feedback(react-bootstrap)在输入无效时显示错误消息。

在这一点上，我建议打开上面的代码沙箱，查看 src/LoginForm/YupValidation.js 下的代码——使用 validationSchema 并尝试使密码字段的验证工作(下一节中的工作解决方案)

## 提交输入

Select the ‘OnSubmit’ Tab in the UI, type valid credentials and see it working

<formik>继续遵循相同的模式，通过提交，我们从父节点传递了一个 *handleSubmit* 函数作为道具，我们用登录按钮的 *onClick* 连接这个调用。</formik>

Formik 知道从子进程(使用其他道具，如 *handleChange* 和 *handleBlur)* 传递的值，因此将这些值传递给组件 *onSubmit* 函数。在这里，我们可以处理值，调度 redux 操作来调用 API 或将值存储在存储中，向客户显示反馈或其他任何内容。

## 结论

本文旨在帮助学习 Formik 附带的学习曲线，文章中提供了代码示例，可以免费复制或分支，不会产生任何后果。我真的希望在阅读完这篇关于 Formik 的文章后，不要感到害怕，并且清楚地知道如何在 ReactJS 或 React-Native 中设置表单。

如果有任何问题或者你认为有更好的方法，请留下评论让我知道！