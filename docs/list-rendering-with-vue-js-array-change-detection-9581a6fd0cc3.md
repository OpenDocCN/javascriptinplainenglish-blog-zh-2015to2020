# 使用 Vue.js 进行列表渲染—阵列变化检测

> 原文：<https://javascript.plainenglish.io/list-rendering-with-vue-js-array-change-detection-9581a6fd0cc3?source=collection_archive---------3----------------------->

![](img/dc66204e9036f8f0e124d90e5da73631.png)

Photo by [Jason Briscoe](https://unsplash.com/@jsnbrsc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用程序框架，我们可以用它来开发交互式前端应用程序。

在本文中，我们将了解 Vue 的阵列变化检测功能。

# 阵列变化检测

Vue 包装了下面的数组变异方法，因此当这些方法被调用时，视图更新将被触发。它们如下:

*   `push()`
*   `pop()`
*   `shift()`
*   `unshift()`
*   `splice()`
*   `sort()`
*   `reverse()`

例如，如果我们有一个按钮，可以触发一个新项目到一个数组，它会更新:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: [{ name: "Joe" }, { name: "Jane" }]
  },
  methods: {
    addPerson() {
      this.persons.push({ name: "Mary" });
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
  </head> <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="addPerson">Add Person</button>
      <div v-for="person in persons">
        {{person.name}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
JoeJane
```

首次呈现数组时。然后，当我们单击添加按钮时，我们会得到:

```
JoeJaneMary
```

因为我们通过点击按钮调用了`addPerson`方法，这就调用了`push`方法来添加一个新条目。

# 替换阵列

非变异数组方法总是返回一个新的数组。我们可以用返回的数组替换原始数组来触发视图更新。

当我们更新数组时，Vue 不会从头开始重新呈现列表。相反，它将尝试最大限度地重用 DOM 元素。

例如，如果我们有以下情况:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: [{ name: "Joe" }, { name: "Jane" }, { name: "Mary" }]
  },
  methods: {
    filterPerson() {
      this.persons = this.persons.filter(p => p.name !== "Joe");
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
  </head> <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="filterPerson">Filter Person</button>
      <div v-for="person in persons">
        {{person.name}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当我们的页面第一次加载时，我们得到以下名称:

```
JoeJaneMary
```

单击“筛选人员”后，我们将获得:

```
JaneMary
```

因为我们通过点击按钮来调用`filterPerson`方法，这就调用了`filter`方法来返回一个包含过滤项的数组。

然后新的数组被分配给`this.persons`并且视图被刷新。

![](img/f08a7bd47055e6f957cf50fea804762f.png)

Photo by [IB Wira Dyatmika](https://unsplash.com/@wiradyatmika?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 引起

Vue 无法通过设置索引或修改数组长度来检测直接将项目分配给数组。

所以:

```
vm.item[indexOfItem] = 'item';
```

以及:

```
vm.items.length = 2;
```

不会被 Vue 拾取并使用更新的数据更新视图。

我们可以调用`Vue.set`或`splice`为给定的索引设置一个条目。

## Vue.set

例如，我们可以编写以下代码:

`src/index.js`

```
new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Mary"]
  },
  methods: {
    addPerson() {
      Vue.set(this.persons, 5, "Jane");
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
  </head> <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="addPerson">Add Person</button>
      <div v-for="(person, index) in persons">
        {{index}} - {{person}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

当页面第一次加载时，我们得到以下呈现的条目:

```
0 - Joe1 - Mary
```

然后，当我们单击“添加人员”时，我们会得到:

```
0 - Joe1 - Mary2 -3 -4 -5 - Jane
```

## 接合

我们可以对`splice`做同样的操作，但是长度要先设置:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Mary"]
  },
  methods: {
    addPerson() {
      this.persons.length = 5;
      this.persons.splice(5, 1, "Jane");
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
  </head> <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="addPerson">Add Person</button>
      <div v-for="(person, index) in persons">
        {{index}} - {{person}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到和以前一样的结果。

我们还可以使用`splice`来设置数组的长度并触发视图更新。为此，我们可以写:

```
this.persons.splice(5);
```

## vm。$set

我们可以如下调用`vm.$set`，它和`Vue.set`一样，除了它只对 Vue 实例可用，而不是全局对象。

例如，我们可以写:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Mary"]
  },
  methods: {
    addPerson() {
      this.$set(this.persons, 5, "Jane");
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
  </head> <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="addPerson">Add Person</button>
      <div v-for="(person, index) in persons">
        {{index}} - {{person}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

# 结论

某些阵列更改会自动触发视图更新。

监视数组变异方法调用以更新视图。

返回新数组的数组方法必须与变异方法区别对待。返回值必须赋给原始变量才能触发视图更新。

当我们将一个条目分配给一个数组索引时，为了触发视图更新，我们可以使用`splice`、`Vue.set`或`this.$set` / `vm.$set.`

为了设置数组的长度并触发视图更新，我们还可以调用`splice`。