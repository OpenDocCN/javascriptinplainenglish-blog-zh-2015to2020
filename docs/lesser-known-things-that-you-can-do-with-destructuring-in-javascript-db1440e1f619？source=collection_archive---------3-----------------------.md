# 在 JavaScript 中使用析构可以做的鲜为人知的事情

> 原文：<https://javascript.plainenglish.io/lesser-known-things-that-you-can-do-with-destructuring-in-javascript-db1440e1f619?source=collection_archive---------3----------------------->

![](img/6d75e343f397c43474bc6335f6acb56e.png)

Photo by [Laurentiu Iordache](https://unsplash.com/@jordachelr?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/to-break?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

析构是一种 ES6 语法，用于将对象的属性或数组的值分配给变量。通过看起来类似于对象文字和数组，这种语法更加清晰，并且促进了变量的合理命名。您应该熟悉这样的作业:

```
const object = {
  x: 1,
};
const { x } = object;
x; // 1const array = [2, 3];
const [a, b] = array;
a; // 2
b; // 3
```

但是还有一些你可能还没有遇到的可能性。

# 获取数组的尾部

您可能不仅想访问数组的第一个元素，还想获得一个新的数组，其中这些元素已经被移除:尾部。您可以使用破坏来实现这一点:

```
const array = [1, 2, 3, 4, 5];
const [head, ...tail] = array;
head; // 1
tail; // [2, 3, 4, 5]
```

# 跳过数组中的元素

您并不总是想访问数组的第一个元素。即使您只需要第三和第五个元素，您也可以使用析构，只需跳过您不需要的元素:

```
const array = [1, 2, 3, 4, 5];
const [, , third, , fifth] = array;
third; // 3
fifth; // 5
```

# 嵌套破坏

想要访问对象的嵌套属性也不是不使用析构的理由。您可以嵌套破坏:

```
const object = {
  x: {
   y: 1,
  },
};const { x: { y } } = object;
y; // 1
x; // undefined
```

值得注意的是，中间值(【示例中的 )没有定义变量。

您甚至可以嵌套数组和对象的破坏:

```
const array = [{ x: 1 }];
const [{ x }] = array;
x; // 1
```

# 别名

将变量命名为您正在访问的属性通常是有意义的，但是您可能需要给它另一个名称。为此，您可以使用别名:

```
const object = {
  x: 1
};
const { x: newName } = object;
newName; // 1
x; // undefined
```

# 默认值

如果您试图访问的属性可以是未定义的，您可以给出一个默认值:

```
const array = [{ x: 1 }];
const [{ x, y = 2}] = array;
x; // 1
y; // 2
```

析构语法涵盖了更多您可能一开始就想到的情况。需要为变量使用一个新的名称，试图访问嵌套的值，可能是未定义的值或数组中的其他值，这些都不是不使用析构的借口。

# **简单明了的一句话**

你知道我们有四个出版物和一个 YouTube 频道吗？你可以在我们位于 [**的主页上找到这些——通过关注我们的出版物并订阅我们的 YouTube 频道**](https://plainenglish.io/) **来表达爱意！**