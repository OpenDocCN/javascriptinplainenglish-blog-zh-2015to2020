# VueJs 表单验证，第 2 部分— Vuelidate

> 原文：<https://javascript.plainenglish.io/vuejs-form-validation-part-2-vuelidate-54807f1bf940?source=collection_archive---------1----------------------->

![](img/474ec057b31a8880bd66b38a8c102319.png)

Vuelidate Form Validation

在我的上一篇文章中，我介绍了 VueJs 验证包 [VeeValidate](https://logaretm.github.io/vee-validate/) 。在第 2 部分中，我们将讨论另一个流行的验证包，叫做 [Vuelidate](https://vuelidate.js.org/) 。VeeValidate 采用基于模板的方法，而 Vuelidate 采用基于模型的方法正好相反。

就像上一篇文章一样，我们将看看如何在注册表单上使用这个包。这会给你一个清晰的概念，告诉你如何在任何形式下使用它。那么，让我们开始编码吧💻！

## 装置

要安装 VeeValidate，请在 Vue 项目的根目录下打开一个终端，并运行以下命令:

```
npm install vuelidate --save
```

接下来，我们将 Vuelidate 作为插件添加到 Vue 中。在 src/main.js 中添加以下内容:

```
import Vuelidate from 'vuelidate'
Vue.use(Vuelidate)
```

## 表单验证

验证字段的方式基于您的模型。export default 中的 data 属性包含输入的模型(v-model)。通过导入 Vuelidate 包，您现在将有一个新的属性可以添加到导出缺省值中，称为“validations”。

你需要做的就是从 Vuelidate ( [可用规则](https://vuelidate.js.org/#sub-builtin-validators))中导入你需要的规则。

```
import { required, minLength } from "vuelidate/lib/validators";
```

然后，向“validations”添加一个属性，该属性与您的模型属性具有完全相同的名称。这将把您的验证绑定到您的模型。

```
*data*: () => ({
 **// *Your Model***name: ""}),
**//*Your model validations***validations: {
 name: {
  required,
  minLength: *minLength*(4)
 }
}
```

接下来，您需要用您的输入和错误在模板中构建表单。

```
<template>
 <div>
  <h2>Vuelidate Form</h2>
  <form @*submit*.*prevent*="submitForm">
   <input *type*="text" *v-model*="name" />
  ** <!-- *display errors* -->**
   <div 
    *class*="error" 
    *v-if*="!*$v.name.*required && *$v.name.*$dirty">
     Field is required
   </div> <div
    *class*="error"
    *v-if*="!*$v.name.*minLength && *$v.name.*$dirty"
   >
    Name must have at least {{*$v.name.$params.minLength.*min}}
    letters.
   </div>
   <input *type*="submit" />
  </form>
 </div>
</template>
```

最后，我们将创建提交表单的方法。在 export default 中，我们将添加一个包含表单提交方法的 methods 属性。

```
methods: {
 **//*Form submission handler*** *submitForm*() {
  **//*fire validations on all fields***this*.$v.$touch*();

  **//*check to see if form is valid before continuing***if (!this*.$v.*$invalid) {
   console*.*log(this*.*name);
  }
 }
}
```

# 注册表单验证

现在让我们看一个真实世界的例子。我们将创建一个包含以下字段的注册表单:

*   名称—必需且唯一的字母(字母)
*   电子邮件—必需的有效电子邮件，
*   密码—必需，最小长度为 6，最大长度为 8
*   性别—必填
*   服务接受条款—必填

对于造型，我将使用引导。

# 自定义验证程序

有时您需要一个 Vuelidate 没有提供的验证器。在这种情况下，您可以编写自己的自定义验证器，如下所示:

```
const mustBeCool = (value) => value.indexOf('cool') >= 0// ...

validations: {
  myField: {
    mustBeCool
  }
}
```

如果你想把他们的验证器和你自己的逻辑结合起来，你可以这样写:

```
import { helpers } from 'vuelidate/lib/validators'
const mustBeCool = (value) => !helpers.req(value) || value.indexOf('cool') >= 0

// ...

validations: {
  myField: {
    mustBeCool
  }
}
```

# 视频教程

Video Tutorial

# 结论

如您所见，Vuelidate 是一个健壮的验证包，您可以在表单上使用它。它允许您轻松快速地添加验证，只需很少甚至不需要验证代码。希望这篇文章已经给了你一个很好的想法，你可以实现什么。下次见，编码快乐！