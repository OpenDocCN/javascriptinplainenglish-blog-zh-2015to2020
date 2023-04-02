# Vue 3 —动态转换和状态动画

> 原文：<https://javascript.plainenglish.io/vue-3-dynamic-transitions-and-state-animations-432c5073ec83?source=collection_archive---------16----------------------->

![](img/ce1119aed1a217810946220b98cb8a47.png)

Photo by [Mitchell Orr](https://unsplash.com/@mitchorr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究如何创建动态转换和状态变化动画。

# 动态转换

我们可以通过向`name`属性传递一个动态字符串来创建动态转换。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
  </head>
  <body>
    <div id="app">
      Fade in
      <input type="range" v-model="fadeDuration" min="0" :max="5000" /> <button @click="show = !show">
        toggle
      </button>
      <transition
        :css="false"
        @before-enter="beforeEnter"
        @enter="enter"
        @leave="leave"
      >
        <p v-if="show">hello</p>
      </transition>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            show: false,
            fadeDuration: 1000
          };
        },
        methods: {
          beforeEnter(el) {
            el.style.opacity = 0;
          },
          enter(el, done) {
            Velocity(
              el,
              { opacity: 1 },
              {
                duration: this.fadeDuration,
                complete: done
              }
            );
          },
          leave(el, done) {
            Velocity(
              el,
              { opacity: 0 },
              {
                duration: this.fadeDuration,
                complete: done
              }
            );
          }
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们添加了速度库来添加我们选择的过渡效果。

可以调整`fadeDuration`,这样我们就可以改变进入和离开效果的长度。

我们只是调用`Velocity`来改变我们想要的不透明度。

# 状态转换

Vue 可以动画显示状态变化。

它们包括数字和计算、显示的颜色、SVG 节点和其他属性。

要做到这一点，我们可以用手表来制作状态变化的动画。

例如，我们可以写下一个简单的状态转换:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.2.4/gsap.min.js"></script>
  </head>
  <body>
    <div id="app">
      <input v-model.number="number" type="number" step="20" />
      <p>{{ animatedNumber }}</p>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            number: 0,
            tweenedNumber: 0
          };
        },
        computed: {
          animatedNumber() {
            return this.tweenedNumber.toFixed(0);
          }
        },
        watch: {
          number(newValue) {
            gsap.to(this.$data, { duration: 1.5, tweenedNumber: newValue });
          }
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

当我们输入一个数字时，它就会被激活。

该动画不在`number`观察器中。

我们称格林索克的`to`方法为最终值制作动画。

`this.$data`有最终数字的值。

`tweenedNumber`是动画制作过程中显示的值。

`animatedNumber`取`tweenedNumber`值，返回并显示。

# 将转换组织成组件

我们可以将动画组织成它们自己的组件。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.2.4/gsap.min.js"></script>
  </head>
  <body>
    <div id="app">
      <input v-model.number="x" type="number" step="20" /> +
      <input v-model.number="y" type="number" step="20" /> = {{ result }}
      <p>
        <animated-integer :value="x"></animated-integer> +
        <animated-integer :value="y"></animated-integer> =
        <animated-integer :value="result"></animated-integer>
      </p>
    </div> <script>
      const app = Vue.createApp({
        data() {
          return {
            x: 400,
            y: 373
          };
        },
        computed: {
          result() {
            return this.x + this.y;
          }
        }
      }); app.component("animated-integer", {
        template: "<span>{{ fullValue }}</span>",
        props: {
          value: {
            type: Number,
            required: true
          }
        },
        data() {
          return {
            tweeningValue: 0
          };
        },
        computed: {
          fullValue() {
            return Math.floor(this.tweeningValue);
          }
        },
        methods: {
          tween(newValue, oldValue) {
            gsap.to(this.$data, {
              duration: 0.5,
              tweeningValue: newValue,
              ease: "sine"
            });
          }
        },
        watch: {
          value(newValue, oldValue) {
            this.tween(newValue, oldValue);
          }
        },
        mounted() {
          this.tween(this.value, 0);
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了`animated-integer`组件来动画化我们的数字。

它仍然动画显示数字，但是我们有一个`tween`方法来创建动画，这样我们可以简化我们的`value`观察器方法。

`fullValue`是一个计算属性，用于将`this.tweeningValue`向下舍入到最接近的整数。

`this.tweeningValue`是将在动画过程中显示的值。

所以当我们输入数字时，我们会看到所有的数字都是动态的。

![](img/30535c96199e9275d0ddabeb14472fc0.png)

Photo by [Ryan Wallace](https://unsplash.com/@accrualbowtie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

观察器的一个很好的用途是将状态变化制成动画。

我们通过格林斯托克图书馆简化了这一过程。

此外，我们可以随时更改过渡的设置。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**