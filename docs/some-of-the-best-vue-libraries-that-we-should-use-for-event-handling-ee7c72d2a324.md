# 我们应该用于事件处理的一些最好的 Vue 库

> 原文：<https://javascript.plainenglish.io/some-of-the-best-vue-libraries-that-we-should-use-for-event-handling-ee7c72d2a324?source=collection_archive---------3----------------------->

![](img/40031cc4db3dc8b79b78f3fe84e885b2.png)

Photo by [Kazi Faiz Ahmed Jeem](https://unsplash.com/@djkazi8?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究一些我们应该用于事件处理的 Vue 库。

# vue 快捷键

`vue-shortkey`包让我们不费吹灰之力就能在应用程序中添加键盘快捷键功能。

我们所要做的就是运行以下命令来安装它:

```
npm install vue-shortkey --save
```

然后我们可以添加插件并如下使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";Vue.config.productionTip = false;
Vue.use(require("vue-shortkey"));new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`:

```
<template>
  <div id="app">
    <button v-shortkey="['ctrl', 'alt', 'o']" @shortkey="onKeyPress()">Open</button>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    onKeyPress() {
      alert('opened')
    }
  }
};
</script>
```

在上面的代码中，我们刚刚在`main.js`中注册了`vue-shortkey`插件。这使得我们可以在我们的应用程序中使用`v-shortkey`指令。

然后在`App.vue`中，我们在值为`[‘ctrl’, ‘alt’, ‘o’]`的按钮上使用了`v-shortkey`。这使得 Ctrl+Alt+O 键盘快捷键与单击“打开”按钮相同。

接下来，我们使用`vue-shortkey`附带的`@shortkey`处理程序来运行一个方法，在本例中是 if `onKeyPress`。

因此，当我们按 Ctrl+Alt+O 时，我们会看到一个显示“已打开”的警告框。

我们也可以定义键盘快捷键来处理多组按键。

例如，我们可以传入一个对象作为`v-shortkey`的值，然后如下处理每个按键情况:

```
<template>
  <div id="app">
    <button v-shortkey="{up: ['arrowup'], down: ['arrowdown']}" @shortkey="onKeyPress">Open</button>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    onKeyPress(event) {
      switch (event.srcKey) {
        case "up":
          alert("up");
          break;
        case "down":
          alert("down");
          break;
        default:
          break;
      }
    }
  }
};
</script>
```

在上面的代码中，我们有:

```
v-shortkey="{up: ['arrowup'], down: ['arrowdown']}"
```

它允许我们通过使用组合键名称作为键来定义组合键，以及我们的应用程序应该接受作为值的键盘快捷键。

然后在`methods`对象的`onKeyPress`方法中，我们获取一个`event`对象，如果我们在`@shortkey`处理程序中传递一个对`onKeyPress`方法的引用而不是调用它，这个对象将是可用的。

然后，我们通过指定想要运行的内容来处理在`onKeyPress`方法中指定的每个组合键。

在上面的例子中，当我们按向上箭头时，我们只显示一个带有“向上”字样的警告框，当我们按向下箭头时，我们只显示一个带有“向下”字样的警告框。

这个插件提供的另一个很棒的特性是，当我们按下一个组合键时，我们可以自动设置一个元素的焦点。

我们可以很容易地通过写:

```
<template>
  <div id="app">
    <button v-shortkey.focus="['ctrl', 'f']">Focus</button>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

在上面的代码中，我们用`focus`修饰符来指定当我们按下给定的组合键时，我们想要聚焦元素。在这种情况下，我们指定 Ctrl+F 将这样做。

当我们按下那个按钮时，我们将聚焦在聚焦按钮上。

![](img/d05ee14302c31ad2fcc092a807dc8a04.png)

Photo by [Ashes Sitoula](https://unsplash.com/@awesome?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 虚拟视点

我们可以使用`vue-waypoint`插件来运行基于元素位置的函数，基于当前的视口。

要安装，我们运行:

```
npm install vue-waypoint --save-dev
```

那么我们可以如下使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueWaypoint from "vue-waypoint";Vue.use(VueWaypoint);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <div >
      <p v-waypoint="{ active: true, callback: onWaypoint, options: intersectionOptions }" v-for="n of new Array(100).fill('').map((_, i) => i)" :key="n">{{n}}</p>
    </div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      intersectionOptions: {
        root: null,
        rootMargin: "0px 0px 0px 0px",
        threshold: [0, 1]
      }
    };
  },
  methods: {
    onWaypoint({ going, direction }) {
      if (going === this.$waypointMap.GOING_IN) {
        console.log("waypoint going in!");
      } if (direction === this.$waypointMap.DIRECTION_TOP) {
        console.log("waypoint going top!");
      }
    }
  }
};
</script>
```

在代码中，我们有一个 p 元素列表，显示从 0 到 100 的数字。我们补充道:

```
v-waypoint="{ active: true, callback: onWaypoint, options: intersectionOptions }"
```

这样当它们的位置改变时我们可以调用`onWaypoint`。

我们还改变了交集选项，这样当 p 元素是 0%和 100%可见时，我们运行`onWaypoint`方法，而我们的`onWaypoint`方法将被调用。

`intersectionOptions`对象与[交叉点观察器 API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) 中指定的选项对象相同。

在`onWaypoint`中，我们检查 p 元素是否进入视口，以及 p 元素的前进方向。

我们通过写入以下内容来记录 p 元素是否进入了视口:

```
if (going === this.$waypointMap.GOING_IN) {
  console.log("waypoint going in!");
}
```

我们记录 p 元素是否向视口顶部移动，方法是:

```
if (direction === this.$waypointMap.DIRECTION_TOP) {
  console.log("waypoint going top!");
}
```

当一个元素在某个方向或者在视口的某个特定位置时，当我们想要做一些事情时，这个插件是很方便的。

# 结论

`vue-shortkey`让我们在应用程序中轻松处理键盘快捷键。它还具有用于组合键上焦点元素的修饰符。

`vue-waypoint`插件让我们可以在某个元素处于某个位置或向某个方向移动时运行代码，而无需我们自己从头开始编写代码。

## **简明英语团队的说明**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****