# 在 Vue2 和 Vue3 中构建相同的组件

> 原文：<https://javascript.plainenglish.io/building-the-same-component-in-vue2-vs-vue3-ed47a09ade15?source=collection_archive---------1----------------------->

![](img/a84eee8a9c73a7a0b6c47ef56c2df984.png)

Photo by [Safar Safarov](https://unsplash.com/@codestorm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 随着 Vue3 的发布即将到来，许多人都在想“Vue2 和 Vue3 有什么不同？”

尽管我们以前写过关于即将到来的最大变化的文章，但我们还没有真正深入地研究过*到底是*我们的代码将如何变化。为了展示这些变化，我们将在 Vue2 和 Vue3 中构建一个简单的表单组件。

到本文结束时，您将理解 Vue2 和 Vue3 之间的主要编程差异，并开始成为一名更好的开发人员。

如果你想知道如何构建你的第一个 Vue3 应用，看看我们的初学者指南。

好吧——我们走！

## 创建我们的模板

对于大多数组件，Vue2 和 [Vue3](https://learnvue.co/2019/12/what-you-need-to-know-about-vue3-in-2020/) 中的代码即使不完全相同，也会非常*相似*。然而，Vue3 支持片段，这意味着组件可以有多个根节点。

这在呈现列表中的组件以移除不必要的包装 div 元素时特别有用。但是，在这种情况下，我们将为表单组件的两个版本保留一个根节点。

```
<template>
  <div class='form-element'>
    <h2> {{ title }} </h2>
    <input type='text' v-model='username' placeholder='Username' />

    <input type='password' v-model='password' placeholder='Password' />

    <button @click='login'>
      Submit
    </button>
    <p> 
      Values: {{ username + ' ' + password }}
    </p>
  </div>
</template>
```

唯一真正的区别是我们如何访问我们的数据。在 Vue3 中，我们的反应数据都包装在一个反应状态变量中——所以我们需要访问这个状态变量来获得我们的值。

```
<template>
  <div class='form-element'>
    <h2> {{ state.title }} </h2>
    <input type='text' v-model='state.username' placeholder='Username' />

    <input type='password' v-model='state.password' placeholder='Password' />

    <button @click='login'>
      Submit
    </button>
    <p> 
      Values: {{ state.username + ' ' + state.password }}
    </p>
  </div>
</template>
```

## 设置数据

**这就是主要区别所在 Vue2 选项 API 与 Vue3** [**组合 API。**](https://learnvue.co)

Options API 将我们的代码分成不同的属性:数据、计算属性、方法等。同时，组合 API 允许我们按照功能而不是属性的类型对代码进行分组。

对于我们的表单组件，假设我们只有两个数据属性:一个`username`和一个`password`。

Vue2 代码看起来像这样——我们只是在数据属性中添加了两个值。

```
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  }
}
```

在 [Vue 3.0](https://learnvue.co/2019/12/what-does-vuejs-3-0-mean-for-web-development/) 中，我们必须付出更多的努力，使用一个新的`setup()`方法，在那里我们所有的组件初始化都应该发生。

此外，为了给开发者更多的控制权，我们可以直接访问 Vue 的 reactivity API。

创建反应数据包括三个步骤:

1.  从 vue 导入`reactive`
2.  使用反应式方法声明我们的数据
3.  让我们的`setup`方法返回反应数据，这样我们的模板就可以访问它

在代码方面，它看起来会有点像这样。

```
import { reactive } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: ''
    })

    return { state }
  }
}
```

然后，在我们的模板中，我们像访问`state.username`和`state.password`一样访问它们

## 在 Vue2 和 Vue3 中创建方法

Vue2 选项 API 有一个单独的方法部分。在其中，我们可以定义我们所有的方法，并按照我们想要的方式组织它们。

```
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    login () {
      // login method
    }
  }
}
```

[Vue3 组合 API](https://learnvue.co) 中的 setup 方法也处理方法。它的工作方式有点类似于声明数据——我们必须首先声明我们的方法，然后**返回它**,以便组件的其他部分可以访问它。

```
export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: ''
    })

    const login = () => {
      // login method
    }
    return { 
      login,
      state
    }
  }
}
```

## 生命周期挂钩

在 Vue2 中，我们可以直接从组件选项中访问[生命周期挂钩](https://learnvue.co/2019/12/a-beginners-guide-to-vuejs-lifecycle-hooks/)。对于我们的例子，我们将等待安装的事件。

```
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  },
  mounted () {
    console.log('component mounted')
  },
  methods: {
    login () {
      // login method
    }
  }
}
```

现在有了 Vue3 组合 API，几乎所有东西都在 setup()方法中。这包括安装的生命周期挂钩。

然而，默认情况下不包含生命周期挂钩，所以我们必须导入在 Vue3 中调用的`onMounted`方法。这看起来和前面导入 reactive 一样。

然后，在我们的 setup 方法中，我们可以通过将 onMounted 方法传递给我们的函数来使用它。

```
import { reactive, onMounted } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    // ..

    onMounted(() => {
      console.log('component mounted')
    })

    // ...
  }
}
```

## 计算属性

让我们添加一个计算属性，将用户名转换成小写字母。

为了在 Vue2 中实现这一点，我们向 options 对象添加了一个计算字段。从这里，我们可以这样定义我们的属性…

```
export default {
  // .. 
  computed: {
    lowerCaseUsername () {
      return this.username.toLowerCase()
    }
  }
}
```

Vue3 的[设计允许开发者导入他们所使用的东西，并且在他们的项目中没有不必要的包。本质上，他们不希望开发人员必须包含他们从未使用过的东西，这在 Vue2 中已经成为一个日益严重的问题。](https://learnvue.co/2019/12/how-vue3-is-designed-for-both-hobby-devs-and-large-projects/)

所以要在 Vue3 中使用 computed 属性，我们首先必须将 computed 导入到我们的组件中。

然后，类似于我们之前创建反应数据的方式，我们可以将一段反应数据作为计算值，如下所示:

```
import { reactive, onMounted, computed } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: '',
      lowerCaseUsername: computed(() => state.username.toLowerCase())
    })

    // ...
  }
```

## 获取道具

访问道具带来了 Vue2 和 Vue3 之间的重要区别——`**this**`**意味着完全不同的东西。**

在 Vue2 中，`this`几乎总是指组件，而不是特定的属性。虽然这在表面上使事情变得容易，但它使类型支持变得痛苦。

然而，我们可以很容易地访问道具——让我们添加一个简单的例子，比如在挂载钩子时打印出我们的`title`道具:

```
mounted () {
    console.log('title: ' + this.title)
}
```

然而在 Vue3 中，我们不再使用`this`来访问道具、发出事件和获取属性。相反，`setup()`方法接受两个参数:

1.  `props` -不可变的访问组件的道具
2.  `context`-vu E3 显示的选定属性(发射、插槽、属性)

使用 props 参数，上面的代码如下所示。

```
setup (props) {
    // ...

    onMounted(() => {
      console.log('title: ' + props.title)
    })

    // ...
}
```

## 发射事件

类似地，Vue2 中的[发射事件](https://learnvue.co/2020/01/a-vue-event-handling-cheatsheet-the-essentials)非常简单，但是 Vue3 让您对如何访问属性/方法有更多的控制。

在我们的例子中，假设当“提交”按钮被按下时，我们希望向父组件发出一个登录事件。

Vue2 代码只需调用`this.$emit`并传入我们的有效载荷对象。

```
login () {
      this.$emit('login', {
        username: this.username,
        password: this.password
      })
 }
```

然而，在 Vue3 中，我们现在知道`this`不再表示同一件事，所以我们必须以不同的方式来做。

幸运的是，上下文对象公开了`emit`，这给了我们与`this.$emit`相同的东西

我们所要做的就是将`context`作为第二个参数添加到我们的设置方法中。我们将析构上下文对象，使我们的代码更加简洁。

然后，我们只需调用 emit 来发送我们的事件。然后，和以前一样，emit 方法接受两个参数:

1.  我们活动的名称
2.  与我们的事件一起传递的有效负载对象

```
setup (props, { emit }) {
    // ...

    const login = () => {
      emit('login', {
        username: state.username,
        password: state.password
      })
    }

    // ...
}
```

## 最终的 Vue2 vs. Vue3 代码！

太好了！我们已经熬到最后了。如你所见，在 Vue2 和 Vue3 中所有的概念都是一样的，但是我们访问属性的一些方式有了一点点的改变。

总的来说，我认为 Vue3 将帮助开发人员编写更有组织的代码——尤其是在大型代码库中。这主要是因为 Composition API 允许您按照特定的特性将代码组合在一起，甚至可以将功能提取到它们自己的文件中，并根据需要将它们导入到组件中。

这里的所有内容之后是 Vue2 中表单组件的代码。

```
<template>
  <div class='form-element'>
    <h2> {{ title }} </h2>
    <input type='text' v-model='username' placeholder='Username' />

    <input type='password' v-model='password' placeholder='Password' />

    <button @click='login'>
      Submit
    </button>
    <p> 
      Values: {{ username + ' ' + password }}
    </p>
  </div>
</template>
<script>
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  },
  mounted () {
    console.log('title: ' + this.title)
  },
  computed: {
    lowerCaseUsername () {
      return this.username.toLowerCase()
    }
  },
  methods: {
    login () {
      this.$emit('login', {
        username: this.username,
        password: this.password
      })
    }
  }
}
</script>
```

这是在 Vue3 中。

```
<template>
  <div class='form-element'>
    <h2> {{ state.title }} </h2>
    <input type='text' v-model='state.username' placeholder='Username' />

    <input type='password' v-model='state.password' placeholder='Password' />

    <button @click='login'>
      Submit
    </button>
    <p> 
      Values: {{ state.username + ' ' + state.password }}
    </p>
  </div>
</template>
<script>
import { reactive, onMounted, computed } from 'vue'

export default {
  props: {
    title: String
  },
  setup (props, { emit }) {
    const state = reactive({
      username: '',
      password: '',
      lowerCaseUsername: computed(() => state.username.toLowerCase())
    })

    onMounted(() => {
      console.log('title: ' + props.title)
    })

    const login = () => {
      emit('login', {
        username: state.username,
        password: state.password
      })
    }

    return { 
      login,
      state
    }
  }
}
</script>
```

我希望这篇教程有助于强调 Vue 代码在 Vue3 中的一些不同之处。如果还有其他问题，就留言回复吧！

编码快乐！

[如果你有兴趣了解更多关于 Vue 3 的知识，请下载我的免费 Vue 3 备忘单，里面有一些基本知识，比如组合 API、Vue 3 模板语法和事件处理。](https://learnvue.co/vue-3-essentials-cheatsheet/)