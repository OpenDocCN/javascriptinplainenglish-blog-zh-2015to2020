# 验证—表单验证

> 原文：<https://javascript.plainenglish.io/vuetify-form-validation-918d2d91409b?source=collection_archive---------2----------------------->

![](img/2b2d3a59306aed7bde9d150bcf86766b.png)

Photo by [Jonny Swales](https://unsplash.com/@jonnyswales1989?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 通过提交和清除进行验证

我们可以用`this.$refs.form.reset()`方法重置一个表单。

我们可以用`this.$refs.form.resetValidation()`方法重置表单验证。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field v-model="name" :counter="10" :rules="nameRules" label="Name" required></v-text-field> <v-text-field v-model="email" :rules="emailRules" label="E-mail" required></v-text-field> <v-select
            v-model="select"
            :items="items"
            :rules="[v => !!v || 'Item is required']"
            label="Item"
            required
          ></v-select> <v-checkbox
            v-model="checkbox"
            :rules="[v => !!v || 'You must agree']"
            label="Do you agree?"
            required
          ></v-checkbox> <v-btn :disabled="!valid" color="success" class="mr-4" @click="validate">Validate</v-btn>
          <v-btn color="error" class="mr-4" @click="reset">Reset Form</v-btn>
          <v-btn color="warning" @click="resetValidation">Reset Validation</v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    valid: true,
    name: "",
    nameRules: [
      (v) => !!v || "Name is required",
      (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
    ],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    select: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
    checkbox: false,
  }), methods: {
    validate() {
      this.$refs.form.validate();
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
  },
};
</script>
```

我们在每个输入组件上都有带有规则的`rules`道具。

此外，我们有名称和电子邮件的规则。

这些方法让我们可以重置应用程序的值和验证。

`validate`方法让我们验证表单字段。

我们有`reset`和`resetValidation`方法来重置表单。

# Vuelidate

我们可以通过使用 Vuelidate 库将表单验证合并到我们的 Vuetify 表单中。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <form>
          <v-text-field
            v-model="name"
            :error-messages="nameErrors"
            :counter="10"
            label="Name"
            required
            [@input](http://twitter.com/input)="$v.name.$touch()"
            [@blur](http://twitter.com/blur)="$v.name.$touch()"
          ></v-text-field>
          <v-text-field
            v-model="email"
            :error-messages="emailErrors"
            label="E-mail"
            required
            [@input](http://twitter.com/input)="$v.email.$touch()"
            [@blur](http://twitter.com/blur)="$v.email.$touch()"
          ></v-text-field>
          <v-select
            v-model="select"
            :items="items"
            :error-messages="selectErrors"
            label="Item"
            required
            [@change](http://twitter.com/change)="$v.select.$touch()"
            [@blur](http://twitter.com/blur)="$v.select.$touch()"
          ></v-select>
          <v-checkbox
            v-model="checkbox"
            :error-messages="checkboxErrors"
            label="Do you agree?"
            required
            [@change](http://twitter.com/change)="$v.checkbox.$touch()"
            [@blur](http://twitter.com/blur)="$v.checkbox.$touch()"
          ></v-checkbox><v-btn class="mr-4" [@click](http://twitter.com/click)="submit">submit</v-btn>
          <v-btn [@click](http://twitter.com/click)="clear">clear</v-btn>
        </form>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import { validationMixin } from "vuelidate";
import { required, maxLength, email } from "vuelidate/lib/validators";export default {
  name: "HelloWorld",
  mixins: [validationMixin],validations: {
    name: { required, maxLength: maxLength(10) },
    email: { required, email },
    select: { required },
    checkbox: {
      checked(val) {
        return val;
      },
    },
  },data: () => ({
    name: "",
    email: "",
    select: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
    checkbox: false,
  }),computed: {
    checkboxErrors() {
      const errors = [];
      if (!this.$v.checkbox.$dirty) return errors;
      !this.$v.checkbox.checked && errors.push("You must agree");
      return errors;
    },
    selectErrors() {
      const errors = [];
      if (!this.$v.select.$dirty) return errors;
      !this.$v.select.required && errors.push("Item is required");
      return errors;
    },
    nameErrors() {
      const errors = [];
      if (!this.$v.name.$dirty) return errors;
      !this.$v.name.maxLength &&
        errors.push("Name must be at most 10 characters long");
      !this.$v.name.required && errors.push("Name is required.");
      return errors;
    },
    emailErrors() {
      const errors = [];
      if (!this.$v.email.$dirty) return errors;
      !this.$v.email.email && errors.push("Must be valid e-mail");
      !this.$v.email.required && errors.push("E-mail is required");
      return errors;
    },
  }, methods: {
    submit() {
      this.$v.$touch();
    },
    clear() {
      this.$v.$reset();
      this.name = "";
      this.email = "";
      this.select = null;
      this.checkbox = false;
    },
  },
};
</script>
```

添加表单域。

我们合并了 Vuelidate 提供的`validationMixin`。

我们添加了带有关键字`name`、`email`和`select`的`validations`对象，它们有规则。

此外，我们有每个字段的计算属性和计算错误消息。

我们从`$v`对象中获取字段，并在`validations`属性中获取键。

返回的错误信息可通过每个字段上的`error-message`属性进行设置。

在`methods`对象中，我们有`reset`方法来清除验证。

这些项目也会被重置。

在`submit`方法中，我们有`$touch`方法来触发验证。

![](img/33c4a803029a98852d44f9e85355480d.png)

Photo by [Heather Lo](https://unsplash.com/@llhheather?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加带有 Vuetify 和 Vuelidate 验证的表单。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**