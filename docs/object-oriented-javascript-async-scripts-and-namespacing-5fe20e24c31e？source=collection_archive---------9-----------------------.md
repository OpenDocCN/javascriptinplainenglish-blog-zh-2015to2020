# 面向对象的 JavaScript——异步脚本和命名空间

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-async-scripts-and-namespacing-5fe20e24c31e?source=collection_archive---------9----------------------->

![](img/e0ceb04f04abd4f42d12a5c87c6146cf.png)

Photo by [James Besser](https://unsplash.com/@jcbesser?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究一些基本的编码和设计模式。

# 异步 JavaScript 加载

默认情况下，脚本标签是同步加载的。

这意味着它们按照在代码中添加的顺序加载。

因此，我们可以将脚本标签移到页面底部，这样我们就可以先看到 HTML 内容。

HTML5 有`defer`属性让我们异步加载脚本，但是是按照它们被添加的顺序。

例如，我们可以写:

```
<script defer src="script.js"></script>
```

我们只是在脚本的底部添加了`defer`属性。

旧的浏览器不支持这一点，但是我们可以通过用 JavaScript 加载我们的脚本来做同样的事情。

例如，我们可以写:

```
<script>
  (function() {
    const s = document.createElement('script');
    s.src = 'script.js';
    document.getElementsByTagName('head')[0].appendChild(s);
  }());</script>
```

我们创建一个脚本元素，将`src`值设置为脚本路径，并将其作为`head`标签的子标签。

# 名称空间

我们可以将一个对象添加到应用程序的名称空间中。

这样，我们的应用程序中就不会有这么多全局变量。

例如，我们可以写:

```
let APP = APP || {};
```

创建一个`MYAPP`名称空间。

然后，我们可以通过编写以下内容向其添加属性:

```
APP.event = {};
```

然后我们可以向`event`属性添加属性:

```
APP.event = {
  addListener(el, type, fn) {
    // .. 
  },
  removeListener(el, type, fn) {
    // ...
  },
  getEvent(e) {
    // ...
  }
  // ... 
};
```

# 命名空间构造函数

我们可以将构造函数放在命名空间对象中。

例如，我们可以写:

```
APP.dom = {};
APP.dom.Element = function(type, properties) {
  const tmp = document.createElement(type);
  //...
  return tmp;
};
```

我们有`Element`构造函数来创建一些对象。

# 命名空间()方法

我们可以创建一个`namespace`方法来动态创建我们的名称空间。

例如，我们可以写:

```
APP.namespace = (name) => {
  const parts = name.split('.');
  let current = APP;
  for (const part of parts) {
    if (!current[part]) {
      current[part] = {};
    }
    current = current[part];
  }
};
```

我们创建了一个`namespace`方法，它采用由点分隔的单词组成的字符串。

然后我们通过循环将属性添加到`current`对象中。

一旦我们定义了它，我们就可以通过编写来使用它:

```
APP.namespace('foo.bar.baz');console.log(APP);
```

我们在控制台日志中看到了`foo.bar.baz`属性。

# 初始化时分支

不同的浏览器可能具有实现相同功能的不同功能。

我们可以检查它们，并通过检查哪个存在来添加我们想要的功能，然后将存在的功能添加到我们的名称空间对象中。

例如，我们可以写:

```
let APP = {};if (window.addEventListener) {
  APP.event.addListener = function(el, type, fn) {
    el.addEventListener(type, fn, false);
  };
  APP.event.removeListener = function(el, type, fn) {
    el.removeEventListener(type, fn, false);
  };
} else if (document.attachEvent) {
  APP.event.addListener = function(el, type, fn) {
    el.attachEvent(`on${type}`, fn);
  };
  APP.event.removeListener = function(el, type, fn) {
    el.detachEvent(`on${type}`, fn);
  };
} else {
  APP.event.addListener = function(el, type, fn) {
    el[`on${type}`] = fn;
  };
  APP.event.removeListener = function(el, type) {
    el[`on${type}`] = null;
  };
}
```

我们检查不同版本的`addEventListener`和`removeEventListener`以及它们的等价物，并将它们放入我们的名称空间中。

这将所有的特性检测代码放在一个地方，这样我们就不必重复那么多次。

![](img/8437d2c405537173f0364d98dfcdf429.png)

Photo by [Vincentiu Solomon](https://unsplash.com/@vincentiu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以创建名称空间对象来使我们的代码远离全局名称空间。

JavaScript 脚本可以异步加载。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**