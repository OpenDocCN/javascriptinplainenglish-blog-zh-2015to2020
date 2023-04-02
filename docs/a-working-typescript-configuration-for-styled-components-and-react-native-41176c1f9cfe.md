# 用于样式化组件和 React Native 的工作类型脚本配置

> 原文：<https://javascript.plainenglish.io/a-working-typescript-configuration-for-styled-components-and-react-native-41176c1f9cfe?source=collection_archive---------4----------------------->

![](img/47ab613c7349c6826de8908ccd4044ed.png)

> 使用 typescript 的好处是，它允许我们在 IDE 中进行类型检查和自动完成，以便获得更可靠的编程和开发体验。

在我们的程序中使用定义文件的语法并有正确的完成并不总是容易的。

# 下面是一个您可以使用的工作配置示例

此配置将允许您:

*   在组件中对主题进行自动完成和类型检查。
*   对样式组件的类型有基本的了解。

> 我们需要什么？

一切都可以包含在一个名为`theme`的文件中，该文件将用于公开一个包含我们主题的对象。如果你愿意，你可以把它分成几个文件。

我们使用[类型脚本接口合并](https://www.typescriptlang.org/docs/handbook/declaration-merging.html)来覆盖默认主题。

在你的应用程序顶层组件的某个地方，你可以将`ThemeProvider`和你导出的`theme`常量一起使用，如上所述。

你现在可以访问你的主题变量和自动完成，如这里声明的:[https://www.styled-components.com/docs/advanced](https://www.styled-components.com/docs/advanced)

**👋🏻对于这篇文章的法文版，🇫🇷:让我们看看我的博客:**[https://modern-JavaScript . fr/une-configuration-pour-styled-components-avec-typescript-et-react-native/](https://modern-javascript.fr/une-configuration-pour-styled-components-avec-typescript-et-react-native/)