# 将 Yup 与 Redux-form 一起使用

> 原文：<https://javascript.plainenglish.io/use-yup-with-redux-form-684d6c23ab41?source=collection_archive---------1----------------------->

![](img/d1c460585aca3246a3055d54546c614e.png)

Photo by [Daniel Frank](https://unsplash.com/@fr3nks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我想分别用`[Yup](https://github.com/jquense/yup)`和`[Formik](https://github.com/jaredpalmer/formik)`替换`Joi`和`Redux-Form`。“现在你为什么要这么做！?"，你可能会问。好吧，让我们快速地过一遍，是和福米克的原因。

## 为什么是的

1.  比 Joi 轻得多的。

这就是“轻量级”如此重要的原因:

> **TL；dr:代码少=解析/编译少+传输少+解压少** [*来源*](https://medium.com/dev-channel/the-cost-of-javascript-84009f51e99e)

2.更容易从返回的错误对象中解析错误消息。

3.非常灵活地定制错误消息，没有字符串操作诡计。

4.Yup 与 Joi 共享非常相似的语法和方法名，使得替换 Joi 成为一件容易的事情。

## 为什么是福米克

见[为什么不是 Redux-Form？](https://github.com/jaredpalmer/formik#why-not-redux-form)

然而，对我来说，用 Formik 替换 redux-form 比用 Yup 替换 Joi 要困难得多，因此这里有一篇关于它的“友好的”中型文章——让 Yup 很好地使用 redux-form 的要点。

首先，我们创建一个验证器函数，稍后它将接受我们的`Yup`模式。

我遇到的主要问题对我来说并不明显——直到我阅读了 [redux-form-yup](https://github.com/patwritescode/redux-form-yup/) 包的[代码](https://github.com/patwritescode/redux-form-yup/blob/master/src/asyncValidate.ts)——是`Line-3`上的那个`abortEarly: false`对象。其默认值为`true`，这意味着:

> `*abortEarly*`:在第一个错误时从验证方法返回，而不是在所有验证运行之后。

因此，如果您希望在第一次呈现时验证所有字段的行为，您应该将它设置为`false`，在我看来，这将产生一个好的 UX，例如，如果用户做的第一件事是单击“提交”按钮，他们将会看到所有字段的错误消息。

最后，我们将在 redux-form 组件中使用上面的验证函数！：

这就是我在这方面的全部内容！谢谢！

![](img/762c192a0a9a74b8ae1766233c9a88e0.png)

Photo by [Yong Chuan](https://unsplash.com/@yongchuannnnn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)