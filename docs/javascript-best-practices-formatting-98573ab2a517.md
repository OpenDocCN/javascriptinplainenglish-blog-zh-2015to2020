# JavaScript 最佳实践—格式

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-formatting-98573ab2a517?source=collection_archive---------1----------------------->

![](img/b6e7de502b03f6c4903eea8128a1f7e6.png)

Photo by [Alex Munsell](https://unsplash.com/@alexmunsell?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将研究如何格式化文件源代码以提高可读性。

# 格式化

格式很重要，因为我们想让一切看起来都很好。

看起来不错的源代码可读性更强。

# 吊带

大括号应该用于所有的控制结构。

这包括块、循环和条件。

即使它们是可选的，我们仍然应该包括它，这样就可以清楚地知道块的开始和结束。

例如，不写:

```
if (condition) foo();
```

我们写道:

```
if (shortCondition()) {
  foo();
}
```

# 非空块

如果我们有非空的块，那么我们应该以特定的方式格式化它。

在打开大括号之前，我们应该没有换行符。

左大括号后应该有一个换行符。

右括号前的另一个换行符。

和右大括号后的换行符，如果 brave 终止了语句或函数体或类方法、类语句或类方法。

例如，我们应该写:

```
class Foo {
  constructor() {}

  method(foo) {
    //... 
  }
}
```

我们有用空行分隔的方法。

# 空块

如果我们有空块，那么大括号应该在同一行。

例如，我们可以写:

```
function nothing() {}
```

# 块缩进

缩进的 2 个空格是最佳间距。

它最大限度地减少了打字，而且很明显有缩进。

# 数组文字

数组文字可以有选择地格式化为类似块的结构。

例如，我们可以写:

```
const arr = [  
  1,
  2,
];
```

或者:

```
const arr = [1, 2];
```

# 对象文字

像数组文字一样，我们可以像块一样缩进对象文字的属性。

例如，我们可以写:

```
const foo = {
  a: 2,
  b: 1,
};
```

# 类别文字

我们可以编写内部块缩进的类文本。

方法后面不应该有分号。

如果我们创建一个子类，那么我们使用`extends`关键字。

例如，我们写道:

```
class Foo {
  constructor() {
    this.x = 1;
  }

  foo() {
    return this.x;
  }
}
```

把它分配给我们写的东西:

```
const Bar = class Foo {
  constructor() {
    this.x = 1;
  } foo() {
    return this.x;
  }
}
```

# 函数表达式

如果我们有函数表达式，那么主体应该比前一个缩进深度多缩进 2 个空格。

例如，我们写道:

```
function foo() {
  if (a1 === a2) {
    baz(a1);
  } else {
    bar(a2);
  }
}
```

或者:

```
function foo() {
  bar()
    .baz()
    .then((result) => {
      if (result) {
        result.use();
      }
    })
}
```

# Switch 语句

`switch`语句应缩进 2 个空格。

每个`case`声明也应该缩进两个空格。

例如，我们写道:

```
switch (val) {
  case 1:
    foo();
    break;

  case 2:
    bar();
    break;

  default:
    throw new Error('invalid value');
}
```

# 声明

我们应该考虑如何格式化语句以提高可读性。

# 每行一条语句

为了使它们更具可读性，我们应该每行有一个语句。

# 总是添加分号

每个语句都应该以分号结尾，这样 JavaScript 解释器就不会为我们添加分号。

这样，它们总是在我们想要的地方。

# 列限制

行长度应为 100 个字符或更少，以避免溢出页面。

我们不想水平滚动来阅读所有内容。

# 换行

我们应该对语句进行分段，以便每行都在列的限制范围内。

![](img/d72da01626e63bffcee44afcb74b1cd3.png)

Photo by [Joseph Gonzalez](https://unsplash.com/@miracletwentyone?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 换行

我们应该在更高的语法层次上换行。

例如，我们可以在函数调用、点符号和左括号后换行。

逗号应该附加在它前面的标记上。

例如，我们可以写:

```
estimate = calc(estimate + x *
    estimate) / 
  2;
```

我们也可以写:

```
const arr = [
  1,
  2
]
```

# 结论

我们应该使用换行符来将行长度控制在 100 个字符以内。

大括号的格式应该具有可读性。

块主体应该有两个缩进空间。

`switch`语句内部应该有缩进。

应该添加换行符来断开长行，也应该在函数调用、用于访问对象属性的点以及逗号之后添加换行符。

# **一封用简单英语写的信**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**