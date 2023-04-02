# 驯服长的和嵌套的 if/else 语句

> 原文：<https://javascript.plainenglish.io/taming-long-and-nested-if-else-statements-5a1e483dc777?source=collection_archive---------8----------------------->

## 回到基础——编写条件句

![](img/c36dfd4107654380d88c7fe19ffe2585.png)

Image by [Steve Buissinne](https://pixabay.com/users/stevepb-282134/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1580168) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1580168)

# 案例一。简单的 if / else if / else 块

```
**if** (mode === "A") {
  **return** { isDisabled: true };
} **else if** (mode === "B") {
  **return** { isDisabled: true };
} **else** {
  **return** { isDisabled: false };
}
```

这是我们在应用程序中常见的模式。然而，未来类似意大利面的更新时机已经成熟。让我们看看如何驯服它。

## 备选方案 1。丢掉别的

我们将尝试删除 else 关键字。事实上，这是多余的。

```
**if** (mode === "A") {
  **return** { isDisabled: true };
}
**if** (mode === "B") {
  **return** { isDisabled: true };
} 
**return** { isDisabled: false };
```

## 选项 2。使用 switch 语句

前面的解决方案不那么混乱，但是为什么不在这里使用 switch 语句呢？

```
**switch**(mode) {
  **case** "A":
    **return** { isDisabled: true };
  **case** "B":
    **return** { isDisabled: true };
  **default**:
    **return** { isDisabled: false };
}
```

如果和**开关**块的**具有大致相同的代码密度。Switch 语句因意外的 case 失败和被遗忘的缺省值而臭名昭著。**

## 选项 3。三元怎么样？

```
return mode === "A" 
  **?** { isDisabled: true }
  **:** mode === "B"
  **?** { isDisabled: true }
  **:** { isDisabled: false };
```

如果你不习惯这些术语，它们可能看起来很陌生，但是如果有适当的格式(使用更漂亮的)，读起来会容易得多，它们比命令式的 **if/else** 语句要少得多。然而，对象模式可能是这里最好的方法。

## 选项 4:对象模式

```
const status = {
  ["A"]: { isDisabled: true },
  ["B"]: { isDisabled: true },
}
return status[mode] || { isDisabled: false };
```

好多了！添加新案例应该和在对象中添加新地图一样简单。然而，我们不能用 object 模式替换嵌套表达式，让我们看另一个例子。

# 案例二。嵌套的 if else 块

```
**const** getMessage = (selected, mode, action) => {
  **if** (selected.length === 0) {
    **return** "Empty List";
  }
  **if** (selected.length > 1 **&&** action !== "edit") {
    **return** "Multi edit";
  }
  **if** (selected.accessible **&&** mode === "A") {
    **if** (action === "edit") {
      **return** "Multi select A Edit";
    }
    **return**  "Multi select A View";
  }
  **return** "No OP";
}
```

在代码评审期间，这种风格被认为是一种代码味道。随着应用程序的老化，这个无辜的小函数将随着更多的条件而成长为一个庞然大物。由于组合的数量众多，单元测试也很困难；就其本质而言，没有人会完全理解它来更新任何现有的测试。如果我们幸运的话，作者会包括一些评论。

我们不能在这里仅仅创建一个对象模型。ternaries 怎么样？

```
const getMessage = (selected, mode, action) => {
  **return** (selected.length === 0) {
    ? "Empty List"
    : (selected.length > 1 && action !== "edit") 
    ? "Multi edit"
    : (selected.accessible && mode === "A")
    ? (action === "edit")
      ? "Multi select A Edit"
      : "Multi select A View"
    : "No OP";
}
```

嵌套 ternaries 可能不那么罗嗦，但令人困惑，对我来说，它是一个不可读的代码块，破译它需要的精神体操不是微不足道的。代码应该尽可能地自我记录，以下是我的建议:

```
**const** emptyListMessage = (selected) =>
  selected.length === 0 ? "Empty List" : null;**const** multiSelectEditMessage = (selected, action) =>
  selected.length > 1 && action !== "edit" ? "Multi edit" : null;**const** multiSelectModeAMessage = (action) =>
  action === "edit" ? "Multi select A Edit" : "Multi select A View";**const** accessibleModeMessage = (selected, mode, action) => 
  selected.accessible && mode === "A"
    ? multiSelectModeAMessage(action) 
    : null;**const** defaultMessage = () => "No OP";**const** getMessage = (selected, mode, action) =>
  emptyListMessage(selected)
    || multiSelectEditMessage(selected, action)
    || accessibleModeMessage(selected, mode, action)
    || defaultMessage();
```

我们现在的代码主要是*函数和表达式，*函数式编程的精髓，它们具有数学性质；一旦经过适当的单元测试，它们往往会保持强大，并变得对错误有弹性。在这里，我们几乎已经将代码表面翻了一倍，它读起来相当不错。逻辑的每个部分都是自我记录的，这在条件复杂的情况下非常有用。我们可以**单元测试**这些更小的功能更容易，它们有时甚至会被重用。对我来说，这里最大的挑战是给它们命名，以便它们有效地传达意图。

**提示:**如果你有复杂的条件表达式，把它们移到自己的函数里。将这个应用到我们上面的例子中，我们得到了一个这样的函数，一个可爱的小 liner，可以简单地进行单元测试。

```
**const** isAccssibleModeA = (accessible, mode) => accessible && mode === “A”;
```

# 命令式 if/else 控制块

到目前为止，我们只看了纯函数/表达式，代码本身没有副作用，它们只是返回一些值而没有改变其他任何东西。然而，有些情况下，我们必须编写命令式嵌套逻辑控制块，这将包含某种副作用。

```
 **if** (condition1) {
    X();
  } **else if** (condition2) {
    **if** (condition3) {
      Y1();
    } **else** {
      Y2();
    }
  } **else** {
    Z();
  }
```

## **尝试 1**

我真的没有什么好的建议，但是重构的一个尝试是立即从一个 **if** 块中**返回**。

```
 **if** (condition1) {
    X();
    **return**;
  } **if** (condition2) {
    **if** (condition3) {
      Y1();
      **return**;
    }
    Y2();
    **return**;
  } Z();
```

它仍然不理想，因为我们不能在 if 块中逻辑地分离不同的代码块。它也容易出错，有可能忘记返回。对于这样的代码，我真的没有一个好的解决方案，但是，这里有另一个选择。

## 尝试 2

```
 **const** doX = () => {
    if (condition1) {
      X();
      **return true;**
    }
  }; **const** doY1 = () => { 
    if (condition3) {
      Y1();
      **return true;**
    }
  } **const** doY2 = () => {
      Y2();
      **return true;**
  }; **const** doY = () => {
    **if** (condition2)  {
      **return** doY1() || doY2();
    }
  }

  doX() || doY() || Z();
```

这样做的工作是将逻辑分成更小的功能。然而，解决方案是强制的，我们必须总是**返回** true/undefined 来指示它是否执行了某件事情，我们现在也有了一个未使用的表达式，一些 linters 肯定会抱怨它。它仍然不是单元可测试的，也不是更具可读性；我想我们只是让事情变得更糟了。我宁愿选择尝试 1。

## 最后的努力

```
 **const** doY = () => {
    **if** (condition3) {
      Y1();
      **return**;
    }
    Y2();
  } **if** (condition1) {
    X();
    **return**;
  } **if** (condition2) {
    doY();
    **return**;
  } Z();
```

当我们将嵌套的、缩进的块移出到另一个函数时，一点理智就出现了，这是我要停止的地方。有没有更好的方法驯服嵌套的 if/else 控制块？请在评论中让我知道。

你可以在这里试一下上面的代码:[https://codesandbox.io/s/refactor-if-else-8ws1x?file=/src/index.js:758-1226](https://codesandbox.io/s/refactor-if-else-8ws1x?file=/src/index.js:758-1226)

## 写给下一个人

将大的函数和逻辑表达式分割成小块看起来似乎是一项很大的工作，但是，下一个从事这项工作的人会感谢你的，尤其是当你用单元测试来陪伴他们的时候。当所有的场景都被恰当地描述出来时，测试有助于非常有效地记录代码。