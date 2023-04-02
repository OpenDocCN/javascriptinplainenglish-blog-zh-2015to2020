# 来自 2.5 年经验的 Vue.js 的 6 个要点提示

> 原文：<https://javascript.plainenglish.io/6-essentials-tips-for-vuejs-from-2-5-years-experience-1-b68dba2d2910?source=collection_archive---------15----------------------->

![](img/7150aaa77ce241d0e3c678c788997b3d.png)

你好。你好吗欢迎，我的名字是 Code Oz，我将与您分享一些关于 Vue.js 的技巧(我有 2.5 年使用该框架的经验)。

# 为了检查从父节点传递给子节点的值是否正确，请始终在您的 props 上使用 validator

```
 props: {
        toto: {
            type: String,
            required: true,
            validator: function (val) {
                return [ 'a', 'b' ].includes(val)
            }
        }
    },
```

如果验证器检测到错误，Vue 将触发 Vue 警告！

# 初始化时触发观察器

```
watch: {
    toto: (newValue, oldValue) => {
        // logic here ...
    }
}
```

⚠️这将在`toto`改变时被触发，但不会在初始化时被触发。

如果你想在初始化阶段触发你的观察器，你可以使用`immediate`属性！

```
watch: {
    toto: {
      immediate: true,
      handler(newValue, oldValue) {
        // logic here ...
      }
    }
}
```

处理程序是修改属性时触发的函数。

# 动态应用类和样式

```
<div :style="{ 'background': isActivated ? 'black' : 'white' }">
```

只有当值为 true 时，才可以应用类/样式！

```
// If isActivated is false, class will be not applied
<div :class="{ 'toto-class': isActivated }">
```

# 不要把 V-if 和 V-for 一起使用

绝不！为什么呢？

当你这样做的时候👇

```
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

当您在同一个节点中使用两者时，`v-for`的**优先级**比`v-if`高，因此`v-if`将在`v-for`的每次迭代中**被触发！**

为了避免这种情况，您可以用以下代码替换您的代码👇

```
<ul v-if="todos.length">
    <li v-for="todo in todos">
    {{ todo }}
    </li>
</ul>
```

但是如果您想将`v-if`用于`isComplete`属性，最好的方法是创建一个基于条件的计算。

```
computed: {
    todosNotCompleted() {
        return this.todos.filter(todo => !todo.isComplete)
    },
}<ul v-if="todos.length">
    <li v-for="todo in todosNotCompleted">
    {{ todo }}
    </li>
</ul>
```

# 你可以把所有道具从父母传给孩子

```
<child-component v-bind="$props"></child-component>
```

# v 型车

v-model 是一个在组件上创建双向数据绑定的指令！

```
<input v-model="message" placeholder="edit me">
```

这等于

```
<input :value="message" @input="message = $event.target.value" placeholder="edit me">
```

当您需要更新一个值或在该值改变时发出一个值时，请使用它作为速记！

我希望你喜欢这些建议！有基本的，我会分享更多关于 Vuejs(更先进)的技巧！

希望你喜欢这篇读物！

🎁你可以免费获得我的新书《javascript 中被低估的技能》,如果你在推特上关注我并给我打电话😁

或者在这里得到它

🎁[我的简讯](https://www.getrevue.co/profile/code__oz)

☕️你能支持我的作品吗🙏

🏃‍♂️，你可以跟着我👇

🕊 [推特](https://twitter.com/code__oz)

👨‍💻 [Github](https://github.com/Code-Oz)

你可以标记🔖这篇文章！

*更多内容请看*[***plain English . io***](http://plainenglish.io/)