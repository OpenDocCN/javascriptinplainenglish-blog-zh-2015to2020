# 常见的 Vue 问题——作用域 CSS、计算属性和观察器等等

> 原文：<https://javascript.plainenglish.io/common-vue-problems-scoped-css-computed-properties-and-watchers-and-more-e1f700c75a62?source=collection_archive---------6----------------------->

![](img/ea191f608e4dbfbe798a05b08895d5aa.png)

Photo by [Agê Barros](https://unsplash.com/@agebarros?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 范围引导 CSS

我们可以通过将引导 CSS 和`scoped`属性一起包含在`style`标签的深度选择器中来确定它们的范围。

例如，我们可以写:

```
<style scoped lang="scss">
.main-wrapper /deep/ {
  @import "~bootstrap/dist/css/bootstrap.min";
}
</style>
```

我们将作用域 CSS 放在一个包装器中，这样作用域 CSS 样式就不会传播到父类。

这是因为它被限制在包含类`main-wrapper`及其子类的元素中。

# 构建相对路径

如果资产位于与托管资产的域不同的域中，那么我们需要使用绝对 URL 而不是相对 URL。

# Vue.extend 的目的

`Vue.extend`用于创建`Vue`的子类，并返回我们添加的额外内容的构造函数。

`Vue.component`用于关联带有字符串 ID 的给定构造函数，这样 Vue 可以在模板中选择它们。

这意味着我们可以在模板中添加任何内容。

我们可以用`Vue.directive`和`Vue.filter`做同样的事情。

`Vue.component`用于创建和注册新组件。

`Vue.directive`用于创建和注册新指令。

`Vue.filter`用于创建和注册一个新的过滤器。

# 计算属性和观察器之间的差异

计算属性用于创建从另一个属性派生的属性。

依赖项由反应性属性组成，如数据属性和其他计算属性。

计算的属性在 getter 中不能有参数，但是在 setter 中可以有新值作为参数。

例如，我们可以写:

```
computed: {
  totalPrice() {
    return this.price * this.quantity;
  }
}
```

此外，我们还可以在计算属性中使用 getters 和 setters:

```
// ...
computed: {
  fullName: {
    get() {
      return `${this.firstName} ${this.lastName}`;
    },

    set(newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

我们有一个用`fullName`的`get`方法编写的 getter。

我们有一个使用`fullName`的`set`方法的 setter。

参数是`newValue`，它有我们想要设置为`fullName`新值的值。

观察器的不同之处在于，我们可以用它来观察一个反应性属性，并基于它执行一个动作。

它可以用来产生副作用。

例如，我们可以写:

```
watch: {
  somethingSelected() {
    this.router.push('someOtherRoute')
  }
}
```

我们也可以像上面所做的那样，在事情发生变化时使用观察器来调用方法。

此外，我们可以监听嵌套属性，将`deep`属性设置为`true`。

# 将数字输入转换为数值

我们可以使用`number`修饰符将输入的值自动转换成数字。

例如，我们可以写:

```
<input v-model.number="value" type="number">
```

那么`value`将是一个数字而不是一个字符串，因为在`v-model`指令后有`number`修饰符。

# 在车身上安装 Vue 应用程序

我们不应该在 body 元素上安装 Vue 实例，

如果我们这样做，Vue 会给我们一个错误。

# 引导模式隐藏事件

我们可以添加一个对模态的引用，然后用它来监听`hidden.bs.modal`事件。

例如，我们可以写:

```
new Vue({
  el:"#app",
  data:{
  },
  methods:{
    onHidden(){
      //do something
    }
  },
  mounted(){
    $(this.$refs.vuemodal).on("hidden.bs.modal", this.onHidden)
  }
})
```

我们用`$(this.$refs.vuemodal`获得引导模式的 DOM 元素。

然后，我们可以从 jQuery 调用`on`方法来监听`hidden.nbs.modal`事件，以运行`this.onHidden`方法来执行模态关闭操作。

# 未捕获的引用错误:在 index.html 没有定义 Vue

如果我们没有定义 Vue，那么我们可能忘记在 HTML 文件中包含 Vue。

我们应该通过编写以下内容来确保我们有一个 Vue 的脚本标签:

```
<script src="https://unpkg.com/vue@2.4.2"></script>
```

然后我们可以在这个加载之后使用 Vue，这意味着 Vue 代码应该在这个脚本标签之后。

![](img/b6525f69a65dc639ee63106f5818fc50.png)

Photo by [Handy Wicaksono](https://unsplash.com/@handywicaksono?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

如果我们将引导 CSS 包含在选择器中，

要将输入的值自动转换成数字，我们可以使用`v-model`上的`number`修饰符。

计算属性和观察器之间有很大的区别。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**