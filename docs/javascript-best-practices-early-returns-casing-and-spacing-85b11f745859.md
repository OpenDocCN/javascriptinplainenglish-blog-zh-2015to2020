# JavaScript 最佳实践—早期返回、大小写和间距

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-early-returns-casing-and-spacing-85b11f745859?source=collection_archive---------5----------------------->

![](img/d9106212004b55812c18cc1d374a6c71.png)

Photo by [Kai Wenzel](https://unsplash.com/@kai_wenzel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 回拨后返回

如果我们提前调用回调，我们应该给它加上一个`return`，这样我们就不会运行它下面的代码。

回调返回什么并不重要。

例如，不写:

```
const foo = (err, callback) => {
  if (err) {
    callback(err);
  }
  callback();
}
```

我们写道:

```
const foo = (err, callback) => {
  if (err) {
    return callback(err);
  }
  callback();
}
```

`return`结束了`foo`的执行，因此`if`之外的代码不会运行。

# 需要茶包

大多数标识符都使用驼峰式大小写。

变量名、属性名和函数名应该是驼色的。

例如，不写:

```
function do_work() {
  // ...
}
```

或者:

```
function foo({ no_camelcased }) {
  // ...
};
```

或者:

```
const { category_id = 1 } = query;
```

我们写道:

```
function doWork() {
  // ...
}
```

或者:

```
function foo({ camelCased }) {
  // ...
};
```

或者:

```
const { categoryId = 1 } = query;
```

# 注释首字母的大写

注释可以是句子或短句。

他们都很好。

我们并不总是需要写出完整的句子。

例如，我们可以写:

```
// Capitalized comment
```

或者:

```
/* eslint semi:off */
```

我们可以在评论里写任何东西。

# 使用`this`强制执行类方法

如果定义实例方法，就要引用`this`。

例如，我们可以写:

```
class A {
  constructor() {
    this.a = "foo";
  } print() {
    console.log(this.a);
  } sayHi() {
    console.log("hi");
  }
}
```

而不是:

```
class A {
  constructor() {
    this.a = "foo";
  } sayHi() {
    console.log("hi");
  }
}
```

如果不引用`this`，我们应该将它们设为静态:

```
class A {
  constructor() {
    this.a = "foo";
  } static sayHi() {
    console.log("hi");
  }
}
```

# 大括号样式

最常见的 JavaScript 括号样式如下:

```
if (foo) {
  bar();
} else {
  baz();
}
```

大括号和关键字在同一行。

# 尾随逗号

尾随逗号使移动对象条目更容易，并使差异更清晰。

所以我们可以写:

```
const obj = {
  bar: "foo",
  qux: "baz",
};
```

# 逗号周围的间距

逗号周围的空格使项目易于阅读。

例如，我们可以写:

```
const foo = 1, bar = 2;
```

或者:

```
const arr = [1, 2];
```

或者:

```
const obj = {"foo": "bar", "baz": "qur"};
```

而不是:

```
const foo = 1,bar = 2;
```

或者:

```
const arr = [1,2];
```

或者:

```
const obj = {"foo": "bar","baz": "qur"};
```

空格字符使阅读更容易。

# 千分位样式

我们在条目后加逗号。

例如，代替书写；

```
const foo = ["apples"
           , "oranges"];
```

或者:

```
const foo = 1
  , bar = 2;
```

或者:

```
const foo = 1
,
bar = 2;
```

我们写道:

```
const foo = 1,
  bar = 2;const foo = 1,
  bar = 2;const foo = ["apples",
  "oranges"
];const obj =  {
    "a": 1,
    "b": 2
  };
}
```

我们在项目后保留逗号。

# 限制圈复杂度

为了让我们的代码更容易理解，我们可以限制代码的路径。

例如，我们可以将路径的数量限制为 2。

我们不是写:

```
function a(x) {
  if (true) {
    return x; 
  } else if (false) {
    return x + 1;
  } else {
    return 2;
  }
}
```

我们写道:

```
function a(x) {
  if (true) {
    return x; 
  } else if (false) {
    return x + 1;
  }
}
```

我们减少了`if`语句中的分支数量。

# 计算属性内的空格

计算属性不需要空格。

例如，我们写道:

```
const a = "prop";
const x = obj[a];
```

或者:

```
const obj = {
  [a]: "value" 
};
```

而不是:

```
const a = "prop";
const x = obj[ a ];
```

或者:

```
const obj = {
  [a ]: "value" 
};
```

![](img/766f24ad1bd66d09f458429bb02f0947.png)

Photo by [Emmanuel Butrón Zapata](https://unsplash.com/@b12690251?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该有合适的空间。

此外，我们应该在函数的早期返回，以阻止它下面的代码运行。

如果我们有实例方法，我们应该在其中引用`this`。

评论可以有任意大小写。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到他们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**