# Vee-Validate —内置验证规则

> 原文：<https://javascript.plainenglish.io/vee-validate-built-in-validation-rules-eeebedc9741d?source=collection_archive---------3----------------------->

![](img/d18b6449b580c8634f7bd983cd2ed533.png)

Photo by [Johann Siemens](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

表单验证是 Vue.js 没有内置的功能。

但是，我们还是非常需要这个功能。

在本文中，我们将了解如何使用带参数的内置验证规则。

# 规则

许多规则都有一个或多个参数。

# 数字

`digits`可以如下使用:

```
<ValidationProvider rules="digits:10" v-slot="{ errors }">
  <input v-model="value" type="text">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

其中 10 是允许的位数

# 规模

`dimensions`检查所选图像文件的尺寸:

```
<ValidationProvider rules="dimensions:220,230" v-slot="{ errors, validate }">
  <input type="file" @change="validate">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

220 是宽度，230 是高度。

# 排除

`excluded`检查列表中没有的内容:

```
<ValidationProvider rules="excluded:1,2" name="number" v-slot="{ errors }">
  <select v-model="value">
    <option value="1">one</option>
    <option value="2">two</option>
    <option value="3">three</option>
  </select>
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

1 和 2 是`excluded`的参数，所以无效。

# 外面的（exterior 的简写）

`ext`检查选定的文件是否有指定的扩展名:

```
<ValidationProvider rules="ext:jpg,png" v-slot="{ errors, validate }">
  <input type="file" @change="validate">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

所以`jpg`和`png`是有效的文件扩展名。

# 不是

`is_not`带参数排除。

例如，我们可以写:

```
<ValidationProvider rules="is_not:foo" v-slot="{ errors }">
  <input type="text" v-model="value">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

`foo`是输入时无效的值。

# 长度

`length`表示字符串必须是具有给定长度的字符串或数组值。

因此，如果我们有:

```
<ValidationProvider rules="length:5" v-slot="{ errors }">
  <input type="text" v-model="value">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

那么输入值的长度必须为 5。

# 最大

`max`检查输入值是否没有超过规定长度。

例如，我们可以写:

```
<ValidationProvider rules="max:5" v-slot="{ errors }">
  <input type="text" v-model="value">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

我们将 5 作为参数，所以输入的值不能超过 5 的长度。

# 最大值

`max_value`检查某物是否不能超过最大值。

我们可以写:

```
<ValidationProvider rules="max_value:5" v-slot="{ errors }">
  <input type="text" v-model="value">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

# 哑剧

`mimes`检查所选文件是否具有给定的 mime 类型。

例如，我们可以写:

```
<ValidationProvider rules="mimes:image/*" v-slot="{ errors, validate }">
  <input type="file" @change="validate">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

这将检查文件是一个图像。

# 部

`min`是验证长度不应小于指定长度。

如果我们有:

```
<ValidationProvider rules="min:5" v-slot="{ errors }">
  <input type="text" v-model="value">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

则输入值的长度必须为 5 或更长。

# 最小值

`min_value`检查数值是否大于或等于给定的参数。

例如，如果我们有:

```
<ValidationProvider rules="min_value:5" v-slot="{ errors }">
  <input type="text" v-model="value">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

那么`value`必须大于等于 5。

# 之一

`oneOf`使用一个或多个参数来检查所选值是否是列出的值之一。

如果我们有:

```
<ValidationProvider rules="oneOf:1,2,3" name="number" v-slot="{ errors }">
  <select v-model="value">
    <option value="1">one</option>
    <option value="2">two</option>
    <option value="3">three</option>
    <option value="4">hour</option>
  </select>
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

我们有 1、2 和 3 作为参数，所以它们是唯一的有效值。

# 正则表达式

`regex`让我们匹配一个与给定正则表达式匹配的值。

如果我们有:

```
<ValidationProvider :rules="{ regex: /^[0-9]+$/ }" v-slot="{ errors }">
  <input type="text" v-model="value">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

然后我们匹配正则表达式`/^[0–9]+$/`，它是一个带数字的字符串。

# 需要

`required`字段可以接受一个具有`allowFalse`属性的对象，如果我们希望允许`false`作为有效值，那么这个属性就是`true`。

例如，当我们使用`required`规则时，为了允许`false`作为有效值，我们编写:

```
<ValidationProvider :rules="{ required: { allowFalse: false } }" v-slot="{ errors }">
  <!-- ... -->
</ValidationProvider>
```

# 必需 _ 如果

`required_if`让我们检查如果一个字段有给定值，另一个字段是否是必需的。

例如，我们可以这样使用它:

```
<ValidationProvider rules="" vid="country" v-slot="{ errors }">
  <select v-model="country">
    <option value="Canada">Canada</option>
    <option value="other">Other country</option>
  </select>
</ValidationProvider><ValidationProvider rules="required_if:country,Canada" v-slot="{ errors }">
  <input type="text" placeholder="province" v-model="province" />
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

我们在第一个字段上有`vid`,这样我们可以在下面的字段中引用它。

`required_if`引用了`vid`值。

第二个参数是值。

# 大小

`size`用于检查文件是否超过给定的千字节大小。

我们可以写:

```
<ValidationProvider rules="size:100" v-slot="{ errors, validate }">
  <input type="file" @change="validate">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

`validate`用于检查文件选择改变时的值。

![](img/6e357c805c77cb12519504678b57aa82.png)

Photo by [DAVID ZHOU](https://unsplash.com/@gzzhouming1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有许多内置的验证规则。

他们中的一些人会争论。有检查文件大小和输入值的规则。

## **简单英语的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**