# Vue 3 —计算属性和观察器

> 原文：<https://javascript.plainenglish.io/vue-3-computed-properties-and-watchers-7a3ddb16dcab?source=collection_archive---------6----------------------->

![](img/ef90aee82e205ec93154fc31aff37b6d.png)

Photo by [Baard Hansen](https://unsplash.com/@ibaard?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究 Vue 计算属性和观察器。

# 计算属性

我们可以添加计算属性来从现有属性中派生一些。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <input v-model="message" />
      <p>{{reversedMessage}}</p>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            message: ""
          };
        },
        computed: {
          reversedMessage() {
            return this.message
              .split("")
              .reverse()
              .join("");
          }
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们向在`data`中返回的对象添加了`message`属性。

然后我们将`computed`属性添加到传递给`createApp`的对象中。

方法在它的 getters 里面，所以我们返回从其他属性派生的东西。

该方法必须是同步的。

现在，当我们在输入中输入一些东西时，我们会看到它下面显示的是相反的版本。

# 计算缓存与方法

我们可以通过运行一个方法来反转字符串来达到相同的结果。

但是，如果我们使用常规方法返回派生数据，就不会基于它们的依赖关系对其进行缓存。

计算出的属性基于原始的反应依赖关系被缓存。

只要`this.message`不变，`reversedMessage`就不会跑。

但是，如果我们的依赖项在计算的属性中没有反应，那么这个方法就不会运行。

在这种情况下，我们必须使用方法使它们更新。

大概是这样的:

```
computed: {
  now() {
    return Date.now()
  }
}
```

不会更新。

# 计算设定值

在我们的计算属性中可以有 setters。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <input v-model="firstName" placeholder="first name" />
      <input v-model="lastName" placeholder="last name" />
      <p>{{fullName}}</p>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            firstName: "",
            lastName: ""
          };
        },
        computed: {
          fullName: {
            get() {
              return `${this.firstName} ${this.lastName}`;
            },
            set(newValue) {
              const names = newValue.split(" ");
              this.firstName = names[0];
              this.lastName = names[names.length - 1];
            }
          }
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们在用`data`返回的对象中有`firstName`和`lastName`属性。

然后我们可以通过返回合并成一个字符串的两个属性来为`fullName`属性创建 getter。

设置器将是`set`方法，它接受`newValue`然后将它拆分回它的各个部分。

我们根据从`get`返回的组合字符串设置`this.firstName`和`this.lastName`。

当`this.firstName`和`this.lastName`改变时，则`get`运行。

如果`get`运行，则`set`运行。

# 观察者

计算属性适用于大多数情况，但有时我们需要观察器为我们提供一种更通用的方法来观察数据变化。

例如，我们可以这样使用它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <input v-model="name" />
      <p>{{data}}</p>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            name: "",
            data: {}
          };
        },
        watch: {
          name(newName, oldName) {
            if (oldName !== newName) {
              this.getData(newName);
            }
          }
        },
        methods: {
          getData(newName) {
            fetch(`https://api.agify.io/?name=${newName}`)
              .then(res => res.json())
              .then(res => {
                this.data = res;
              });
          }
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们有一个`getData`方法，它接受一个`newName`参数并从一个 API 获取一些数据。

然后我们有我们的`watch`属性，它有一个带有观察器的对象。

方法的名称将与实例属性的名称相匹配。

观察器将一个旧值和一个新值作为参数。

我们可以在里面随心所欲地运行。

我们运行的代码是异步的，所以我们必须使用一个观察器，而不是计算属性。

![](img/1c552733153d1d8aa85aca5ab3ded06f.png)

Photo by [Félix Lam](https://unsplash.com/@feliixlam?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

计算属性和观察器让我们观察反应性数据变化，并让我们在它们变化时运行代码。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**