# NgClass 与 Class 绑定的角度比较

> 原文：<https://javascript.plainenglish.io/ngclass-vs-class-binding-in-angular-e748d71471b1?source=collection_archive---------9----------------------->

![](img/214eb6452337486c84eba3dd0afd5de2.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像数据一样，我们可以将类绑定到角度元素。为了在任何元素上有条件地设置某种样式，我们可以简单地在一个类选择器上编写 CSS 样式，并且我们可以有条件地将该 CSS 类添加到任何元素中。

# 类绑定

为了从元素中动态添加或移除 CSS 类，我们可以使用类绑定。语法是:

```
[classs.className] = "boolean expression"
```

如果给定的布尔表达式是 **truthy** 那么将添加所提供的类，或者如果它是 **falsy** 那么它将从元素中移除。例如:

```
<div [class.error]="hasError"> </div><div [class.adult]="age >= 18 ? true : false"> </div>
```

使用类绑定，我们只能有条件地添加单个类。但是要设置多个类，我们可以使用`ngClass`指令。

# ngClass 指令

为了添加多个类，我们可以传递一个对象，其中键是类名，值是相应的布尔表达式。

```
<div [ngClass]="{'error': hasError, 'warning': hasWarning}"> </div>
```

如果布尔表达式为 **truthy** ，则相应的类将被添加到元素中。

如果我们有更复杂的对象传递给`ngClass`，我们也可以在组件中定义对象，并在模板中使用该对象。

## **TS**

```
classes = {
    'hasValue': this.password.length > 0,
    'valid': this.password.length >= 6,
    'invalid': this.password.length < 6,
}
```

## **HTML**

```
<div [ngClass]="classes"> </div>
```

我们也可以将字符串和数组值传递给`ngClass`。

```
<div [ngClass]="'box'"> </div><div [ngClass]="['box', 'card', 'container']"> </div>
```

现在，您已经准备好向 HTML 元素中添加动态类，并使用任何适合您的绑定。

# 有关系的

[角部样式与样式绑定](https://jscurious.com/ngstyle-vs-style-binding-in-angular/)

*感谢您抽出时间* ☺️
更多的网络开发博客，请访问[jscurious.com](http://jscurious.com/)