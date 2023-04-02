# 简街编码面试问题

> 原文：<https://javascript.plainenglish.io/jane-street-coding-interview-questions-5265efff7372?source=collection_archive---------2----------------------->

## 通过每天解决一个问题，变得非常擅长编写面试代码

![](img/a744f4f1755197ccdfebcb6eaab768d6.png)

# 日常编码问题

它们是受真实编程面试启发的各种各样的问题，带有深入的解决方案，清晰地带您了解每个核心概念。

> 通过每天解决一个问题，变得格外擅长编写面试代码。

我们将一起使用 JavaScript 解决这些问题。

# 问题#1

## 问题

级别:中等

`cons(a, b)`构造一个对，`car(pair)`和`cdr(pair)`返回该对的第一个和最后一个元素。

例如:

```
car(cons(3, 4)) returns 3, and cdr(cons(3, 4)) returns 4.
```

给定`cons`的这个实现。

```
def cons(a, b):
    def pair(f):
        return f(a, b)
    return pair
```

执行`car`和`cdr`。

## 解决办法

首先，我们实现`cons`函数。

```
const cons = (a, b) => {
  const pair = (f) => {
    return f(a,b);
  }
  return pair;
}
```

接下来，我们实现`car`函数。如问题所述，`car`需要返回该对中的第一个元素。

```
const car = (pair) => {
  const f = (a, b) => { return a; }
  return pair(f);
}
```

然后，我们实现`cdr`函数。`cdr`需要返回该对的第二个元素。

```
const cdr = (pair) => {
  const f = (a, b) => { return b; }
  return pair(f);  
}
```

## 测试

最后，我们尝试测试我们的解决方案。让我们执行一些基本测试来确保这些实现正确工作。

```
console.log(car(cons(3, 4)))
**<< 3**console.log(cdr(cons(3, 4)))
**<< 4**
```

很简单，对吧？

我将更新简街在这篇文章中提出的新问题，请🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读😘，再见👋，别忘了👏最多 50 次并跟随！