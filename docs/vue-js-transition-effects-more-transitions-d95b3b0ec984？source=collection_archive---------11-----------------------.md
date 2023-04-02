# Vue.js 转场效果-更多转场

> 原文：<https://javascript.plainenglish.io/vue-js-transition-effects-more-transitions-d95b3b0ec984?source=collection_archive---------11----------------------->

![](img/e83d968688f1a2a5687f9d9ef11fc69c.png)

Photo by [Cristina Gottardi](https://unsplash.com/@cristina_gottardi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看如何使用定制的过渡类，并使用 JavaScript 来创建过渡和动画。

# 自定义过渡类

我们可以通过为转换的不同阶段提供自己的类名来指定定制的转换类。

为此，我们可以添加以下属性:

*   `enter-class`
*   `enter-active-class`
*   `enter-to-class` —自 Vue 2.1.8 以来
*   `leave-class`
*   `leave-active-class`
*   `leave-to-class` —从 Vue 2.1.8 开始

这对于使用现有的 CSS 动画库(如 Animate.css)非常有用。

我们可以在代码中使用 Animate.css 类进行转换，如下所示:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: { show: false }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
    <link
      href="[https://cdn.jsdelivr.net/npm/animate.css@3.5.1](https://cdn.jsdelivr.net/npm/animate.css@3.5.1)"
      rel="stylesheet"
      type="text/css"
    />
  </head>
  <body>
    <div id="app">
      <button v-on:click="show = !show">
        Toggle
      </button>
      <transition
        name="bounce"
        enter-active-class="animated rubberBand"
        leave-active-class="animated bounceOutRight"
        enter-to-class="animated rubberBand"
      >
        <p v-if="show">hi</p>
      </transition>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们可以从`rubberBand`和`bounceOutRight`中看到反弹效果。

## 一起使用过渡和动画

Vue 需要连接事件监听器来知道转换何时结束。根据应用的 CSS 规则的类型，它可以是`transitionend`或`animationend`。

如果我们希望在同一个元素中既有动画又有过渡，那么我们必须在一个值为`animation`或`transition`的`type`属性中显式地指定它。

## 显式过渡持续时间

从 Vue 2.2.0 开始，我们可以指定过渡持续时间。

例如，我们可以用毫秒数来指定过渡持续时间:

```
<transition :duration="1000">...</transition>
```

或者，我们可以指定特定过渡阶段的持续时间，单位为毫秒:

```
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

## JavaScript 挂钩

我们可以监听在转换过程中发出的动画或转换事件。

使用它们是为了让我们可以在过渡的不同阶段操纵被激活的元素，以获得我们想要的过渡效果。

可以收听以下事件:

```
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"
  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  ...
</transition>
```

我们可以如下使用它们:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: { show: false },
  methods: {
    beforeEnter(el) {
      el.style.opacity = 0;
    },
    enter(el, done) {
      Velocity(el, { opacity: 1, fontSize: "2.5em" }, { duration: 300 });
      Velocity(el, { fontSize: "1em" }, { complete: done });
    },
    leave(el, done) {
      Velocity(el, { translateX: "25px", rotateZ: "50deg" }, { duration: 600 });
      Velocity(el, { rotateZ: "120deg" }, { loop: 2 });
      Velocity(
        el,
        {
          rotateZ: "75deg",
          translateY: "50px",
          translateX: "70px",
          opacity: 0
        },
        { complete: done }
      );
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
    <script src="[https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js](https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js)"></script>
  </head>
  <body>
    <div id="app">
      <button v-on:click="show = !show">
        Toggle
      </button>
      <transition
        name="fade"
        v-on:before-enter="beforeEnter"
        v-on:enter="enter"
        v-on:leave="leave"
      >
        <p v-if="show">hi</p>
      </transition>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

上面的代码应该会在字体过渡时改变我们的大小，当我们关闭文本时，它会在消失之前向上平移。

我们还改变了文本输入时的不透明度。

上面的代码使用速度库在过渡期间操作`p`元素。

添加`v-bind:css=”false”`也是一个好主意，这样可以跳过 CSS 检测。

![](img/839c10cad07b3fc48ebdc84cbb0a1910.png)

Photo by [Arnel Hasanovic](https://unsplash.com/@arnelhasanovic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 初始渲染时的过渡

我们可以添加`appear`属性来将过渡应用到节点的初始渲染。

例如，我们可以如下使用它:

`src/index.js`:

```
new Vue({
  el: "#app"
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
    <link
      href="[https://cdn.jsdelivr.net/npm/animate.css@3.5.1](https://cdn.jsdelivr.net/npm/animate.css@3.5.1)"
      rel="stylesheet"
      type="text/css"
    />
  </head>
  <body>
    <div id="app">
      <transition
        appear
        appear-to-class="animated bounceIn"
        appear-active-class="animated bounceOut"
      >
        <p>hi</p>
      </transition>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，当元素以`bounceIn`和`bounceOut`效果出现时，我们使用 Animate.css 的类来制作`p`元素的动画。

使用 JavaScript 挂钩，我们可以编写以下代码:

`src/index.js`:

```
new Vue({
  el: "#app",
  methods: {
    beforeAppear(el) {
      el.style.opacity = 0;
    },
    appear(el, done) {
      Velocity(el, { opacity: 1, fontSize: "2.5em" }, { duration: 300 });
      Velocity(el, { fontSize: "1em" }, { complete: done });
    },
    afterAppear(el, done) {
      Velocity(el, { translateX: "25px", rotateZ: "50deg" }, { duration: 600 });
      Velocity(el, { rotateZ: "120deg" }, { loop: 2 });
      Velocity(
        el,
        {
          rotateZ: "75deg",
          translateY: "50px",
          translateX: "70px",
          opacity: 0
        },
        { complete: done }
      );
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
    <script src="[https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js](https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js)"></script>
  </head>
  <body>
    <div id="app">
      <transition
        v-on:before-appear="beforeAppear"
        v-on:appear="appear"
        v-on:after-appear="afterAppear"
      >
        <p>hi</p>
      </transition>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们可以看到`hi`这个词在离开屏幕前被翻译和拉伸了一小段时间。

# 结论

使用 Vue，我们可以创建自定义的过渡类，使用我们想要的过渡类名称。

我们也可以使用 JavaScript 来实现过渡和动画，而不是使用 CSS。

最后，我们还可以为某些元素出现的情况添加过渡效果。

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。