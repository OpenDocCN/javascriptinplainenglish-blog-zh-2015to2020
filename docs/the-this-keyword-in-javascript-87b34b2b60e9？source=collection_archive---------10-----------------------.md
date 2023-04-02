# 有助于您理解 JavaScript 中“这个”关键词的 5 种场景

> 原文：<https://javascript.plainenglish.io/the-this-keyword-in-javascript-87b34b2b60e9?source=collection_archive---------10----------------------->

## 五种不同的场景来了解“这”是什么意思。

![](img/f2f63a90f8633cce1c75ba25404ae773.png)

Photo by [Prateek Katyal](https://unsplash.com/@prateekkatyal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/you-got-this?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 最令人困惑的特征之一是`this`关键字。`this`虽然看起来令人生畏，但并不那么难。下面是**五个不同的场景**来最终解读`this`的神秘！

开始前:

*   您可以通过在浏览器的开发人员控制台中执行代码片段来继续。使用以下快捷方式打开您的 Chrome Developer Console***Mac:**Cmd+Option+J | |**Windows:**Ctrl+Shift+J。

# **JavaScript 中的“这个”是什么？**

我们可以将`this`定义为:*“正在执行当前函数的对象”*

这是什么意思？`this`的价值通常由**执行上下文**决定。**执行上下文**是执行 JavaScript 代码的环境**。知道 **JavaScript 运行时**跟踪这些执行上下文的**堆栈**是很重要的，位于该堆栈顶部的是当前正在执行的。**

**关键字`this`所指的对象在每次执行上下文改变时都会改变。**

**实际上`this`的值是由*如何执行*代码决定的。**

# ****1。全局对象****

**让我们看一个例子:**

```
function fn () {
  console.log(this);
}
fn(); //Window {…}
```

**默认情况下**执行上下文**为**全局上下文**。对于浏览器，全局上下文是`window`。因为我们从窗口上下文中调用函数`fn()`，所以`this`指的是窗口对象。**

# **2.声明的对象**

**当`this`在声明的对象内部使用时，`this`的值被设置为**最接近的父对象**，该方法被调用。**

**示例:**

```
let student = {
  name: 'John',
  introduction: function() {
    console.log("Hi, my name is " + this.name); 
  },
  secondStudent:{
  name: 'Theresa',
  introduction: function() {
    console.log("Hi, my name is " + this.name);
  }
 }
};
student.introduction();
student.secondStudent.introduction();//Hi, my name is John
//Hi, my name is Theresa
```

**调用`student.introduction()`时，`this`在`student`对象内，等于**最接近的父对象。****

**同时，当`student.secondStudent.introduction()`被调用时，在整个函数中，它被绑定到`secondStudent`对象，在这种情况下，是最接近的父对象。**

# **3.调用、应用、绑定**

**`call()`、`bind()`和`apply()`允许我们显式设置`this`的值。用这些方法，我们可以操纵`this`。**

**示例:**

```
const jordan = {
  name: 'Michael',
  shootingAccurany: 95,
  practice() {
    console.log(this.shootingAccurany = 100);
  } 
};const ebeling = {
  name: 'Gianmarco',
  shootingAccurany: 65,
};jordan.practice.call(ebeling);// 100
```

**我们想从我们的`jordan`对象借出方法`practice()`。我们可以用`call()`来做。当我们使用`jordan.practice.call(ebeling)`时，第一个参数是`this`应该绑定到什么。**

**在第一个参数之后`call()`可以有许多 *n 个*后续参数，这些参数被传递到我们正在调用的函数中。`apply()`的工作方式与`call()`完全相同，但是在第一个参数之后，它只能再有一个参数，并且始终是一个数组。**

**`bind()`的工作方式与`call()`和`apply()`非常相似，但这里的主要区别在于`bind()`并不立即调用函数，而是返回一个原始函数的副本，该函数的`this`关键字被设置为一个提供的值。**

# **4.“新”关键字**

**`new`关键字用于从构造函数创建一个对象。使用关键字`new`我们创建了一个新的对象，它绑定了`this`的值。**

**示例:**

```
function Tesla(model, color) {
  this.model = model;
  this.color = color;
}const modelX= new Tesla('X', 'white');console.log(modelX.model);
console.log(modelX.color);// X
// white
```

# **5.箭头功能**

**与普通函数不同，**箭头函数**不绑定`this`，而是在词汇上绑定。**

```
const object = {
  print: () => { console.log(this); }
};object.print()
//Window {…} const object2 = {
  print: function() {
    console.log (this);
  }
};object2.print()
//{print: ƒ}
```

# **结论**

1.  **`this`的值通常由**执行上下文决定。****
2.  **默认情况下，**执行上下文**就是**全局上下文**。**
3.  **通过`call()`、`bind()`和`apply()`，我们可以显式设置`this`的值。**
4.  **使用关键字`new`我们创建了一个新的对象，它绑定了`this`的值。**
5.  **箭头函数不绑定`this`，但是它被词汇绑定。**

**喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！****