# Vuelidate —错误和列表验证

> 原文：<https://javascript.plainenglish.io/vuelidate-errors-and-lists-validation-2439f50a9dac?source=collection_archive---------5----------------------->

![](img/b038549001cc69a4b92d35d4da05bae7.png)

Photo by [Emma Matthews Digital Content Production](https://unsplash.com/@emmamatthews?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

默认情况下，Vue.js 没有任何表单验证功能。

因此，我们需要添加自己的表单验证库或自己的代码。

在本文中，我们将了解如何用 Vuelidate 验证表单。

# $error vs $anyError

我们可以用`$error`和`$anyError`验证属性得到表单验证错误。

`$dirty`和`$anyDirty`检查表格是否被修改。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <div :class="{ 'form-group--error': $v.name.$error }">
        <label>name</label>
        <input v-model.trim="$v.name.$model">
        <div class="error" v-if="!$v.name.required">name is required.</div>
      </div><div :class="{ 'form-group--error': $v.age.$error }">
        <label>age</label>
        <input v-model.trim="$v.age.$model">
      </div>
      <div class="error" v-if="!$v.age.required">age is required.</div>
    </div>
  </div>
</template>
<script>
import { required } from "vuelidate/lib/validators";
export default {
  name: "App",
  data() {
    return {
      name: "",
      age: ""
    };
  },
  validations: {
    name: {
      required
    },
    age: {
      required
    }
  }
};
</script>
```

然后我们给出了`$v.age.$error`和`$v.name.$error`属性来得到误差。

如果它们是`true`，那么`form-group--error`类将被应用。

# 验证组

我们可以创建验证组来一起验证所有的表单域。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <div :class="{ 'form-group--error': $v.name.$error }">
        <label>name</label>
        <input v-model.trim="$v.name.$model">
        <div class="error" v-if="!$v.name.required">name is required.</div>
      </div> <div :class="{ 'form-group--error': $v.age.$error }">
        <label>age</label>
        <input v-model.trim="$v.age.$model">
        <div class="error" v-if="!$v.age.required">age is required.</div>
      </div> <div :class="{ 'form-group--error': $v.phone.mobile.$error }">
        <label>mobile phone</label>
        <input v-model.trim="$v.phone.mobile.$model">
        <div class="error" v-if="!$v.phone.mobile.required">mobile phone is required.</div>
      </div><div v-if="$v.validationGroup.$error">group is invalid.</div>
    </div>
  </div>
</template>
<script>
import { required } from "vuelidate/lib/validators";
export default {
  name: "App",
  data() {
    return {
      name: "",
      age: "",
      phone: {
        mobile: ""
      }
    };
  },
  validations: {
    name: {
      required
    },
    age: {
      required
    },
    phone: {
      mobile: {
        required
      }
    },
    validationGroup: ["name", "age", "phone.mobile"]
  }
};
</script>
```

我们在`validations`属性中添加了`validationGroup`属性来添加一个验证组。

它有一个包含属性名称的字符串数组。

如果属性是嵌套的，则属性由点分隔。

然后在模板中，我们获取`$v.validationGroup.$error`属性来查看组中的字段是否有错误。

# 集合验证

我们可以用关键字`$each`验证集合。

例如，我们可以写:

```
<template>
  <div id="app">
    <div v-for="(v, index) in $v.people.$each.$iter">
      <div class="form-group" :class="{ 'form-group--error': v.$error }">
        <label>Name</label>
        <input v-model.trim="v.name.$model">
      </div>
      <div v-if="!v.name.required">Name is required.</div>
      <div
        v-if="!v.name.minLength"
      >Name must have at least {{ v.name.$params.minLength.min }} letters.</div>
    </div>
    <div>
      <button class="button" [@click](http://twitter.com/click)="people.push({name: ''})">Add</button>
      <button class="button" [@click](http://twitter.com/click)="people.pop()">Remove</button>
    </div>
    <div class="form-group" :class="{ 'form-group--error': $v.people.$error }"></div>
    <div
      v-if="!$v.people.minLength"
    >List must have at least {{ $v.people.$params.minLength.min }} elements.</div>
    <div v-else-if="!$v.people.required">List must not be empty.</div>
    <div v-else-if="$v.people.$error">List is invalid.</div>
    <button class="button" [@click](http://twitter.com/click)="$v.people.$touch">$touch</button>
    <button class="button" [@click](http://twitter.com/click)="$v.people.$reset">$reset</button>
  </div>
</template>
<script>
import { required, minLength } from "vuelidate/lib/validators";
export default {
  name: "App",
  data() {
    return {
      people: [
        {
          name: "mary"
        },
        {
          name: ""
        }
      ]
    };
  },
  validations: {
    people: {
      required,
      minLength: minLength(3),
      $each: {
        name: {
          required,
          minLength: minLength(2)
        }
      }
    }
  }
};
</script>
```

在`validations`属性中，我们用`$each`属性和`minLength`属性来设置`people`数组的最小长度。

此外，我们在`name`字段中有`minLength`属性来检查输入的名称是否具有所需的长度。

在模板中，我们有`$touch`和`$reset`方法，可以分别用来触发验证和重置验证状态。

# 结论

我们可以使用 Vuelidate 验证列表并检查错误。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**