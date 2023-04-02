# 将 Formik 与 React Native 一起使用

> 原文：<https://javascript.plainenglish.io/using-formik-with-react-native-c2b5820cc718?source=collection_archive---------3----------------------->

## 使用 Formik 和 Yup 轻松地将表单集成到 React 本机应用程序中

为 web 或移动应用程序开发人员创建表单几乎是不可能的。对于 ReactJS 开发者来说，Formik 是天赐之物。这是一个为使 web 应用程序更容易处理表单而开发的项目。然而，用 React native 编写的移动应用程序似乎没有充分利用这个库。这篇简短的帖子演示了如何使用 React 本地应用程序以及使用 [Yup](https://github.com/jquense/yup) 添加验证。

![](img/8944fecb4ffb402aeb90e6feccc61814.png)

source: pexels

首先使用 yarn 或 npm 将 Formik 和 Yup 添加到项目中。

`yarn add formik yup`

接下来，我们将使用上面添加的项目来创建一个表单。从导入 Formik 和 Yup 开始，如下所示

```
import * as yup from "yup";import { Formik } from "formik";
```

让我们从编写验证开始。这些验证稍后将被插入到表单中。

```
let validationSchema = yup.object().shape({
  issueDescription: yup.string().min(20).required(),
  email: yup.string().email().required(),
});
```

验证架构概述了表单可以接受的值的类型及其各自的约束。上面的变量`validationSchema`为 issueDescription 和 email 字段创建验证准则。问题描述不得少于 20 个字符，并且不能为空。同样，电子邮件值必须是一个字符串和有效的电子邮件。该字段也可以不为空。

像输入字段和复选框这样的表单元素被包装在 Formik 标签之间。

```
<Formik
***initialValues***={{ issueDescription: '', email: ''}}
***onSubmit***={*values* => handleForm(values)}
***validationSchema***={validationSchema}>{({ *values*, *handleChange*, *errors*, *handleSubmit* }) => (....)}
</Formik>
```

在上面的代码片段中，粗斜体文本表示 Formik 使用的属性。`initialValues`包含一个带有字段及其初始值的对象。这些值在第一次出现时被注入到表单域中。该对象的键应该与表单元素的名称相匹配。例如，在我们将要创建的表单中应该有一个名为`issueDescription`的输入字段。

`onSubmit`接受一个处理程序，这个处理程序概括了表单提交时应该做的事情。`validationSchema`描述了决定表单元素中的值是有效还是无效的规则。这就是 Yup 的用武之地。

```
{({ *values*, *handleChange*, *errors*, *handleSubmit* })
```

上面一行显示了表单组件可用的一组值。它们被用来显示验证错误，记录输入字段中的任何变化，识别字段何时成为焦点，等等。这些值作为表单的参数，作为缺失的部分，如下所示。

```
<TextInput
*name="issueDescription"
placeholder*="Describe the issue"
*style*={styles.inputs}
*value*={***values***.issueDescription}
*onChangeText*={**handleChange**('issueDescription')}
/>{(**errors**.issueDescription) && <Text>Invalid issue description</Text>}
```

上面代码片段中加粗的部分是作为参数传递给表单的值。如果描述无效，它的最后一部分有条件地呈现一个错误。

最后，可以使用一个按钮来提交表单。该按钮大致如下所示

```
 <Button 
title="Report an issue"
onPress={**handleSubmit**}
/>
```

为了简单起见，省略了可以根据表单状态禁用按钮的部分。这可以通过使用此处的导轨[轻松完成。](https://formik.org/docs/guides/validation)

希望这能让构建表单对你来说更容易一些。