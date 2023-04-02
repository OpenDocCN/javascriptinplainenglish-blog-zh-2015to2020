# 使用 Vue 保活组件保存动态组件的状态

> 原文：<https://javascript.plainenglish.io/using-vue-keep-alive-component-to-save-the-state-of-dynamic-components-e52144e2260e?source=collection_archive---------4----------------------->

![](img/b21877dc60066a1b004c638fa3458f2f.png)

Photo by [James Lee](https://unsplash.com/@picsbyjameslee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究如何在用户在动态组件之间导航时保存状态，以避免用户受挫。

# 保活组件

当用户使用默认配置在动态组件之间导航时，用户在选项卡之间导航时会丢失数据。

这是因为当我们转换动态组件时，当我们呈现组件的实例、移除当前组件并重新呈现该组件的新实例时，会添加和移除组件。

keep-alive 组件是一个包装器组件，即使当用户离开该组件时，它也能保持旧实例的活动状态。

这是一种叫做抽象元素的成分。它不呈现 DOM 元素，也不显示为组件。

我们可以使用 keep-alive 组件来做任何需要我们在屏幕上保留现有数据的事情。

例如，我们可以用它来缓存表单中的用户输入或缓存 API 调用结果。

keep-alive 组件还将组件存储在缓存中，因此渲染速度更快。我们应该只在渲染动态组件时使用它，因为我们不想缓存所有的东西。

否则，用户可能会看到他们不希望看到的旧值。

# 保持活动组件挂钩

保持活动的组件具有自定义挂钩。他们是`activated`和`deactivated`。

当一个保活组件被激活时，调用`activated`钩子，当保活组件被停用时，调用`deactivated`钩子。

# 简单的例子

我们使用 Vue 内置的`keep-alive`组件来进行缓存。

例如，我们可以创建一些组件，并使用`keep-alive`中的`component`组件动态地切换 keep 组件，同时保持它们被缓存。

为此，我们可以编写以下代码:

`components/EmployeeForm.vue`

```
<template>
  <div>
    <input v-model="name" placeholder="employee">
  </div>
</template><script>
export default {
  name: "EmployeeForm",
  data() {
    return {
      name: ""
    };
  }
};
</script>
```

`components/StudentForm.vue`

```
<template>
  <div>
    <input v-model="name" placeholder="student">
  </div>
</template><script>
export default {
  name: "StudentForm",
  data() {
    return {
      name: ""
    };
  }
};
</script>
```

`components/UserForm.vue`

```
<template>
  <div>
    <input v-model="name" placeholder="user">
  </div>
</template><script>
export default {
  name: "UserForm",
  data() {
    return {
      name: ""
    };
  }
};
</script>
```

`App.vue`

```
<template>
  <div id="app">
    <button @click="currentTab = 'user-form'">user form</button>
    <button @click="currentTab = 'student-form'">student form</button>
    <button @click="currentTab = 'employee-form'">employee form</button>
    <keep-alive>
      <component :is="currentTab"></component>
    </keep-alive>
  </div>
</template><script>
import UserForm from "./components/UserForm";
import StudentForm from "./components/StudentForm";
import EmployeeForm from "./components/EmployeeForm";export default {
  name: "App",
  components: {
    UserForm,
    StudentForm,
    EmployeeForm
  },
  data() {
    return {
      currentTab: "user-form"
    };
  }
};
</script>
```

在上面的代码中，我们有 3 个组件和 3 个表单，每个表单的输入都绑定到它们自己的模型。

然后在`App.vue`中，我们有了`keep-alive`组件，它包裹着`component`组件。

`keep-alive`组件让我们缓存组件的呈现实例，而不是创建一个新的组件实例并呈现它。

`component`组件使用设置为`is`属性的值的名称来呈现组件。

它会自动匹配烤肉串案件名称。这意味着我们为组件准备的 PascalCase 名称— `UserForm`、`StudentForm`和`EmployeeForm`将被自动映射到`user-form`、`student-form`和`employee-form`。

当我们点击按钮时，按钮的点击处理程序将`currentTab`的值设置为烤肉串盒组件名称。

现在，当我们在每个选项卡的输入框中输入内容时，我们会看到输入的值由于`keep-alive`组件的缓存而被保留。

如果我们删除了`keep-alive`组件，输入的值将不会被缓存，如果我们在不同的组件之间切换，它们将会消失。

如果我们添加如下的`activated`和`deactivated`挂钩:

`components/UserForm.vue`

```
<template>
  <div>
    <input v-model="name" placeholder="user">
  </div>
</template><script>
export default {
  name: "UserForm",
  data() {
    return {
      name: ""
    };
  },
  activated() {
    console.log("UserForm has been activated");
  },
  deactivated() {
    console.log("UserForm has been deactivated");
  }
};
</script>
```

`component/StudentForm.vue`

```
<template>
  <div>
    <input v-model="name" placeholder="student">
  </div>
</template><script>
export default {
  name: "StudentForm",
  data() {
    return {
      name: ""
    };
  },
  activated() {
    console.log("StudentForm has been activated");
  },
  deactivated() {
    console.log("StudentForm has been deactivated");
  }
};
</script>
```

`component/EmployeeForm.vue`

```
<template>
  <div>
    <input v-model="name" placeholder="employee">
  </div>
</template><script>
export default {
  name: "EmployeeForm",
  data() {
    return {
      name: ""
    };
  },
  activated() {
    console.log("EmployeeForm has been activated");
  },
  deactivated() {
    console.log("EmployeeForm has been deactivated");
  }
};
</script>
```

然后，当我们点击`App.vue`中的按钮在表单之间切换时，我们将看到控制台日志输出记录在控制台中。

![](img/0986f8d3bdf8b0dadeee6204f9566df2.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

当我们在不同组件之间动态切换时，Vue `keep-alive`组件允许我们通过缓存来保存当前组件的状态。

这也使得渲染速度更快，因为它们是缓存的。

## **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io)上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**